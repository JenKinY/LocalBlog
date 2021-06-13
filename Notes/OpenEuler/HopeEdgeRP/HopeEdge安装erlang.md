## HopeEdge 编译安装 Erlang

### 1. 下载源码

[官网下载](https://www.erlang.org/downloads)

[GITHUB](https://github.com/erlang)

![image-20210130011910794](image-20210130011910794.png)

### 2. 编译安装

> 参考官方教程：https://github.com/erlang/otp/blob/master/HOWTO/INSTALL.md

#### 01.所需构建工具：

除了 Perl 5 和 sed HopeEdge自带之外，其余几个包都在本地源内有，可直接使用 dnf 安装。

- [ ] GNU `make`

  ```shell
  # HopeEdge 本地源有可直接 dnf 安装
  dnf install -y make
  ```

- [ ] Compiler -- GNU C Compiler, `gcc` or the C compiler frontend for LLVM, `clang`.

  ```shell
  # HopeEdge 本地源有可直接 dnf 安装
  dnf install -y gcc
  ```

- [x] Perl 5

- [ ] GNU `m4`

  ```shell
  # HopeEdge 本地源有可直接 dnf 安装
  dnf install -y m4
  ```

- [ ] `ncurses`, `termcap`, or `termlib` -- The development headers and libraries are needed, often known as `ncurses-devel`. Use `--without-termcap` to build without any of these libraries. Note that in this case only the old shell (without any line editing) can be used.

  ```shell
  # HopeEdge 本地源有可直接 dnf 安装
  dnf install -y ncurses
  dnf install -y ncurses-devel
  ```

- [x] `sed` -- Stream Editor for basic text transformation.

#### 02. 开始编译

```shell
# 解压
tar -zxf otp_src_23.2.tar.gz 
# 现在将目录更改为基本目录，并设置$ERL_TOP变量。
cd otp_src_23.2
export ERL_TOP=`pwd`
# 配置并生成 Makefile , --prefile=安装位置
./configure --prefix=/usr/local/erlang
# 开始编译
make
```

结果最后出错了：

```shell
...
configure: WARNING: No OpenGL headers found, wx will NOT be usable
configure: WARNING: No GLU headers found, wx will NOT be usable
./configure: line 4628: wx-config: command not found
configure: WARNING:
                wxWidgets must be installed on your system.

		Please check that wx-config is in path, the directory
		where wxWidgets libraries are installed (returned by
		'wx-config --libs' or 'wx-config --static --libs' command)
		is in LD_LIBRARY_PATH or equivalent variable and
		wxWidgets version is 2.8.4 or above.
...
```

![image-20210130021023553](image-20210130021023553.png)

可以看到这句话，提示我们必须要安装 `wxWidgets` 这个包，官方推荐3.0，触发支线任务：

（我也不太懂装这个库干嘛）

------

官方 Github 仓库https://github.com/wxWidgets/wxWidgets

官网：http://www.wxwidgets.org/downloads/

```shell
# 首先 dnf 安装 gtk3,gtk3-devel
dnf install -y gtk3
dnf install -y gtk3-devel

# 下载 Releases 3.1.4 (必须3.0+)
tar -zxf wxWidgets-3.1.4
cd wxWidgets-3.1.4

# 安装子模块
# 可以在官方 github 仓库 /src/ 下看到子模块
# 我使用了一种笨办法，将其一个一个 clone 下来再编译安装
# 子模块：zlib,expat,png,jpeg,tiff
# zlib-devel,expat-devel HopeEdge本地源都有
```

```shell
# 本地源安装所需库
dnf install -y zlib-devel
dnf install -y expat-devel
# 编译安装所需库
# 以 tiff为例
# clone 到本地
git clone https://github.com/wxWidgets/libtiff.git
cd libtiff
./configure
make
make install
```

```shell
../configure --with-gtk
make
make install
```

```shell
# 安装完成之后 检测 出现版本号则表示安装成功
[root@localhost]# wx-config --version
3.1.4
```

------

继续生成我们的 Erlang / OTP 的 makefile:

```shell
[root@localhost]# ./configure --prefix=/usr/local/erlang
...
checking GL/gl.h usability... yes
checking GL/gl.h presence... yes
checking for GL/gl.h... yes
checking GL/glu.h usability... no
checking GL/glu.h presence... no
checking for GL/glu.h... no
checking OpenGL/glu.h usability... no
checking OpenGL/glu.h presence... no
checking for OpenGL/glu.h... no
checking for debug build of wxWidgets... checking for wx-config... /usr/local/bin/wx-config
checking for wxWidgets version >= 2.8.4 (--unicode --debug=yes)... yes (version 3.1.4)
checking for wxWidgets static library... no
checking for standard build of wxWidgets... checking for wx-config... (cached) /usr/local/bin/wx-config
checking for wxWidgets version >= 2.8.4 (--unicode --debug=no)... yes (version 3.1.4)
checking for wxWidgets static library... no
checking if we can add -Werror=return-type to CFLAGS (via CFLAGS)... yes
checking if we can add -Werror=return-type to CXXFLAGS (via CFLAGS)... yes
configure: creating aarch64-unknown-linux-gnu/config.status
config.status: creating config.mk
config.status: creating c_src/Makefile
configure: WARNING: No GLU headers found, wx will NOT be usable

*********************************************************************
**********************  APPLICATIONS DISABLED  **********************
*********************************************************************
...
```

![image-20210130174105477](image-20210130174105477.png)

尝试安装 mesa-libGLU 和 mesa-libGLU-devel ，这两个库在本地源有：

```shell
# dnf 安装 mesa-libGLU 和 mesa-libGLU-devel
dnf install -y mesa-libGLU
dnf install -y mesa-libGLU-devel
```

OK 这个问题解决了，但是又来了新的问题：

```shell
[root@localhost]# ./configure --prefix=/usr/local/erlang
...
checking GL/glu.h usability... yes
checking GL/glu.h presence... yes
checking for GL/glu.h... yes
checking for debug build of wxWidgets... checking for wx-config... /usr/local/bin/wx-config
checking for wxWidgets version >= 2.8.4 (--unicode --debug=yes)... yes (version 3.1.4)
checking for wxWidgets static library... no
checking for standard build of wxWidgets... checking for wx-config... (cached) /usr/local/bin/wx-config
checking for wxWidgets version >= 2.8.4 (--unicode --debug=no)... yes (version 3.1.4)
checking for wxWidgets static library... no
checking for wxwidgets 2.8 compatibility ... no
checking for wxwidgets opengl support... no
checking for GLintptr... yes
checking for GLintptrARB... yes
checking for GLchar... yes
checking for GLcharARB... yes
checking for GLhalfARB... yes
checking for GLint64EXT... yes
checking GLU Callbacks uses Tiger Style... no
checking for wx/stc/stc.h... yes
checking if we can link wxwidgets programs... no
checking if we can add -Werror=return-type to CFLAGS (via CFLAGS)... yes
checking if we can add -Werror=return-type to CXXFLAGS (via CFLAGS)... yes
configure: creating aarch64-unknown-linux-gnu/config.status
config.status: creating config.mk
config.status: creating c_src/Makefile
configure: WARNING: Can not link wx program are all developer packages installed?

*********************************************************************
**********************  APPLICATIONS DISABLED  **********************
*********************************************************************
...
```

这个问题弄了很久，一直没法链接到 wx , 最后放弃直接使用 Docker运行。

下面是过程中一些记录：

**猜测：**

- 可能是 wx 相关的依赖包没有安装完整，也就是需要将其所有依赖都安装完整，但是在HopeEdge配置费时费力。

- Cache 原因。有可能是缓存原因，但是感觉可能性不大，因为我全部删除之后重新构建依旧不行。

  [猜测来源：安装erlang\] 解决方法 an not link the wx driver, wx will NOT be](https://blog.csdn.net/iteye_563/article/details/82553929)

**帮助很大的文章：**

[从零开始搭建wxWidgets，erlang，RabbitMQ](https://blog.csdn.net/gfk3009/article/details/104646345/)



**最后 wx-Widgets 的安装情况：**

![image-20210201165742289](image-20210201165742289.png)

![image-20210202020601577](image-20210202020601577.png)

**编译 Erlang/OTP 的结果：**

依旧是 APPLICATIONS DISABLED....

但是！！！！！！！！！！！！

直到我换了一个 Erlang/OPT 版本后，同样的版本居然成功了！！！！！好活好活

![image-20210202023632722](image-20210202023632722.png)

