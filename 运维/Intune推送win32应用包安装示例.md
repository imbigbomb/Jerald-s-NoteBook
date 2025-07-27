
准备工具：
1. **[Microsoft-Win32-Content-Prep-Tool](https://github.com/microsoft/Microsoft-Win32-Content-Prep-Tool)** 下载仓库内的IntuneWinAppUtil.exe
2. 示例软件WinSCP https://winscp.net/download/WinSCP-6.5.3-Setup.exe/download

# 一、转换格式
以WinSCP软件安装包为例，将转换器.exe和软件安装包放到一起比较方便操作。
![[Pasted image 20250727150404.png]]
1. 在地址栏输入cmd回车。 ![[Pasted image 20250727150519.png]]
2. 输入命令，将WinSCP-6.5.3-Setup.exe转换为Intunewin格式。这里也可选双击运行IntuneWinAppUtil.exe根据命令提示框内提示操作。
```
IntuneWinAppUtil.exe -c C:\YourSourceFolder -s WinSCP-6.5.3-Setup.exe -o C:\OutputFolder
```
 3. 最终： ![[Pasted image 20250727150940.png]]

# 二、创建Apps
1. 在Intune管理中心中，点击左侧的Apps，在右侧选择Windows平台。 ![[Pasted image 20250727145702.png]]
2. 点击Create创建一个App，并在右侧选择App类型为Win32。![[Pasted image 20250727145959.png]]
3. 点击下方的Select，在下一页面点击Select app package file选择我们转换好的Intunewin文件。随后点击OK。![[Pasted image 20250727151307.png]]
4. App信息请按需填写，有星号\*的为必填。完成后点击Next。![[Pasted image 20250727151903.png]]
5. 填写安装命令和卸载命令，每个应用都会不同，以WinSCP示例: ```
```
# 安装命令 (静默的/S一定是大写的S)

WinSCP-6.5.3-Setup.exe /S

# 卸载命令

"C:\Program Files (x86)\WinSCP\unins000.exe" /VERYSILENT /NORESTART

```
6. 继续填写Program页，完成后点击Next。![[Pasted image 20250727153212.png]]
7. 