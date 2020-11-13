# Android RxTasks

[![](https://jitpack.io/v/Terenfear/rx-tasks.svg)](https://jitpack.io/#Terenfear/rx-tasks)
![](https://img.shields.io/github/license/ashdavies/rx-tasks.svg)

[![](https://img.shields.io/github/last-commit/terenfear/rx-tasks.svg)](https://github.com/Terenfear/rx-tasks/commits/master)

**Fork of the [ashdavies/rx-tasks](https://github.com/ashdavies/rx-tasks) - Simple and lightweight RxJava2 conversion for the [Google Tasks APIs](https://developers.google.com/android/guides/tasks)**

**Now for RxJava3!**

## The Tasks API
> Starting with Google Play services version 9.0.0, you can use a `Task` API and a number of methods that return `Task` or its subclasses. `Task` is an API that represents asynchronous method calls, similar to `PendingResult` in previous versions of Google Play Services.

## Usage
> A common method that returns a `Task` is `FirebaseAuth.signInAnonymously()`. It returns a `Task<AuthResult>` which means the task will return an `AuthResult` object when it succeeds.

For example the Firebase sign in API asynchronously returns an `AuthResult` which can be consumed via `toSingle` method as an extension of `Task<T>`.

If consuming from Java code, use `SingleTaskKt.toSingle<T>(Task<T>)` and `CompletableTaskKt.toCompletable(Task<*>)`.

It is currently only possible to create a `Completable` from a `Task<Void>`, as this honours the correct API behaviour and it is not the responsibility of this library to convert between reactive types.

```kotlin
FirebaseAuth
  .getInstance()
  .signInAnonymously()
  .toSingle()
  .subscribe { /* ... */ }
```

**Gradle**
```groovy
allprojects {
    repositories {
        maven { url 'https://jitpack.io' }
    }
}
```
```groovy
implementation 'com.github.Terenfear:rx-tasks:+'
```

## Threading
> Listeners attached to a thread are run on the application main (UI) thread by default. When attaching a listener, you can also specify an `Executor` that is used to schedule listeners.

You may pass an `Executor` to the `Task` extension functions to relay to the Tasks API, to specify that the `Task` listener will execute on the provided `Executor`.

```kotlin
FirebaseAuth
  .getInstance()
  .signInAnonymously()
  .toSingle(Executor { Thread(it).run() })
  .subscribe { /* ... */ }
```

## Activity-scoped listeners
> If you are listening for task results in an `Activity`, you may want to add activity-scoped listeners to the task. These listeners are removed during the onStop method of your Activity so that your listeners are not called when the Activity is no longer visible.

Additionally, you may pass an `Activity` to the `Task` extension function to relay to the Tasks API, which will result in the `Task` listener being scoped to the provided `Activity` and will be removed during `onStop`.

```kotlin
FirebaseAuth
  .getInstance()
  .signInAnonymously()
  .toSingle(activity)
  .subscribe { /* ... */ }
```
