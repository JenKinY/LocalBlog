##  安装PyQt5、QtDesigner、PyUIC、opencv等工具

```
pip install pyqt5
```

电脑会自动下载并安装合适版本的pyqt5.

完成后，再输入

```
pip install pyqt5-tools
```

自动完成QtDesigner和PyUIC等的安装。



## ubuntu--20.04 安装中文输入法（google拼音）

安装指令：

    sudo apt-get install language-pack-zh-hans
     
    sudo apt-get install fcitx-googlepinyin

配置：

1、搜索框输入：Language Support，选择fcitx，重启机器

## 树莓派安装中文输入法Fcitx及Google拼音输入法

```bash
sudo apt install fcitx fcitx-googlepinyin fcitx-module-cloudpinyin fcitx-sunpinyin
```



## 树莓派安装 dlib , face_recognition

使用以下命令安装所需的库：

```shell
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install build-essential \
    cmake \
    gfortran \
    git \
    wget \
    curl \
    graphicsmagick \
    libgraphicsmagick1-dev \
    libatlas-dev \
    libavcodec-dev \
    libavformat-dev \
    libboost-all-dev \
    libgtk2.0-dev \
    libjpeg-dev \
    liblapack-dev \
    libswscale-dev \
    pkg-config \
    python3-dev \
    python3-numpy \
    python3-pip \
    zip
sudo apt-get clean
```

安装具有阵列支持的picamera python库（如果使用相机）：

```shell
sudo apt-get install python3-picamera
sudo pip3 install --upgrade picamera[array]
```

临时启用更大的交换文件大小（因此dlib编译不会因内存有限而失败）：

```shell
sudo vi /etc/dphys-swapfile

将 CONF_SWAPSIZE=100 改为 CONF_SWAPSIZE=1024

sudo /etc/init.d/dphys-swapfile restart
```

下载并本地编译安装dlib v19.6：

```shell
mkdir -p dlib
git clone -b 'v19.6' --single-branch https://github.com/davisking/dlib.git dlib/
cd ./dlib
sudo python3 setup.py install
```

安装`face_recognition`：

```shell
sudo pip3 install face_recognition
```

（如果此步下载缓慢可以直接下载对应的 *.whl 文件 再使用 pip3 install *.whl进行本地安装）

现在已经安装了dlib，恢复了交换文件大小的更改：

```shell
sudo vi /etc/dphys-swapfile

将 CONF_SWAPSIZE=1024 改回 CONF_SWAPSIZE=100 、

sudo /etc/init.d/dphys-swapfile restart
```

下载人脸识别代码示例：

```shell
git clone --single-branch https://github.com/ageitgey/face_recognition.git
cd ./face_recognition/examples
python3 facerec_on_raspberry_pi.py
```

完全可选：如果需要桌面GUI，请安装PIXEL：

```shell
sudo apt-get install --no-install-recommends xserver-xorg xinit raspberrypi-ui-mods
```



> 官方树莓派安装文档：https://gist.github.com/ageitgey/1ac8dbe8572f3f533df6269dab35df65