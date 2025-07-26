# 前言

本文档是基于B战作者：[长鸽门徒](https://space.bilibili.com/46839902) 的补充文档，文中工具均由[长鸽门徒](https://space.bilibili.com/46839902)整理开发，教程以及网盘均取自[长鸽门徒](https://space.bilibili.com/46839902)的专栏，我仅进行了整理补充。

[https://www.bilibili.com/video/BV1n4421Q7tm](https://www.bilibili.com/video/BV1n4421Q7tm)

十分感谢[长鸽门徒](https://space.bilibili.com/46839902)让我这个饥荒小白玩到了热乎的服务器，非常非常感谢。

# 环境

- Windows Server 2022
- 有公网IPv4

**这里要说明很重要的一点，饥荒服务器仅支持运行在单个线程(核心)上，也就是说如果你用i7-12700甚至i5-12600都能很轻松的跑起饥荒服务端。但是如果你使用16核32线程的服务器处理器，那么你将获得非常糟糕的联机体验，仅能支持3-4人联机。**

**饥荒诞生于一个i3默秒全的时代，很老了，无解，如果你有大于4人的联机需求，请咬咬牙去淘宝买专用的服务器面板服或者用你的台式机开服。**

# UU加速器

由于Steam缺少Wlanapi.dll组件，所以UU无法正常启动，在个别地区，需要UU加速Steam才可以访问创意工坊，甚至无法访问Steam，所以在一切开始之前，请先搞定加速器。

1.**以管理员身份**启动PowerShell，分别输入下列命令两行命令

```
get-windowsfeature wireless*|Install-WindowsFeature #安装Wireless功能
restart-computer -force #强制重启电脑
```

# 下载清单

1. [网易UU加速器](https://uu.163.com/)（能加速Steam就行爱用哪个用哪个）
2. [Steam](https://store.steampowered.com/about/)

Steam内需要下载：

- Don't Starve Together Dedicated Server（工具）
- Don't Starve Mod Tools（工具）
- 饥荒联机版（游戏）

3. 鸽之力开服工具

- 下载地址：https://cgmt.lanzout.com/b04duzyda 密码:2333
- 如果链接挂了的话就去[长鸽门徒的个人空间](https://space.bilibili.com/46839902)找一下

4. 备用百度网盘内有教学及工具、历史文件

- 链接：https://pan.baidu.com/s/10nvu2JlG0cTa8HnnDDqHxA
- 提取码：2333

5. 微软常用运行库

- 4的百度网盘里有，或者[微软常用运行库合集(Visual C++)2024.11.07 - 423Down](https://www.423down.com/10835.html)

# 准备事项

## Token

去Klei官网申请一个你的服务器Token令牌，需要用购买游戏的Steam登录。

[https://accounts.klei.com/account/game/list](https://accounts.klei.com/account/game/list)

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733506133978-93d061a0-723c-4c72-8512-5879ab982a19.png)

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733506404277-fba80625-ab7f-465b-9461-11bed187d153.png)

如果什么都没有就创建一个，我们**只需要Token**，存档功能不需要。此处复制下这样的一串Token保留好就可以关闭网页了。

## MOD

在饥荒联机版（游戏）的创意工坊中，尽情订阅你需要游玩的Mod。

这一步可以给朋友去做，你专心开服。使用Steam创意工坊收藏夹可以一键订阅所有选好的Mod。

# 开服工具选项卡设置

## 主页

1. 关闭杀毒工具，打开鸽之力。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733506586800-18470511-52a1-46db-bb57-96c0a469a86a.png)

- 第一行填写Don't Starve Together Dedicated Server路径，例如：

`C:\Program Files (x86)\Steam\steamapps\common\Don't Starve Together Dedicated Server`

- 第二行填写我们刚刚生成好的[Token](https://www.yuque.com/yosugo/gphwc5/ludgoafg568eu3cx#FzM4h)。

2. 点击令牌输入栏下方的快速生成存档

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733506789450-22d06df4-e7e4-42e2-8ae3-ea5b2cda12f0.png)

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733507032527-46fcc966-1708-4293-847b-30e5211f0881.png)

此处仅需自定义红圈内容即可，其他设置相同。完成后点击下方生成存档。

此时在界面中央便会出现我们创建的存档，点击存档名前方的方框，左侧便会出现存档基本信息。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733507188376-5bb5b4db-31b0-4e0a-820b-147e835af3ec.png)

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733507259480-c09bd912-794e-4f7d-9f13-679f6a1f6539.png)

屏幕右侧我们按需勾选随后点击保存基础设置。

## 世界启动项

1. 在这里勾选是否开启地下世界，以及显示服务器将会用到的端口号。

你只需要关注 server_port（世界端口号） 不需要关注 master_server_port（主服务器端口） 和 authentication_port（认证端口） 把他们删掉都是没问题的

- **如果没有需求，不要闲着修改端口号。**
- **记录下来，稍后需要打开防火墙对端口放通。**

此处我们需要放通的就是10999和10998端口。

2. 确定地图模式后，点击保存世界启动项设置。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733507639042-b61f2a4f-3cc7-4050-ada6-4c9faded4a29.png)

## 世界设置

这里可以对世界生物、资源进行自定义，同样在修改后需要点击保存世界设置。如果没有什么要自定义的，此处跳过。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733507696068-ede07360-c99b-4a33-9b13-b6a12b8854ee.png)

