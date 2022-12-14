# study_vps



# <a id="user-content-科学上网vps-搭建-x-ui-面板"></a>[](#科学上网vps-搭建-x-ui-面板)科学上网：VPS 搭建 X-UI 面板

## <a id="user-content-使用x-ui搭建代理服务具有以下优点"></a>[](#使用x-ui搭建代理服务具有以下优点)使用X-UI搭建代理服务，具有以下优点：

- 支持系统状态监控：如CPU、内存、硬盘等状态
- 支持多用户多协议，网页可视化操作
- 支持流量统计
- 支持自定义Xray配置模板
- 支持HTTPS访问面板
- 支持面板自定义端口，账号与密码
- 快速生成分享连接或二维码
- 支持CDN套用
- 支持Fallback分流设置

## <a id="user-content-一-准备工作"></a>[](#一-准备工作)一、 准备工作

- 1、已经解析的域名，Win+R输入CMD 回车：键入ping 空格输入你的域名，检查一下是否可以ping 通过；
- 2、一台境外VPS主流系统，例如：Debian/Ubuntu/CentOS
- 3、下载并安装FinalShell SSH工具

Windows版下载地址: http://www.hostbuf.com/downloads/finalshell_install.exe

macOS版下载地址: http://www.hostbuf.com/downloads/finalshell_install.pkg

## <a id="user-content-二安装更新运行环境"></a>[](#二安装更新运行环境)二、安装更新运行环境



下面环境的安装方式，大家根据自己的系统选择命令安装就好了。

### <a id="user-content-1debianubuntu系统执行以下命令"></a>[](#1debianubuntu系统执行以下命令)Debian/Ubuntu系统执行以下命令：

```shell
apt update -y && apt install -y curl && apt install -y socat 
```

### <a id="user-content-2centos系统执行以下命令"></a>[](#2centos系统执行以下命令)CentOS系统执行以下命令：

```shell
yum update -y && yum update -y && yum install -y socat 
```

## 注意时间同步
#### 查看系統時間命令： `date`
#### 修改系统时间命令：`dpkg-reconfigure tzdata`


### <a id="user-content-运行acme脚本"></a>[](#运行acme脚本)运行Acme脚本

```shell
curl https://get.acme.sh | sh 
```

### <a id="user-content-申请证书及密钥"></a>[](#申请证书及密钥)申请证书及密钥

此处注意若失败可能是端口开放原因
```shell
iptables -I INPUT -p tcp --dport 80 -j ACCEPT 
```
【将代码中注释的邮箱和域名，改为你自己的】

```shell
~/.acme.sh/acme.sh --register-account -m example@gmail.com 
```

* * *

```shell
~/.acme.sh/acme.sh  --issue -d 输入你的域名  --standalone 
```

### <a id="user-content-下载证书及密钥"></a>[](#下载证书及密钥)下载证书及密钥

```shell
~/.acme.sh/acme.sh --installcert -d 输入你的域名 --key-file /root/private.key --fullchain-file /root/cert.crt 
```
***
## <a id="user-content-安装-x-ui-面板"></a>[](#安装-x-ui-面板)三、安装 X-ui 面板

### <a id="user-content-申请-ssl-证书"></a>[](#申请-ssl-证书)申请 SSL 证书
### <a id="user-content-安装升级x-ui面板一键脚本"></a>[](#安装升级x-ui面板一键脚本)安装&升级x-ui面板一键脚本

```shell
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh) 
```

### <a id="user-content-bbr2加速"></a>[](#bbr2加速)安装BBR2加速

```shell
wget --no-check-certificate -q -O bbr2.sh "https://github.com/yeyingorg/bbr2.sh/raw/master/bbr2.sh" && chmod +x bbr2.sh && bash bbr2.sh auto 
```

## <a id="user-content-注意事项"></a>[](#注意事项)注意事项

- 1、如何在申请证书及密钥这一步搭建失败，检查并放行端口，口令如下：

### <a id="user-content-放行80端口"></a>[](#放行80端口)放行80端口

```shell
 iptables -I INPUT -p tcp --dport 80 -j ACCEPT 
```

### <a id="user-content-放行443端口把80改成443"></a>[](#放行443端口把80改成443)放行443端口，把80改成443

```shell
 iptables -I INPUT -p tcp --dport 443 -j ACCEPT 
```

查看已开放的端口

```shell
 firewall-cmd --list-ports 
```

开放端口（开放后需要要重启防火墙才生效）

```shell
firewall-cmd --zone=public --add-port=3338/tcp --permanent 
```

重启防火墙

```shell
firewall-cmd --reload 
```

停止防火墙

```shell
systemctl stop firewalld 
```

- 1、如果是全新的初次安装x-ui面板要求输入用户名和密码以及端口；可以直接回车采用默认
- 2、如果直接回车全新安装，默认网页端口为 54321，用户名和密码默认都是 admin ， 如何遇到打不开的情况，可能是端口没有放行，用【方法1】键入停止防火墙代码，或键入开放端口代码。
- 3、V2ray软件：设置——参数设置——V2rayN设置——Core类型改为Xray_Core
