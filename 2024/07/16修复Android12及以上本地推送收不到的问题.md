## 修复Android12及以上本地推送收不到的问题

修改前：

```java
final PendingIntent pendingIntent = PendingIntent.getBroadcast(androidContext, localNotification.code.hashCode(), intent, 0);
```

参考文档：https://www.flybuy.com/android-12-pendingintent-mutability-and-geofences

重点：Apps that target Android 12 (SDK31) must now [specify a mutability flag for any pending intents.](https://developer.android.com/about/versions/12/behavior-changes-12#pending-intent-mutability) 

即Android12及其以上对于任一的pending intent都需要设置一个mutability的标识位，否则pending event不会得到触发。

修改后:

```java
int pendingIntentFlags = PendingIntent.FLAG_UPDATE_CURRENT;
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.S) {
    pendingIntentFlags |= PendingIntent.FLAG_IMMUTABLE; // 使用适当的标志
}
final PendingIntent pendingIntent = PendingIntent.getBroadcast(androidContext, localNotification.code.hashCode(), intent, pendingIntentFlags);
```



原有的本地推送代码针对Android6.0（Build.VERSION_CODES.M）以及以上还使用了精准闹钟的接口：

```java
AlarmManager am = (AlarmManager)androidActivity.getSystemService(Context.ALARM_SERVICE);
am.setExactAndAllowWhileIdle(AlarmManager.RTC_WAKEUP, notificationTime, pendingIntent);
```

参考文档：

https://stackoverflow.com/questions/71031091/android-12-using-schedule-exact-alarm-permission-to-get-show-data-at-specific-t

https://developer.android.com/about/versions/14/changes/schedule-exact-alarms
