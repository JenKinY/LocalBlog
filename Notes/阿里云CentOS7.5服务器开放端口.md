两个步骤：

1. 主机防火墙端口开放
2. 配置安全组

## 一、 主机防火墙开放

**CentOS7 默认使用 firewalld 防火墙，如果想换回 iptables 防火墙，可关闭 firewalld 并安装iptables**

#### 一、关闭 firewall：

1. 关闭防火墙

```shell
systemctl stop firewalld.service
```

2. 禁止开机启动防火墙

```shell
systemctl disable firewalld.service
```

3. 查看防火墙状态（关闭后显示`not running`，开启后显示`running`）

```shell
firewall-cmd --state
```

#### 二、安装 iptables

1. 查看可用安装包

```
yum list | grep iptables
```

2. 安装 iptables

```shell
yum install iptables-services
```

1. 重启防火墙使配置文件生效

```shell
systemctl restart iptables.service
```

1. 设置 iptables 防火墙为开机启动项

```shell
systemctl enable iptables.service
```

执行状态查询命令，显示如下，说明安装启动成功：：

```shell
systemctl status iptables.service
```

```
[root@localhost /]# systemctl status iptables.service
● iptables.service - IPv4 firewall with iptables
	  Loaded: loaded (/usr/lib/systemd/system/iptables.service; enabled; vendor preset: disabled)
	  Active: active (exited) since Mon 2019-03-18 02:18:04 PDT; 20h ago
	 Process: 6029 ExecStart=/usr/libexec/iptables/iptables.init start (code=exited, status=0/SUCCESS)
 Main PID: 6029 (code=exited, status=0/SUCCESS)
 CGroup: /system.slice/iptables.service

Mar 18 02:18:03 localhost.localdomain systemd[1]: Starting IPv4 firewall with...
Mar 18 02:18:04 localhost.localdomain iptables.init[6029]: iptables: Applying...
Mar 18 02:18:04 localhost.localdomain systemd[1]: Started IPv4 firewall with ...
Hint: Some lines were ellipsized, use -l to show in full.
```

#### 三、通过 iptables 开放端口

1. 查看本机 iptables 的设置情况

```shell
iptables -nL
```

2. 开放指定端口，编辑配置

```shell
vim /etc/sysconfig/iptables
```

**内容如下：**

```shell
# Generated by iptables-save v1.4.21 on Wed Mar 13 01:55:54 2019
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A INPUT -p tcp -m tcp --dport 443 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 8081 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 5060 -j ACCEPT
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT
# Completed on Wed Mar 13 01:55:54 2019
~                                         
```

**注意：** 要在文件的中间即`COMMIT`命令上面添加
如果有下面2行，需要在下面2行的上面添加

```shell
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
```

可仿照上面的22，80,3306等端口方式，复制一份修改端口号即可

1. 修改完后保存退出`:wq!`
2. 重启服务即可

```shell
systemctl restart iptables.service
```



> 参考：
>
> [威威dett-CentOS7使用iptables开放特定端口-CSDN](https://blog.csdn.net/u013626215/article/details/88661484)

## 二、配置安全组

在阿里云ECS控制台配置安全组即可