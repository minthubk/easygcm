# easygcm

Android Library for easy GCM integration. Including push notifications sender task for Gradle.

This library is based on sample client code provided by Android documentation at http://developer.android.com/google/gcm/client.html and https://code.google.com/p/gcm/

It includes all the common code from the sample including AndroidManifest.xml that is automatically merged into your application. Also content of sample's DemoActivity is included in this library extracted in GcmHelper class. This library further improves original sample client in several ways, see [changelog](https://github.com/inloop/easygcm/blob/master/CHANGELOG.md).

### How to use:

* add this to your build.gradle: `compile 'eu.inloop:easygcm:1.3.0@aar'`
* in your `Application` implement [GcmListener](http://github.com/inloop/easygcm/blob/master/easygcm/src/main/java/eu/inloop/easygcm/GcmListener.java) interface with two methods: 
 * `onMessage()` (don't forget to call wakeLockRelease.release() after you are done)
 * `sendRegistrationIdToBackend()`
* in your MainActivity `onCreate()`, call `GcmHelper.init(this);`
* define `easygcm_sender_id` string resource and set it to your GCM Sender ID
* add manifest placeholder with your package name in your build config (see sample project): `manifestPlaceholders = [ localApplicationId:"your.package.here" ]` - this is temporary workaround for [this Android bug](https://code.google.com/p/android/issues/detail?id=156015)

If you need ```GET_ACCOUNTS``` permission, add this to you application's manifest:

    <uses-permission
        android:name="android.permission.GET_ACCOUNTS"
        tools:remove="android:maxSdkVersion"/>

That's it. You might also want to use Gradle task for sending push notifications to your device:

* add Gradle dependency to buildscript section: `classpath 'eu.inloop:easygcm-tasks:1.3.0'`
* edit push notification data, apiKey and registration ID in build.gradle
* call `./gradlew push`

### How to build locally:

So far, it is not possible to use project dependencies in Gradle's buildscript section (needed for our plugin). That means you have to build the library first (and install in local Maven repo) and just then build the sample application.

    ./gradlew clean uploadArchives
    ./gradlew -p easygcm-sample clean assembleDebug
