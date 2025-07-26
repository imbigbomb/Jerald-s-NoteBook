# 服了
```
https://developer.aliyun.com/mirror/centos?spm=a2c6h.13651102.0.0.3e221b11o53Rj3
```
# 报错

在CentOS中用yum安装时报错。

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1734405690162-f0f4eff7-98d4-453c-8005-6bc9ee5b438e.png)

# 原因

CentOS 7 的生命周期已经结束，CentOS 7 的官方仓库在 2024 年 6 月 30 日之后已经停止维护。

# 解决方法

首先进入CentOS-Base.repo的目录。

```
cd /etc/yum.repos.d #进入目录
ls                  #列出目录下所有文件
```

![](https://cdn.nlark.com/yuque/0/2024/png/32714118/1734406082956-a4873bc7-af28-47e4-8d81-691a1fa0792e.png)

我们需要对CentOS-Base.repo进行修改。

```
cp CentOS-Base.repo  CentOS-Base.repo.backup  #备份CentOS-Base.repo
vi CentOS-Base.repo                           #使用vi修改CentOS-Base.repo
```

进入编辑器后，我们按下`Shift`+`;`，在屏幕下方出现`：` 后，我们输入`%d`删除所有内容。

然后按下i键进入插入模式

将下方全部字样粘贴进清空后的编辑器。

```
# CentOS-Base.repo
#
# The mirror system uses the connecting IP address of the client and the
# update status of each mirror to pick mirrors that are updated to and
# geographically close to the client.  You should use this for CentOS updates
# unless you are manually picking other mirrors.
#
# If the mirrorlist= does not work for you, as a fall back you can try the 
# remarked out baseurl= line instead.
#
#
 
[base]
name=CentOS-$releasever - Base
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
#baseurl=http://vault.centos.org/7.9.2009/x86_64/os/
baseurl=http://vault.centos.org/7.9.2009/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
 
#released updates 
[updates]
name=CentOS-$releasever - Updates
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
#baseurl=http://vault.centos.org/7.9.2009/x86_64/os/
baseurl=http://vault.centos.org/7.9.2009/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
 
#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
#$baseurl=http://mirror.centos.org/centos/$releasever/extras/$basearch/
#baseurl=http://vault.centos.org/7.9.2009/x86_64/os/
baseurl=http://vault.centos.org/7.9.2009/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
 
#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/centosplus/$basearch/
#baseurl=http://vault.centos.org/7.9.2009/x86_64/os/
baseurl=http://vault.centos.org/7.9.2009/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```

确认无误后，按下ESC回到命令行模式，按下`Shift`+`;`，在屏幕下方出现`：` 后，输入wq，回车退出。

接下来我们清理 YUM 缓存并重新生成缓存。