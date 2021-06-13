#### 1. Clone Adafruit_CircuitPython_SSD1306 到本地

```shell
https://github.com/adafruit/Adafruit_CircuitPython_SSD1306.git
```

#### 2. 本地安装

```shell
# 进入项目
cd Adafruit_CircuitPython_SSD1306
python3 setup.py install
```

#### 3. 处理报错

`秉承谁报错安装谁的原则处理报错`

![image-20210325221923728](image-20210325221923728.png)

##### 1. 安装 setuptools_scm

```shell
python3 -m pip install --upgrade setuptools_scm  --trusted-host=pypi.python.org --trusted-host=pypi.org --trusted-host=files.pythonhosted.org --trusted-host=pypi.tuna.tsinghua.edu.cn -i https://pypi.tuna.tsinghua.edu.cn/simple/
```

##### 2. 继续 安装

![image-20210325222123169](image-20210325222123169.png)

```shell
python3 -m pip install --upgrade spidev --trusted-host=pypi.python.org --trusted-host=pypi.org --trusted-host=files.pythonhosted.org --trusted-host=pypi.tuna.tsinghua.edu.cn -i https://pypi.tuna.tsinghua.edu.cn/simple/

python3 -m pip install --upgrade adafruit-circuitpython-busdevice --trusted-host=pypi.python.org --trusted-host=pypi.org --trusted-host=files.pythonhosted.org --trusted-host=pypi.tuna.tsinghua.edu.cn -i https://pypi.tuna.tsinghua.edu.cn/simple/
```

#### 3. 最后成了

![image-20210325222245829](image-20210325222245829.png)

感谢这篇文章：https://www.freesion.com/article/95601397676/

