### 推荐使用MobaX，可直接实现scp+ssh。

# 一、创建新的私钥

1. 创建新的私钥前，首先准备.cnf文件。

```
[ req ]
default_bits       = 2048
prompt             = no
default_md         = sha256
distinguished_name = dn
req_extensions     = req_ext

[ dn ]
C  = CN
ST = Shanghai
L  = Shanghai
O  = xxxx
OU = IT
CN = openwrt.xxxx.one

[ req_ext ]
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = openwrt.xxxx.one
IP.1 = 10.0.0.1

# 其中
# CN = 
# DNS.1 =
# IP.1 =
# 这三项必须修改，这仨都需要与你的openwrt对应，不对应的话访问还是不安全。
```

2. 保存此文件，命名为openwrt-req.cnf，上传至openwrt的/etc/ssl/。如果你没有图形化的scp工具，那么使用命令行scp。之后不再举例。

```
scp openwrt-req.cnf root@10.0.0.1:/etc/ssl/
```

3. 然后，我们使用openwrt-req.cnf在openwrt上生成CSR和私钥。

```
cd /etc/ssl
openssl genrsa -out openwrt.key 2048

openssl req -new -key openwrt.key \
  -out openwrt.csr \
  -config openwrt-req.cnf
```

# 二、签发

1. 将新生成的openwrt.key和openwrt.csr取回，并传至CA Server进行签发。

A. 在CA Server上签发示例：

```
certreq -submit -attrib "CertificateTemplate:WebServer" openwrt.csr openwrt.crt
```

B. 而我使用的是CA Server的Web界面，示例：

![](https://cdn.nlark.com/yuque/0/2025/png/32714118/1753500380553-49c0bad9-edc0-4b23-bc00-38fecaa9b0f1.png)

![](https://cdn.nlark.com/yuque/0/2025/png/32714118/1753500412806-793668c2-9dac-45eb-9aea-22112c1b3b04.png)

使用文本编辑器打开取回的csr文件，将-----BEGIN CERTIFICATE REQUEST-----开头的所有内容粘贴至框内，注意，是所有内容，包括文件头和尾。

证书模板选择Web Server，如果未显示则证明你使用的不是管理员账号登录的CA Server。

![](https://cdn.nlark.com/yuque/0/2025/png/32714118/1753500470553-575e8797-053b-457c-8bad-fb1f52398553.png)

点Submit提交。

![](https://cdn.nlark.com/yuque/0/2025/png/32714118/1753500670768-a0365c0a-596d-4b93-98a7-32c75ce9dbf8.png)

选择Base 64编码，我们只需要证书即可，也可以下载证书链留存。

在首页的Download a CA certificate, certificate chain, or CRL下载Base64格式的根证书。

![](https://cdn.nlark.com/yuque/0/2025/png/32714118/1753500864114-66812cf8-af16-4c88-9caf-225df0a8a363.png)

![](https://cdn.nlark.com/yuque/0/2025/png/32714118/1753500897913-b72690e8-fef8-47a2-a801-f14930aeb3eb.png)

2. 将下载好的openwrt.cer和rootca.cer上传至openwrt的/etc/ssl/

# 三、配置新证书

进入/etc/ssl/

```
cd /etc/ssl/
```

将刚上传的openwrt证书和根证书进行合并，因为是base64编码，cer和crt可以互转，这里直接编辑合并。

```
cat openwrt.cer rootca.cer > openwrt-full.crt
```

然后配置uhttpd

```
uci set uhttpd.main.cert='/etc/ssl/openwrt-fullchain.crt'
uci set uhttpd.main.key='/etc/ssl/openwrt.key'
uci commit uhttpd
/etc/init.d/uhttpd restart
```

# 搞定

测试请关闭所有浏览器窗口，重新打开Openwrt。

如果不可以，检查CN = DNS.1 = IP.1 = 这三项是否正确。并且，在你本地计算机上是否安装了根证书。看浏览器报的认证错误代码，寻找对应攻略。