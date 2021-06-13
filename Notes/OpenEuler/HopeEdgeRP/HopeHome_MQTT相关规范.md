emqx 主目录：

```shell
[root@openEuler emqx]# pwd
/root/emqx-rel/_build/emqx-edge/rel/emqx
```



## 命名规则

### 1. MQTT 客户端命名

```c++
// 固定以"HHP-设备类型"为首（HHP,HopeHomePerception,HopeHome感知终端）
// HH[P:perception,G:gateway,C:Cloud]
// 例如："HHP-ESP8266"
String clientId = "HHP-ESP8266" + WiFi.macAddress();
```

### 2. ESP8266-NodeMCU 发布主题

```c++
// 统一为 "hopehome/当前设备位置[perception,gateway,cloud]/pub/设备类型/MAC地址"
String topicString = "hopehome/perception/pub/esp8266/" + WiFi.macAddress();
```

### 3. ESP8266-NodeMCU 订阅主题

```c++
// 统一为 "hopehome/来源位置[perception,gateway,cloud]/sub/设备类型/MAC地址"
String topicString = "hopehome/gateway/sub/esp8266/" + WiFi.macAddress();
```

且 perception 端收到的所有消息都必须来自 gateway ，cloud 发布的消息通过 EMQXedge桥接转换