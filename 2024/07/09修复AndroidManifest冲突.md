升级af-android-sdk到6.14.2后，打Android包失败。

报错如下：

>  Attribute application@allowBackup value=(false) from [oaid_sdk_2.5.0.aar] AndroidManifest.xml:40:18-45
> 	is also present at [com.appsflyer:af-android-sdk:6.14.2] AndroidManifest.xml:35:9-35 value=(true).
> 	Suggestion: add 'tools:replace="android:allowBackup"' to <application> element at AndroidManifest.xml:7:5-190:19 to override.

解决：

App层级的AndroidManifest文件的Application元素下添加如下标签

```xml
android:allowBackup="true"
tools:replace="android:allowBackup"
```

