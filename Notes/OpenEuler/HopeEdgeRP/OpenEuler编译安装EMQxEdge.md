## 源码编译

首先需要安装 erlang 环境！！！

```shell
git clone https://github.com/emqx/emqx-rel.git

cd emqx-rel && make emqx-edge

cd _build/emqx-edge/rel/emqx && ./bin/emqx console
```

编译出错可查看文章: [OpenEuler安装erlang环境](./OpenEuler安装erlang环境.md)

## 程序启动

> **注意：**此时我们的目录在：/root/emqx-rel/_build/emqx-edge/rel/emqx

控制台调试模式启动，检查 *EMQ X Edge* 是否可正常启动:

```shell
./bin/emqx console
```

*EMQ X Edge* 消息服务器如启动正常，控制台输出:

```shell
Starting emqx on node emqx@127.0.0.1
Start http:management listener on 8081 successfully.
Start http:dashboard listener on 18083 successfully.
Start mqtt:tcp listener on 127.0.0.1:11883 successfully.
Start mqtt:tcp listener on 0.0.0.0:1883 successfully.
Start mqtt:ws listener on 0.0.0.0:8083 successfully.
Start mqtt:ssl listener on 0.0.0.0:8883 successfully.
Start mqtt:wss listener on 0.0.0.0:8084 successfully.
EMQ X Edge 4.2.2 is running now!
```

CTRL+C 关闭控制台。

守护进程模式启动:

```shell
./bin/emqx start
```

启动错误日志将输出在 log/ 目录。

*EMQ X Edge* 消息服务器进程状态查询:

```shell
./bin/emqx_ctl status
```

正常运行状态，查询命令返回:

```shell
$ ./bin/emqx_ctl status
Node 'emqx@127.0.0.1' is started
emqx edge 4.2.2 is running
```

*EMQ X Edge* 消息服务器提供了 Web 管理控制台 URL:

```shell
http://localhost:18083
```

停止服务器:

```shell
./bin/emqx stop
```



## 配置桥接

我们需要修改桥接插件 emqx_bridge_mqtt 的配置：

修改后文件：（本文件已保存到本地）

官方教程：https://docs.emqx.cn/cn/broker/latest/bridge/bridge-mqtt.html#%E9%85%8D%E7%BD%AE-mqtt-%E6%A1%A5%E6%8E%A5%E7%9A%84-broker-%E5%9C%B0%E5%9D%80

```properties



## PEM-encoded CA certificates of the bridge.
##
## Value: File
bridge.mqtt.aws.cacertfile = etc/certs/cacert.pem

## Client SSL Certfile of the bridge.
##
## Value: File
bridge.mqtt.aws.certfile = etc/certs/client-cert.pem

## Client SSL Keyfile of the bridge.
##
## Value: File
bridge.mqtt.aws.keyfile = etc/certs/client-key.pem

## SSL Ciphers used by the bridge.
##
## Value: String

## Ciphers for TLS PSK.
## Note that 'bridge.${BridgeName}.ciphers' and 'bridge.${BridgeName}.psk_ciphers' cannot
## be configured at the same time.
## See 'https://tools.ietf.org/html/rfc4279#section-2'.
#bridge.mqtt.aws.psk_ciphers = PSK-AES128-CBC-SHA,PSK-AES256-CBC-SHA,PSK-3DES-EDE-CBC-SHA,PSK-RC4-SHA

## Ping interval of a down bridge.
##
## Value: Duration
## Default: 10 seconds
bridge.mqtt.aws.keepalive = 60s

## TLS versions used by the bridge.
##
## Value: String
bridge.mqtt.aws.tls_versions = tlsv1.2,tlsv1.1,tlsv1

## Bridge reconnect time.
##
## Value: Duration
## Default: 30 seconds
bridge.mqtt.aws.reconnect_interval = 30s
```

### 启用 bridge_mqtt 桥接插件

```shell
./bin/emqx_ctl plugins load emqx_bridge_mqtt
```

### 桥接 CLI 命令

```bash
$ ./bin/emqx_ctl bridges
bridges list                                    # List bridges
bridges start <Name>                            # Start a bridge
bridges stop <Name>                             # Stop a bridge
bridges forwards <Name>                         # Show a bridge forward topic
bridges add-forward <Name> <Topic>              # Add bridge forward topic
bridges del-forward <Name> <Topic>              # Delete bridge forward topic
bridges subscriptions <Name>                    # Show a bridge subscriptions topic
bridges add-subscription <Name> <Topic> <Qos>   # Add bridge subscriptions topic
```

###  列出全部 bridge 状态

```bash
$ ./bin/emqx_ctl bridges list
name: emqx     status: Stopped
```

### 启动指定 bridge

```bash
$ ./bin/emqx_ctl bridges start emqx
Start bridge successfully.
```

### 停止指定 bridge

```bash
$ ./bin/emqx_ctl bridges stop emqx
Stop bridge successfully.
```

###  列出指定 bridge 的转发主题

```bash
$ ./bin/emqx_ctl bridges forwards emqx
topic:   topic1/#
topic:   topic2/#
```

### 添加指定 bridge 的转发主题

```bash
$ ./bin/emqx_ctl bridges add-forwards emqx topic3/#
Add-forward topic successfully.
```

### 删除指定 bridge 的转发主题

```bash
$ ./bin/emqx_ctl bridges del-forwards emqx topic3/#
Del-forward topic successfully.
```

###  列出指定 bridge 的订阅

```bash
$ ./bin/emqx_ctl bridges subscriptions emqx
topic: cmd/topic1, qos: 1
topic: cmd/topic2, qos: 1
```

### 添加指定 bridge 的订阅主题

```bash
$ ./bin/emqx_ctl bridges add-subscription emqx cmd/topic3 1
Add-subscription topic successfully.
```

###  删除指定 bridge 的订阅主题

```bash
$ ./bin/emqx_ctl bridges del-subscription emqx cmd/topic3
Del-subscription topic successfully.
```



## 配置环境变量

上面我们每次执行相关命令都需要进入EMQx的主目录。我们可以将其加入 PATH 环境变量，一劳永逸：

```shell
# 修改环境变量相关文件
[root@openEuler ~]# vi /etc/profile
# 在最后追加一行：（在vi中使用G直接跳到最后一行）
export PATH=/root/emqx-rel/_build/emqx-edge/rel/emqx/bin:$PATH
# :wq 保存退出
# 刷新或重启来重载配置文件
[root@openEuler ~]# source /etc/profile
```

