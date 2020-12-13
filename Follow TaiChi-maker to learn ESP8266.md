> 视频地址：https://www.bilibili.com/video/BV1L7411c7jw
>
> TaiChi-Maker 官方网站：http://www.taichi-maker.com/homepage/esp8266-nodemcu-iot/

# 0-1 序言

ESP8266-NodeMCU开发板，核心是 ESP8266 芯片，NodeMCU 则是开发板的名字。

![image.png](https://skyzc-halo.oss-cn-shenzhen.aliyuncs.com/blog-img/image_1607758667693.png)

![image.png](https://skyzc-halo.oss-cn-shenzhen.aliyuncs.com/blog-img/image_1607758742728.png?x-oss-process=style/skyzc-halo-img)

![image.png](https://skyzc-halo.oss-cn-shenzhen.aliyuncs.com/blog-img/image_1607758791138.png?x-oss-process=style/skyzc-halo-img)

其中：
- 开发板上印刷的是开发板的引脚名称。
- 蓝底白字表示的 GPIO 编号指的是 ESP8266 芯片的引脚编号。
  例如 芯片的 GPIO4 对应开发板的 D2 引脚。我们要设置其为高电平，有两种方式：
```c
// 第一种：使用开发板编号
digitalWrite(D2,HIGH);
// 第一种：使用芯片编号(GPIO)
digitalWrite(4,HIGH);
```
其中 A0(ADC) 表示模拟引脚,其余的 GPIO 都为数字引脚。
- 红底白字表示电源相关的引脚。GND 表示接地，3V3 表示输出3.3V电压，Vin 表示为开发板供（因此，有两种方式为 NodeMCU 供电：数据线和Vin引脚)。
ps: NodeMCU 数字引脚输出电压为 3.3V。输入的时候电压不能超过 3.3V。
NodeMCU 模拟引脚可读取电压范围为 0-1V;
- 蓝底黑字表示的为 NodeMCU的通讯引脚。用于和其他设备通信，常见的通过方式有串口通信，SPI通信，I2C通信，其中最常用的是串口通信。NodeMCU 有两个硬件串口 其一为**RX(U0TXD) 接收数据和TX(U0TXD)发送数据**，其二为：D4(U1TXD) 接收数据和SD1(U1RXD)。
- 黑底白字表示的是操作NodeMCU自身存储单元的引脚。
- 灰底白字可以忽略

两个按键 
RST: RESET 复位键
FLASH: 刷固件时使用

# 1-1 开发板详解



