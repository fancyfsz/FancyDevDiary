今天新部署了一台打包机，安装了最新的Xcode15.4。

iOS包在Archive阶段报错如下：
>// UnityNotificationWrapper.m
Incompatible function pointer types assigning to 'AuthorizationRequestResponse' (aka 'void (\*)(struct iOSNotificationAuthorizationData \*)') from 'AUTHORIZATION_CALBACK \*' (aka 'void (\*)(struct iOSNotificationAuthorizationData)')


解决：在对应Target的Build Settings页签下搜索Flags

找到Apple Clang - Custom Compiler Flags Other C Flags

添加如下Flag

**-Wno-error=incompatible-function-pointer-types**

重新Archive，接着出现第二个报错

>// AppsFlyer+AppController.m 
Type specifier missing, defaults to 'int'; ISO C99 and later do not support implicit int

搜索到了对应的issue

https://github.com/AppsFlyerSDK/appsflyer-unity-plugin/issues/263

可见AF已经在其最新的SDK版本解决了该问题