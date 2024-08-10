背景：

https://support.google.com/googleplay/android-developer/answer/11926878

自 2024 年 8 月 31 日起：

- 新应用和应用更新必须以 Android 14（API 级别 34）为目标平台，才能提交到 Google Play（Wear OS 应用和 Android TV 应用除外，此类应用必须以 Android 13 或更高版本为目标平台）。
- 现有应用必须以 Android 13（API 级别 33）或更高版本为目标平台，Google Play 才会在新用户的设备搭载的 Android OS 版本高于应用的目标 API 级别时，继续向这些用户提供此类应用。如果应用以 Android 12（API 级别 31）或更低版本（如果是 Wear OS 应用，则为 Android 10 或更低版本；如果是 TV OS 应用，则为 Android 11 或更低版本）为目标平台，则只能在搭载的 Android OS 版本不高于应用的目标 API 级别的设备上使用。

如果您需要更多时间来更新应用，可申请延期至 2024 年 11 月 1 日。今年晚些时候，您将能够在 Play 管理中心内找到应用的延期表单。



操作：

1. 将涉及到的`build.gradle`的`compileSdkVersion`和`targetSdkVersion`修改到`34`，进行打包。

打包报错：`aapt2 unexpected error during link`

```groovy
Execution failed for task ':app:processReleaseResources'.
> A failure occurred while executing com.android.build.gradle.internal.res.LinkApplicationAndroidResourcesTask$TaskAction
   > AAPT2 aapt2-4.2.2-7147631-osx Daemon #0: Unexpected error during link, attempting to stop daemon.
     This should not happen under normal circumstances, please file an issue if it does.
```

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



使用Android14手机进行测试，进游戏闪退：

```
java.lang.SecurityException: (这里是应用的bundle id): One of RECEIVER_EXPORTED or RECEIVER_NOT_EXPORTED should be specified when a receiver isn't being registered exclusively for system broadcasts
```

报错参考文档：

https://stackoverflow.com/questions/77235063/one-of-receiver-exported-or-receiver-not-exported-should-be-specified-when-a-rec

其中提到的一个关键点是：

> As discussed at Google I/O 2023, registering receivers with intention using the RECEIVER_EXPORTED / RECEIVER_NOT_EXPORTED flag was introduced as part of Android 13 and is now a requirement for apps running on Android 14 or higher (U+).

即对于运行在Android14以及以上的手机，registerReceiver需要写明使用RECEIVER_EXPORTED或者RECEIVER_NOT_EXPORTED的flag，否则系统就会抛出一个安全异常，对于没有进行try catch操作的代码来说，表现就是闪退了。

修改前：

```java
        registerReceiver(UpdateAndInstall.receiver, new IntentFilter(DownloadManager.ACTION_DOWNLOAD_COMPLETE));
```

修改后:

```java
    private void registerActionDownloadComplete() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) {
            registerActionDownloadCompleteNew();
        } else {
            registerActionDownloadCompleteOld();
        }
    }

    @RequiresApi(api = Build.VERSION_CODES.TIRAMISU)
    private void registerActionDownloadCompleteNew() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            registerReceiver(UpdateAndInstall.receiver, new IntentFilter(DownloadManager.ACTION_DOWNLOAD_COMPLETE), Context.RECEIVER_NOT_EXPORTED);
        }
    }

    private void registerActionDownloadCompleteOld() {
        registerReceiver(UpdateAndInstall.receiver, new IntentFilter(DownloadManager.ACTION_DOWNLOAD_COMPLETE));
    }

```

升级Android14上线之后，发现崩溃率上升2个点

主要崩溃如下：

```
Caused by java.lang.SecurityException
: One of RECEIVER_EXPORTED or RECEIVER_NOT_EXPORTED should be specified when a receiver isn't being registered exclusively for system broadcasts
          Caused by java.lang.SecurityException: com.more.lastfortress.gp: One of RECEIVER_EXPORTED or RECEIVER_NOT_EXPORTED should be specified when a receiver isn't being registered exclusively for system broadcasts
       at android.os.Parcel.createExceptionOrNull(Parcel.java:3087)
       at android.os.Parcel.createException(Parcel.java:3071)
       at android.os.Parcel.readException(Parcel.java:3054)
       at android.os.Parcel.readException(Parcel.java:2996)
       at android.app.IActivityManager$Stub$Proxy.registerReceiverWithFeature(IActivityManager.java:5684)
       at android.app.ContextImpl.registerReceiverInternal(ContextImpl.java:1868)
       at android.app.ContextImpl.registerReceiver(ContextImpl.java:1804)
       at android.app.ContextImpl.registerReceiver(ContextImpl.java:1792)
       at android.content.ContextWrapper.registerReceiver(ContextWrapper.java:765)
       at com.onevcat.uniwebview.UniWebViewFileDownloader.register(UniWebViewFileDownloader.kt:70)
       at com.onevcat.uniwebview.UniWebView.<init>(UniWebView.kt:68)
       at com.onevcat.uniwebview.UniWebViewContainer.<init>(UniWebViewContainer.kt:76)
       at com.onevcat.uniwebview.UniWebViewContainer.<init>(UniWebViewContainer.kt:28)
       at com.onevcat.uniwebview.UniWebViewInterface$Companion$init$1.invoke(UniWebViewInterface.kt:30)
       at com.onevcat.uniwebview.UniWebViewInterface$Companion$init$1.invoke(UniWebViewInterface.kt:28)
       at com.onevcat.uniwebview.UniWebViewInterface$Companion.runSafelyOnUiThread$lambda-0(UniWebViewInterface.kt:593)
       at com.onevcat.uniwebview.UniWebViewInterface$Companion.$r8$lambda$6__t4l1bsNDfkejCf664qhlG6vE()
       at com.onevcat.uniwebview.UniWebViewInterface$Companion$$ExternalSyntheticLambda1.run(:2)
       at android.os.Handler.handleCallback(Handler.java:958)
       at android.os.Handler.dispatchMessage(Handler.java:99)
       at android.os.Looper.loopOnce(Looper.java:255)
       at android.os.Looper.loop(Looper.java:364)
       at android.app.ActivityThread.main(ActivityThread.java:8938)
       at java.lang.reflect.Method.invoke(Method.java)
       at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:572)
       at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:1053)
        
Caused by android.os.RemoteException
```

跟进游戏闪退遇到的其实是同一个问题，即registerReceiver没有设置Flag。这回出问题的是 UniWebView插件，它涉及到的是游戏内跟内置网页相关的功能，没有测试到。

![UniWebView5.6.2](/Users/fancy/Documents/UniWebView5.6.2.png)

据官方文档，它已于5.6.2版本修复此问题，因此我们把该插件升级到这个版本及其以上即可解决。

反思：

可以利用例如  https://github.com/skylot/jadx  等工具，先静态检测一遍包体是否有registerReceiver没有适配Android14的情形（特别是没有加 TryCatch 的），有针对性的测试相关的功能，如有需要升级相关的SDK。

