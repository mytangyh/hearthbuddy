# HearthBuddy版本整理

## 目前可用版本

### 贴吧折腾版（2021.04.01）

- 可以修改策略，需要修改策略，需要 `\Silverfish\cards` 卡牌数据来识别卡牌。
- 贴吧折腾版打开时会访问 Gitee 上的一个 [JSON 文件](https://gitee.com/yl-hb/check_hb/raw/master/ 20210401check.json)（已删库），内容是：`{"state": "0", "tips": "版本关停"}`
  ，控制能否进入和显示公告信息，可以用 Fiddler4 等工具把这个请求拦截并修改为：`{"state": "1", "tips": ""}` 即可启动。
- 使用贴吧折腾版启动器，可以不用每次抓包启动。

### 怒风版（202.03.27/2021.04.11）

- 怒风旧版提示下载更新，需要修改时间到 2021 年 4 月 25 日。
- 第一次使用方法：下载解压缩打开Hearthbuddy.exe，提示长时间没用过期，随便输入 `q` 确认，再次打开，解压缩的目录下有个 `HB机器码.txt` 打开，用 Keygen 生成注册码，DefaultBot 对战模式选自动，修改卡组名称点 start
- 怒风版 2021.03.27 改时间一样可以用，但怒风版 2021.04.17 不能用注册机的旧码
- 怒风版类似整合版，怒风版的策略不在 Routines 里，策略打包在程序里没法换，`Silverfish\cards` 下没有卡牌信息，留牌策略还在
- 怒风版 2021.03.27 ，基本可以在绳子出现前结束回合
- 怒风版 2021.03.27 ，打开后会有一个弹窗，目前来看改时间需要到3.27比较合适，但是还是会出现一个无伤大雅的弹窗，使用火绒或者其他弹窗拦截软件标记两次就可以了。
- 修改 AI 计算操作间隔，修改 Main 里 MsBetweenTicks 计算间隔到15或以下，InputEventMsDelay 操作间隔到30或以下

### 卡卡酒馆战旗版

需要 Keygen 生成注册码，配置-版本-游戏模式-玩家对战模式，修改游戏模式为战旗模式，点开始即可

### 云骋版

云骋版标明禁止传播，故不提供文件下载。
云骋版旧版本要改时间，被要求更新，新版本能用注册机的旧码

## Keygen

多版本 Keygen，支持生成的版本有：贴吧折腾版、云骋版、卡卡版和怒风版。
Keygen 启动需要修改时间到 2021 年 4 月 15 日。

## 关于修改时间

有的 Hearthbuddy 或 Keygen 会加 DNGuard 壳，导致过期无法运行的问题，可以通过修改时间来解决问题，Hearthbuddy 通常设置为版本的后一天。

如：

- Keygen 需要修改时间到 2021 年 4 月 15 日
- 贴吧版 2021.04.01 需要修改时间到 2021 年 4 月 2 日
- 怒风版 2021.03.27 需要修改时间到 2021 年 4 月 25 日

可以通过此批处理 `Launcher.cmd` 启动相关程序，注意**需要管理员权限运行**：

```bat
@echo off
set d=%date%
cd /d %~dp0
::此处通常设置为兄弟版本后一天
date 2021-4-25
::此处后面注意添加 %1 %2 用以传递自动开始的参数
start Hearthbuddy.exe %1 %2
ping 127.0.0.1 -n 5 >nul
date %d%
exit
```

## 关于策略

策略添加，删除复制粘贴 `Routines` 文件夹
插件在 `Plugins` 文件夹

- 小小不哭的故事的2021.1.1 Routines新
- 小小不哭的故事的2020.12.18 Routines旧

2022.2021.1.1 Routines新奥秘法会用非公平游戏和同时期的卡，DefaultBehavior选rush

留牌策略
留牌策略是在 `Routines\DefaultRoutine\Silverfish\behavior\` 要调整的策略文件夹_mulligan.txt

## Hearthbuddy 旧版本/脱壳版

### 贴吧折腾版（2021.01.09）

### 炉石兄弟修复版 2020.05.17

需要输入地址对应验证

### 奥秘法（2020.1.17）

### 整合版（2020.10[3.2.7601.15205]）

使用策略需要码

### maxiori 无壳分享版（2019.8.17）

没有DNGuard壳，可以反编译，需要更新，GitHub 搜索 maxioriv 即可找到

## 旧版本需要修改以正常使用

2021年4月炉石传说退环境更新经典模式，修改了模式选择，无法继续使用旧版本

旧版本运行「开始」就会闪退，总的来说有三个地方要改。

1. 卡组的信息，原来有个isWild字段表示卡组是否为狂野卡组，现在改成了一个枚举类型，有经典 狂野 标准三种值
2. 游戏模式定义，以前也是一个bool值表示是否为狂野，另外还有一个bool值表示是否为休闲模式。现在同样改成了一个枚举值，有经典 狂野 标准 休闲四个值。
3. 是卡牌信息对应的EntityBase类里有个 `GetSpellPower` 的函数没了，因为现在的法术强度有了不同属性，所以获取法术强度的函数也做了相应的变更