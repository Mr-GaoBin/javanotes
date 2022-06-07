# windows

## 快捷键

```
alt+f4									关闭当前窗体
Ctrl+W                                     是一个键盘组合快捷键，关闭当前打开的所有网页
```

## Dos命令

免重启刷新环境变量

```shell
打开cmd 输入 set path=test
输入 echo %path% 输出为test
```



```
shutdown             -s -t 时间            -- 指定时间后关机
shutdown             -r                    -- 重启
shutdown             -a                    -- 取消重启
```

桌面图标缓存清理

```shell
rem 关闭Windows外壳程序explorer
taskkill /f /im explorer.exe
rem 清理系统图标缓存数据库
attrib -h -s -r "%userprofile%\AppData\Local\IconCache.db"
del /f "%userprofile%\AppData\Local\IconCache.db"
attrib /s /d -h -s -r "%userprofile%\AppData\Local\Microsoft\Windows\Explorer\*"
del /f "%userprofile%\AppData\Local\Microsoft\Windows\Explorer\thumbcache_32.db"
del /f "%userprofile%\AppData\Local\Microsoft\Windows\Explorer\thumbcache_96.db"
del /f "%userprofile%\AppData\Local\Microsoft\Windows\Explorer\thumbcache_102.db"
del /f "%userprofile%\AppData\Local\Microsoft\Windows\Explorer\thumbcache_256.db"
del /f "%userprofile%\AppData\Local\Microsoft\Windows\Explorer\thumbcache_1024.db"
del /f "%userprofile%\AppData\Local\Microsoft\Windows\Explorer\thumbcache_idx.db"
del /f "%userprofile%\AppData\Local\Microsoft\Windows\Explorer\thumbcache_sr.db"
rem 清理 系统托盘记忆的图标
echo y|reg delete "HKEY_CLASSES_ROOT\Local Settings\Software\Microsoft\Windows\CurrentVersion\TrayNotify" /v IconStreams
echo y|reg delete "HKEY_CLASSES_ROOT\Local Settings\Software\Microsoft\Windows\CurrentVersion\TrayNotify" /v PastIconsStream
rem 重启Windows外壳程序explorer
start explorer
```

- DESKTOP.INI

  参考https://baike.baidu.com/item/DESKTOP.INI/11001208?fr=aladdin

```
新建文件夹.{20D04FE0-3AEA-1069-A2D8-08002B30309D}			-- 生成我的电脑图标
```

```
C:\Windows\System32\drivers\etc							    -- 修改本地host文件
mrt                                                           -- 恶意删除检测
%tepm%												     -- 缓存
recent												     -- 操作记录
regedit                          						   -- 注册表
ipconfig/ flushdns                                           -- 刷新 DNS 解析缓存
ipconfig/ displaydns							           -- 查看DNS缓存记录
netstat -aon|findstr 11026							       -- 产看指定端口 
taskkill /pid 9620 /f								       --关闭指定端口
get-executionpolicy									       --计算机执行策略
set-executionpolicy remotesigned                              -- 更改计算机执行策略
Get-ExecutionPolicy -List
```

## PowerShell 执行策略

这些策略的强制仅在 Windows 平台上发生。 PowerShell 执行策略如下所示：

### AllSigned

- 脚本可以运行。
- 要求所有脚本和配置文件都由受信任的发布者签名，包括在本地计算机上编写的脚本。
- 在从尚未归类为受信任或不受信任的发布者运行脚本之前，将提示您。
- 运行已签名但恶意脚本的风险。

### Bypass

- 不阻止任何操作，并且没有任何警告或提示。
- 此执行策略适用于以下配置：将 PowerShell 脚本内置于更大的应用程序或配置，其中 PowerShell 是具有其自己的安全模型的程序的基础。

### Default

- 设置默认的执行策略。
- **Restricted** 对于 Windows 客户端。
- 用于 Windows 服务器的 **RemoteSigned** 。

### RemoteSigned

- Windows server 计算机的默认执行策略。
- 脚本可以运行。
- 要求来自受信任的发布者的脚本和配置文件的数字签名，这些脚本和配置文件是从 internet 下载的，其中包括电子邮件和即时消息程序。
- 不需要在本地计算机上编写的脚本上的数字签名，也不需要从 internet 下载。
- 如果未对脚本进行阻止，则运行从 internet 下载的脚本，而不是未签名的脚本，例如通过使用 `Unblock-File` cmdlet。
- 从 internet 以外的源运行未签名脚本的风险，以及可能是恶意的签名脚本。

### Restricted

- Windows 客户端计算机的默认执行策略。
- 允许单独的命令，但不允许脚本。
- 阻止运行所有脚本文件，包括格式设置和配置文件 (`.ps1xml`) 、模块脚本文件 (`.psm1`) 和 PowerShell 配置文件 (`.ps1`) 。

### Undefined

- 当前作用域中没有设置执行策略。
- 如果所有作用域中的执行策略都是 **Undefined** ，则有效的执行策略 **Restricted** 适用于 Windows 服务器 Windows 客户端和 **RemoteSigned** 。

### Unrestricted

- 非 Windows 计算机的默认执行策略无法更改。
- 未签名的脚本可以运行。 存在运行恶意脚本的风险。
- 在运行不在本地 intranet 区域中的脚本和配置文件之前警告用户。

## 重装系统

```apl
1.msdn下载系统镜像【https://msdn.itellyou.cn/】

2.制作启动盘，将镜像拷贝至同级目录下

3.重启进入Bois界面,设置启动项启动盘优先级（安装完成需还原设置）
```

## wsl

```
```

