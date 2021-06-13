> 参考文章：
>
> [树莓派如何连接使用 IIC(I2C) OLED 显示屏](https://zhuanlan.zhihu.com/p/205596025)

- RPi 4B + HopeEdgeOS 已启用I2C

### 1. 将 OLED 屏幕连接到 树莓派

查看当前的 I2C 设备：

```shell
i2cdetect -y 1
```

![image-20210324205514795](image-20210324205514795.png)

检测到的 OLED 的地址为 *3C* 。对于旧版本的树莓派，如果没有看到地址，可以尝试使用命令 `i2cdetect -y 0` 检测。

### 2. 安装 OLED Python 库

```shell
pip3 install adafruit-circuitpython-ssd1306
```

### 3. Clone 示例代码

```shell
git clone https://github.com/adafruit/Adafruit_CircuitPython_SSD1306.git
```

### 4. 运行 HelloWord

```shell
python3 examples/c_pillow_demo.py
```

![img](0W_ROMBW23Q3YGIJ34SDJC.jpg)