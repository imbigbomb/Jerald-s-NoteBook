# 前情提要

在一些情况下需要安装官方渠道的O365/M365，以防安全问题。如果要以合规的形式从微软官方下载，是非常麻烦繁琐的，尤其在无法使用订阅的管理员账号的情况下。本文将介绍如何通过配置文件xml+setup.exe的情况下安装Office套件。

# 需要的网址

[https://config.office.com/](https://config.office.com/)
# 准备和配置

## 网页内

进入页面后，点击页面下方的“创建新的配置”的**创建**按钮。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733928075046-99cf1f01-aa44-493b-8a8c-c12c808b5b94.png)

接下来将进入配置文件界面，在这里，事无巨细，Office相关的一切提前设置均可在此处配置，请自行配置。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733928161396-c0d9c3ac-e24e-486d-83e4-9443e8990860.png)

### 安装方式

在这里重点讲述左侧的**安装**选项卡。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733928285233-9fbdfe43-6089-4f07-be57-e4967c1a2db7.png)

在这里，如果选择：第一项，**Office内容分发网络(CDN)**。那么代表会自动下载所需文件。

如果选择第二项，本地源，则是你本地已经有了下载好的CAB文件的话，在这里指明本地文件路径。
### 导出XML

#### 默认文件格式

在配置完所有内容后，点击右上角的**导出**，在弹出默认文件格式页中按需选择，默认选择第一项，点击确定。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733928596139-0259e5ce-0423-4989-b5a5-bde25ade589a.png)

#### 将配置导出到XML

在之后页面，我们先点击下方的**单击此处下载最新版本**，此时会弹出一个新的页面，打开后我们先不管，在此页面，我们接受许可协议、命名文件名后，先导出XML文件。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733928760482-9d753add-0afe-4714-ae48-f0d9460ccd15.png)

## Setup.exe

[https://www.microsoft.com/en-us/download/details.aspx?id=49117](https://www.microsoft.com/en-us/download/details.aspx?id=49117)

这是[上一步](https://www.yuque.com/yosugo/sa40im/vnw2y80fladxoz8a#oBsD9)弹出的网页，我们默认**英文English**即可，点击**Download**下载。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733929077354-1224324f-ae86-4f99-be54-eb2d4fae2bca.png)

下载的文件名为officedeploymenttool_<版本号>.exe，打开之后进程为The Microsoft Office 2016 Click-to-Run Administrator Tool，这里我们**不要被Office2016迷惑到**，这只是个Setup.exe文件。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733929102668-299d9e98-2bf0-4afd-bc09-3db6c3e7e827.png)![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733929213357-5606ba93-86c9-41c4-80a1-f5ac51332a70.png)

安装过程很简单，我们需要其中的Setup.exe，所以建议把**安装目录选择到一个你能找到的文件夹内**。完成后如图，包含一个默认的xml文件和Setup.exe。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733929309260-ea8df067-2b8c-4be5-b30b-fb7ec7f3cf93.png)

  

# 使用

将我们[配置好的xml文件](https://www.yuque.com/yosugo/sa40im/vnw2y80fladxoz8a#oBsD9)放到此文件，删除/替换掉默认的配置，**在地址栏输入cmd按下回车**，或者在开始菜单以管理员模式运行CMD并使用CD命令进入setup.exe根目录。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733930116560-353d09a0-a281-4ae9-9fd1-797bb6d69afa.png)

打开CMD，输入命令，回车即可安装。后面的configuration_1.xml请**替换成你的xml文件名**

```
setup.exe /download configuration_1.xml
```

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1733930245406-98746cba-67d1-4da1-8a4f-246d315d9f36.png)# XML文件长什么样子/原理

此处以一个随便配置的xml举例：

```
<Configuration ID="efb9328a-86b6-41cf-b936-a8d12a53750c">
  <Add OfficeClientEdition="64" Channel="Current">
    <Product ID="O365ProPlusRetail">
      <Language ID="en-us" />
      <ExcludeApp ID="Access" />
      <ExcludeApp ID="Groove" />
      <ExcludeApp ID="Lync" />
      <ExcludeApp ID="OneDrive" />
      <ExcludeApp ID="Publisher" />
      <ExcludeApp ID="Teams" />
      <ExcludeApp ID="Bing" />
    </Product>
  </Add>
  <Updates Enabled="TRUE" />
  <RemoveMSI />
  <AppSettings>
    <User Key="software\microsoft\office\16.0\common\portal" Name="disablesetpersonalsite" Value="1" Type="REG_DWORD" App="office16" Id="L_Disableuserfromsettingpersonalsiteasdefaultlocation" />
    <User Key="software\microsoft\office\16.0\excel\options" Name="autorecovertime" Value="10" Type="REG_DWORD" App="excel16" Id="L_AutoRecovertime" />
  </AppSettings>
  <Display Level="Full" AcceptEULA="TRUE" />
</Configuration>
```

其中包含先前配置好的信息以及功能设置等，运行setup.exe会读取xml中的配置，按照xml内的需求进行定制安装，听起来复杂，动手很简单。