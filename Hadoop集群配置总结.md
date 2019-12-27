- 配置 JDK

```bash
# 解压安装包并复制到对应文件夹
[root@master BigData]# tar -zxvf jdk-8u231-linux-x64.tar.gz -C /usr/lib/jvm
# 设置环境变量（全局）
[root@master BigData]# vi /etc/profile
# 在其中输入：
	export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_231
	export JER_HOME=${JAVA_HOME}/jre
	export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
	export PATH=${JAVA_HOME}/bin:$PATH
# 刷新
[root@master BigData]# source /etc/profile
# 检测
[root@master BigData]# java -version

```

- 配置 ssh 免密登录