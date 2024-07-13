问题：Bugly后台设备型号那一栏全部显示为fail

解决：查阅FAQ得知，最新版本默认不再获取手机型号，App开发者可在合规获取到手机型号设置给sdk。

```c#
// 通过UserStrategy设置
strategy.setDeviceModel("userdefinedModel");
```

```java
// 也可以通过CrashReport类设置，context和deviceModel不能为空（或空字符串）
CrashReport.setDeviceModel(Context context, String deviceModel);
```

举例

```java
 String deviceModel = Build.MODEL;
 if (deviceModel != null && !deviceModel.isEmpty()) {
     CrashReport.setDeviceModel(this, deviceModel);
 }
```