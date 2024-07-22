背景：

https://support.google.com/googleplay/android-developer/answer/11926878

自 2024 年 8 月 31 日起：

- 新应用和应用更新必须以 Android 14（API 级别 34）为目标平台，才能提交到 Google Play（Wear OS 应用和 Android TV 应用除外，此类应用必须以 Android 13 或更高版本为目标平台）。
- 现有应用必须以 Android 13（API 级别 33）或更高版本为目标平台，Google Play 才会在新用户的设备搭载的 Android OS 版本高于应用的目标 API 级别时，继续向这些用户提供此类应用。如果应用以 Android 12（API 级别 31）或更低版本（如果是 Wear OS 应用，则为 Android 10 或更低版本；如果是 TV OS 应用，则为 Android 11 或更低版本）为目标平台，则只能在搭载的 Android OS 版本不高于应用的目标 API 级别的设备上使用。

如果您需要更多时间来更新应用，可申请延期至 2024 年 11 月 1 日。今年晚些时候，您将能够在 Play 管理中心内找到应用的延期表单。



操作：

1. 将涉及到的`build.gradle`的`compileSdkVersion`和`targetSdkVersion`修改到`34`，进行打包。

打包报错：`aapt2 unexpected error during link`

报错参考文档：

[**https://github.com/facebook/facebook-android-sdk/issues/1223**](https://github.com/facebook/facebook-android-sdk/issues/1223) （这个不改也没问题）

https://stackoverflow.com/questions/67233807/android-gradle-build-error-aapt2-aapt2-4-1-0-6503028-windows-daemon-0-unexpec

https://stackoverflow.com/questions/72898494/cordova-build-failed-with-aapt2-unknown-chunk-type-200

总的来说就是`AGP(Android Gradle Platform)`的版本需要升级。

修复：

修改前：

```groovy
classpath 'com.android.tools.build:gradle:4.2.2'
```
修改后：
```groovy
classpath 'com.android.tools.build:gradle:7.1.2'
```

升级完AGP后，原有的gradle版本也不够用了，同样需要升级

修改前：

```groovy
distributionUrl=https\://services.gradle.org/distributions/gradle-6.7.1-bin.zip
```

修改后：

 ```groovy
 distributionUrl=https\://services.gradle.org/distributions/gradle-7.2-bin.zip
 ```

另外`android.enableR8`属性已废弃，需要从`gradle.properties`文件中移除。

