
准备工具：
1. **[Microsoft-Win32-Content-Prep-Tool](https://github.com/microsoft/Microsoft-Win32-Content-Prep-Tool)** 下载仓库内的IntuneWinAppUtil.exe
2. 示例软件WinSCP https://winscp.net/download/WinSCP-6.5.3-Setup.exe/download

# 一、转换格式
以WinSCP软件安装包为例，将转换器.exe和软件安装包放到一起比较方便操作。
![[../Pasted image 20250727150404.png]]
1. 在地址栏输入cmd回车。 ![[Pasted image 20250727150519.png]]
2. 输入命令，将WinSCP-6.5.3-Setup.exe转换为Intunewin格式。这里也可选双击运行IntuneWinAppUtil.exe根据命令提示框内提示操作。
```
IntuneWinAppUtil.exe -c C:\YourSourceFolder -s WinSCP-6.5.3-Setup.exe -o C:\OutputFolder
```
 3. 最终： ![[Pasted image 20250727150940.png]]

# 二、创建Apps
1. 在Intune管理中心中，点击左侧的Apps，在右侧选择Windows平台。 ![[Pasted image 20250727145702.png]]
- 点击Create创建一个App，并在右侧选择App类型为Win32。![[Pasted image 20250727145959.png]]
2. 点击下方的Select，在下一页面点击Select app package file选择我们转换好的Intunewin文件。随后点击OK。![[Pasted image 20250727151307.png]]
-  App信息请按需填写，有星号\*的为必填。完成后点击Next。![[Pasted image 20250727151903.png]]
3. 填写安装命令和卸载命令，每个应用都会不同，以WinSCP示例: ```
```
# 安装命令 (静默的/S一定是大写的S)

WinSCP-6.5.3-Setup.exe /S

# 卸载命令

"C:\Program Files (x86)\WinSCP\unins000.exe" /VERYSILENT /NORESTART

```
- 填写Program页，完成后点击Next。![[Pasted image 20250727153212.png]]
4. 填写系统需求，填写完成后点击Next。![[Pasted image 20250727153357.png]]
5. 检测规则，用于检测用户是否已经安装了该软件，我们选择手动配置规则，并点击Add。![[Pasted image 20250727153519.png]]
    - 在右侧弹出的子菜单中，输入文件夹路径、文件名、检测模式选择[文件或文件夹是否存在]、Associated with a 32-bit app on 64-bit clients选择[YES]因为WinSCP默认安装到x86文件夹。完成后点击OK、Next。![[Pasted image 20250727153848.png]]
6. Dependence和Supersedence：
    - Dependence是依赖项，WinSCP无需依赖程序，直接Next。
    - Supersedence用于取代其他应用，例如你安装WinSCP就可以设置自动卸载掉MobaX，或者更新WinSCP，则自动卸载先前的旧版本。这根据你自己需求填写，无需求直接Next。
7. Assignments权限分配，在这里注意三个区域，从上到下第一个是只要写进去就会直接推送安装，第二个是按需求安装，用户可以选择不装，第三个是卸载安装此App的组。![[Pasted image 20250727155324.png]]
    * 我们在Required中添加想要强制推送的组，组内所有用户的Windows设备都会被安装，此处可以加入[Included]一些用户或组或者排除[Excluded]一些用户。
    * 设置好后我们点击Next。![[Pasted image 20250727155712.png]]
8. 最终确认无误后，点击Create。![[Pasted image 20250727160650.png]]

# 三、确认安装状态
1. 点击左侧Devices==>Windows进入目标计算机页面。![[Pasted image 20250727160927.png]]
2. 点击左侧的Managed Apps。![[Pasted image 20250727161018.png]]
3. 查看安装状态。![[Pasted image 20250727161055.png]]
    - 如果一直卡在Waiting for install status，则可以尝试同步几次，若长时间不行，则从安装指令或检测逻辑找原因。