## MOD管理

### 区别客户端MOD及服务器MOD

MOD管理会自动读取你的**饥荒联机版（游戏）创意工坊**，可以点击刷新查看数量是否对得上，个别MOD为本地端使用，服务器无效（例如汉化），此处不会显示，所以数量对不上，但不会差的太离谱，举例：

我创意工坊有31个订阅，此处只显示24个，这是正常的，如果你订阅了几十个这里只有个位数，那就不对了。

解决办法是：

1. 重启开服工具、Steam
2. 在下方指定**饥荒联机版（游戏）路径**，点击保存路径。

例：C:\Program Files (x86)\Steam\steamapps\common\Don't Starve Together

---

1. 此处我们打开界面正常会显示一排![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733508131897-e8c0ccd6-0481-482a-9bea-17a4a46f1be2.png)，这是对的，重要的是对数量，如果没问题，我们直接挨个点击![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733508152408-06eb9871-7ff4-4eb6-874e-5c04f886b119.png)，启用MOD。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733508178781-d574fb6e-febd-4a8b-8e3d-b1baff9cc090.png)

双击其中一个MOD后，可以在右侧对其功能设定进行修改，每次更改都需要点击保存MOD设置，否则切换就没。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733508243947-f558ff43-2398-472f-a0d8-13887b799c30.png)

2. 在全部准备妥当后，可以点击页面中间最下方的**自检MOD**跑一下看看能不能通过。全都没问题了点击右下角的**保存MOD设置**。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733508314567-116e1e77-32f8-49cf-bd3f-1b9376cf9083.png)

# 防火墙

前面我们在**开服工具选项卡设置==>世界启动项**中提到了端口，现在需要对其中的端口进行放通。

## 自搭建家庭路由设置

这里我们讲一下端口转发，以OpenWrt为例。

在**网络==>防火墙==>端口转发**中，我们添加若干条，此处仅以默认10999为例。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733508675409-03b20704-d9e3-4d87-a49b-46c4f69664ad.png)

至于源区域和目标区域，按照我图设就可以。

随后**保存**，再点击**保存与应用**，其余再来几遍放通其他需要的端口。

## 云服务器

按照上面来即可，云服务器不需要转发，直接放通需要的端口。

---

如果你实在小白，且服务器规模小打小闹的话，遇事不决全部放开，只要你守护住你的Windows登录账号密码。

- **不要使用默认Administrator用户账号**
- **使用强密码**

FhaJK#H5Bg*Qy&A7 这就叫强密码

|   |   |   |
|---|---|---|
||Detail|意味（危）|
|IP|`0.0.0.0/0`|放开全部IP|
|端口|`0-65535`|放开全部端口|

# 开启服务器

此时，我们做完了全部的准备，就可以回到**开服工具**的**主页**了。

点击右下角的开启服务器，耐心等待。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733509648600-6c619c50-695f-484f-a0f1-ef83425a2c1e.png)

  

游戏加载时间较长，**注意不要点击黑色的命令行窗口**，你的点击会导致程序**暂停**。

如果你不确定你有没有点过，是不是卡死了，注意程序标题是否出现了**选择**字样。

按下回车即可取消选择暂行继续运行。

# 自用Q&A

## MOD加载失败

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733544670605-602f5575-fae2-40f5-921c-5749aa7a986e.png)

这是因为自动下载模组功能失效，可以通过如下方式解决：

1. 打开**MOD管理页面**，选择下方的**克隆模组**。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733544789886-fd57b574-2d5c-4adb-abc8-bbe7bd07e5e0.png)

2. 回到主页，**取消勾选**右侧的**模组优先。**

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733544906241-8701d7bc-e8d4-4f71-a1d5-56edf0130afe.png)

3. 启动服务器，会自动加载克隆的MOD，无需调用下载。

## Windows当病毒给我开服工具删喽T_T

1. 按下开始按钮，输入Windows，打开Windows安全中心。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733545475676-03ba2b6a-8ece-4c91-beb6-46f096a38fa9.png)

2. 进入病毒和威胁防护

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733545548244-a9f70049-940d-4419-bb74-58f49b7499ff.png)

3. 进入保护历史记录

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733545589355-62cb91cf-1e6c-44cc-b87a-099d0c13efbb.png)

4. 找到被扫描到的威胁选项，注意核对受影响项目，在右下角选择还原/允许在设备上，点击**操作**。

（我已经调整过了，无法举例，自己领悟吧）

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733545675610-4737a9f2-acd1-4779-a3d4-8244183d3fc0.png)

可以关闭此功能，但过一段时间后会自动打开，如果不是每天都用的话，不必与其斗争。

## 游戏角色跑的飞快，时间加速了

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733545802884-e71005a4-37a8-4ba3-9728-06f6f586b098.png)

点名这个中文合集MOD，直接禁用。不知道为什么要加个移动速度set在这个mod里