## 一、设备名称

1. ESP8266-NodeMCU 节点 clientId 遵循以下规则：

```c++
// 固定以"HHP-设备类型"为首（HHP,HopeHomePerception,HopeHome感知终端）
// 例如："HHP-ESP8266-84:CC:A8:A2:5A:CF"
String clientId = "HHP-ESP8266-" + WiFi.macAddress();
```

2. ESP8266-NodeMCU 节点发布消息规则如下：

```c++
// 这么做是为确保不同用户进行MQTT信息发布时，ESP8266客户端名称各不相同，
// 统一为 "hopehome/当前设备位置[perception,gateway,cloud]/pub/设备类型/MAC地址"
String topicString = "hopehome/perception/pub/esp8266/"+WiFi.macAddress();
```

2. ESP8266-NodeMCU 节点订阅主题规则如下：

```c++
// 这么做是为确保不同设备使用同一个MQTT服务器测试消息订阅时，所订阅的主题名称不同
// 统一为 "hopehome/来源位置[perception,gateway,cloud]/sub/设备类型/MAC地址"
// 例如："hopehome/gateway/sub/esp8266/"
String topicString = "hopehome/gateway/sub/esp8266/" + WiFi.macAddress();
```

