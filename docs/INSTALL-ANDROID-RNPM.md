# Android RNPM Installation

```shell
npm install git+https://git@github.com:transistorsoft/react-native-background-geolocation-android.git --save
```

#### With React Native 0.27+

```shell
react-native link react-native-background-geolocation-android
```

#### With older versions of React Native

You need [`rnpm`](https://github.com/rnpm/rnpm) (`npm install -g rnpm`)

```shell
rnpm link react-native-background-geolocation-android
```

## Gradle Configuration

RNPM does a nice job, but we need to do a bit of manual setup.

* In **`android/app/build.gradle`**

```diff
...
+repositories {
+    flatDir {
+        dirs "../../node_modules/react-native-background-geolocation-android/android/libs"
+    }
+}
dependencies {
+  compile(name: 'tslocationmanager', ext: 'aar')
}
```


## AndroidManifest.xml

```diff
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.transistorsoft.backgroundgeolocation.react">

  <application
    android:name=".MainApplication"
    android:allowBackup="true"
    android:label="@string/app_name"
    android:icon="@mipmap/ic_launcher"
    android:theme="@style/AppTheme">

    <!-- react-native-background-geolocation licence -->
+   <meta-data android:name="com.transistorsoft.locationmanager.license" android:value="YOUR_LICENCE_KEY_HERE" />
    .
    .
    .
  </application>
</manifest>

```


## Proguard Config
* In your `proguard-rules.pro` (`android/app/proguard-rules.pro`)

```proguard
# BackgroundGeolocation lib tslocationmanager.aar is *already* proguarded
-keep class com.transistorsoft.** { *; }
-dontwarn com.transistorsoft.**

-keep class com.google.**
-dontwarn com.google.**
-dontwarn org.apache.http.**
-dontwarn com.android.volley.toolbox.**

# BackgroundGeolocation (EventBus)
-keepclassmembers class * extends de.greenrobot.event.util.ThrowableFailureEvent {
    <init>(java.lang.Throwable);
}
-keepattributes *Annotation*
-keepclassmembers class ** {
    @org.greenrobot.eventbus.Subscribe <methods>;
}
-keep enum org.greenrobot.eventbus.ThreadMode { *; }
-keepclassmembers class * extends org.greenrobot.eventbus.util.ThrowableFailureEvent {
    <init>(java.lang.Throwable);
}

# logback
-keep class ch.qos.** { *; }
-keep class org.slf4j.** { *; }
-dontwarn ch.qos.logback.core.net.*
```

