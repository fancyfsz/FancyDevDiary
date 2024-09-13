1. 之前是在Windows下开发Unity游戏，使用Rider作为代码编辑器，加EmmyLua插件可以对Lua的代码逻辑打断点，一切都很丝滑。

2. 今年基本上都是在Mac下开发，发现EmmyLua打断点不好使了。会有一个报错：

   ```
   iOS: XLua.LuaException: error loading module 'emmy_core' from file 'emmy_core.dylib': dynamic libraries not enabled; check your Lua installation
   ```

   尝试过生成对应架构的`.a`, 没成功，后面不了了之了，最近开发新版月卡，语言选的是Lua，查问题只能各种打log，效率比较低。幸好组内的小伙伴阿文解决了Mac下用EmmyLua的问题，知乎文章如下：

​	https://zhuanlan.zhihu.com/p/719786212

3. 提交对应的文件后，Android打包报错。仔细看下打包日志，问题如下：

   ```
   Exception: Unknown CPU architecture  for library Assets/ThirdPart/XLua/Plugins/iOS/libxlua.a
   ```

小贴士：把Jenkins上的控制台输出已文本形式查看，滑动到最底部，再`Ctrl+A` ` Ctrl+C` ` Ctrl+V`保存一个文本文件，如Log.txt，再使用下面的脚本快速过滤报错项。

   ```shell
   # 保存为DetectBuildPackageError.sh 后面还可以再扩展
   file="$1"
   grep -E "^[^a-zA-Z]*Exception:" "$file"
   ```

```   shell
sh DetectBuildPackageError.sh Log.txt
```

 发现是`Assets/ThirdPart/XLua/Plugins/iOS/libxlua.a.meta`误提交了，之前Include Platforms勾选的是iOS，后面误改为Any Platfrom了。

4. 这里记录下今天遇到的一个报错：

```
StandaloneOSXUniversal : XLua.LuaException: [Emmy]address already in use
```

顾名思义，就是咱的端口被占用了。

我的配置如下：

```lua
package.cpath = package.cpath .. ';/Users/fancy/Library/Application Support/JetBrains/Rider2023.3/plugins/EmmyLua/debugger/emmy/mac/x64/?.dylib'
local dbg = require('emmy_core')
dbg.tcpListen('localhost', 9966)
```

即9966被占用了

可以检查下端口状态

```shell
#lsof -i :<port>
lsof -i :9966
```

得到结果为

```shell
COMMAND   PID  USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
Unity   49194 fancy  155u  IPv4 0x67380f631a803d0d      0t0  TCP *:odnsp (LISTEN)
```

可以得到进程号即PID为 49194 使用kill 命令杀掉它即可

```shell
kill 49194
```

