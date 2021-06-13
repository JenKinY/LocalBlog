### 1. 启动 EMQxEdge

```shell
docker run -d --name emqxedge -p 1883:1883 -p 18083:18083 emqx/emqx-edge:4.2.8
```

### 2. 进入容器

```shell
docker exec -it e81821315c84 /bin/bash
```

### 3. 编辑桥接配置文件

```shell
vi ./etc/plugins/emqx_bridge_mqtt.conf
```

```shell
# 配置 桥接地址（云服务器地址）
## 桥接地址： 使用节点名则用于 rpc 桥接，使用 host:port 用于 mqtt 连接
bridge.mqtt.aws.address = 127.0.0.1:1883

## 桥接的协议版本
## 枚举值: mqttv3 | mqttv4 | mqttv5
bridge.mqtt.aws.proto_ver = mqttv4

## mqtt 连接是否启用桥接模式
bridge.mqtt.aws.bridge_mode = true

## mqtt 客户端的 client_id
bridge.mqtt.aws.client_id = bridge_aws

## mqtt 客户端的 clean_start 字段
## 注: 有些 MQTT Broker 需要将 clean_start 值设成 `true`
bridge.mqtt.aws.clean_start = true

## mqtt 客户端的 username 字段
bridge.mqtt.aws.username = user

## mqtt 客户端的 password 字段
bridge.mqtt.aws.password = passwd

## mqtt 客户端是否使用 ssl 来连接远程服务器
bridge.mqtt.aws.ssl = off

## 客户端 SSL 连接的 CA 证书 (PEM格式)
bridge.mqtt.aws.cacertfile = etc/certs/cacert.pem

## 客户端 SSL 连接的 SSL 证书
bridge.mqtt.aws.certfile = etc/certs/client-cert.pem

## 客户端 SSL 连接的密钥文件
bridge.mqtt.aws.keyfile = etc/certs/client-key.pem

## SSL 加密算法
bridge.mqtt.aws.ciphers = ECDHE-ECDSA-AES256-GCM-SHA384,ECDHE-RSA-AES256-GCM-SHA384

## TLS PSK 的加密算法
## 注意 'listener.ssl.external.ciphers' 和 'listener.ssl.external.psk_ciphers' 不能同时配置
##
## See 'https://tools.ietf.org/html/rfc4279#section-2'.
bridge.mqtt.aws.psk_ciphers = PSK-AES128-CBC-SHA,PSK-AES256-CBC-SHA,PSK-3DES-EDE-CBC-SHA,PSK-RC4-SHA

## 客户端的心跳间隔
bridge.mqtt.aws.keepalive = 60s

## 支持的 TLS 版本
bridge.mqtt.aws.tls_versions = tlsv1.2,tlsv1.1,tlsv1
```

```shell
# 配置 MQTT 桥接转发和订阅主题
## 桥接的 mountpoint(挂载点)
bridge.mqtt.aws.mountpoint = bridge/aws/${node}/

## 转发消息的主题
bridge.mqtt.aws.forwards = topic1/#,topic2/#

## 用于桥接的订阅主题
bridge.mqtt.aws.subscription.1.topic = cmd/topic1

## 用于桥接的订阅 qos
bridge.mqtt.aws.subscription.1.qos = 1

## 用于桥接的订阅主题
bridge.mqtt.aws.subscription.2.topic = cmd/topic2

## 用于桥接的订阅 qos
bridge.mqtt.aws.subscription.2.qos = 1
```

### 4. 启动桥接插件

```shell
./bin/emqx_ctl plugins load emqx_bridge_mqtt
```

### 列出全部 bridge 状态

```bash
$ ./bin/emqx_ctl bridges list
name: emqx     status: Stopped
```

#### 启动指定-bridge启动指定 bridge

```bash
$ ./bin/emqx_ctl bridges start cloud
Start bridge successfully.
```



| 端口  | 说明                  |
| ----- | --------------------- |
| 1883  | TCP 端口              |
| 18083 | Dashboard Web监控页面 |
| 8083  | Websocket 端口        |
| 8084  | Websocket/TLS 端口    |
| 8883  | TCP/TLS 端口          |

