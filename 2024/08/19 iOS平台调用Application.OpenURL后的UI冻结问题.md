1. 问题描述

   游戏里面有个社群功能，简单来说就是针对不同的语言或渠道的玩家展示不同的引流入口，点击入口会跳转到外部链接。

   简单使用`Application.OpenURL`接口来实现。

   测试发现在iOS手机上点击引流入口，打开浏览器后将浏览器放到后台，切回应用的前台，再次点击任一引流入口大概率应用会卡死。（补充：本人测试发现如果这时将应用切到后台再切回前台，应用的卡死会恢复，因此将此问题定义为Freeze）

   构建与测试环境：Unity：2018.4.36f1 iOS版本17.5.1（机型iPhone11、iPhone 13 Pro、iPhone14）均复现

   

2. 相关报错日志

   暂未发现明显报错，以下为可能有帮助的控制台输出信息

   ```
   Invalidating grant <invalid NS/CF object> failed
   ```

   ```
   trying to get apparently bogus texture handle 0
   ```

   ```
   Error acquiring assertion: <Error Domain=RBSServiceErrorDomain Code=1 "((target is not running or doesn't have entitlement com.apple.developer.web-browser-engine.rendering AND target is not running or doesn't have entitlement com.apple.developer.web-browser-engine.networking AND target is not running or doesn't have entitlement com.apple.developer.web-browser-engine.webcontent))" UserInfo={NSLocalizedFailureReason=((target is not running or doesn't have entitlement com.apple.developer.web-browser-engine.rendering AND target is not running or doesn't have entitlement com.apple.developer.web-browser-engine.networking AND target is not running or doesn't have entitlement com.apple.developer.web-browser-engine.webcontent))}>
   ```

   

3. 类似问题链接

​	https://issuetracker.unity3d.com/issues/ios-application-hangs-when-a-button-calls-to-application-dot-openurl

​	https://forums.developer.apple.com/forums/thread/727290



4. Unity已经在2021.3.39f1的版本将该问题修复，更新Note见下方

   https://unity.com/cn/releases/editor/whats-new/2021.3.39#notes

​	具体条目为：
- iOS: Fixed application hanging when rapidly queueing OpenURL requests by making the call non-blocking. ([UUM-70281](https://issuetracker.unity3d.com/issues/ios-application-hangs-when-a-button-calls-to-application-dot-openurl))