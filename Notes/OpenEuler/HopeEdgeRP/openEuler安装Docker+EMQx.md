> 系统信息：
> ```shell
$ uname -a
Linux host-12-0-0-154 4.19.90-2003.4.0.0036.oe1.aarch64 #1 SMP Mon Mar 23 19:06:43 UTC 2020 aarch64 aarch64 aarch64 GNU/Linux
> ```shell
> $ cat /etc/os-release 
NAME="openEuler"
VERSION="20.03 (LTS)"
ID="openEuler"
VERSION_ID="20.03"
PRETTY_NAME="openEuler 20.03 (LTS)"
ANSI_COLOR="0;31"

更新 yum 安装 dnf
```shell
# 升级所有包同时也升级软件和系统内核
$ yum update -y
# 只升级所有包，不升级软件和系统内核
$ yum upgrade -y
```

更新系统时间
```shell
# 设置系统时间
$ date -s "20210413 19:36:00"
# 将系统时间同步到硬件，防止系统重启后时间被还原
$ hwclock --systohc
```
安装 Docker
```shell
# 删除掉旧版本
sudo yum remove docker docker-common docker-selinux docker-engine
# 下载发行版 repo 文件 （选择 CentOS的）
wget -O /etc/yum.repos.d/docker-ce.repo https://download.docker.com/linux/centos/docker-ce.repo
# 把软件仓库地址替换为 TUNA
sudo sed -i 's+download.docker.com+mirrors.tuna.tsinghua.edu.cn/docker-ce+' /etc/yum.repos.d/docker-ce.repo
# 编辑 repo 文件
# 由于我们并不是 CentOS 所以需要将其中的 $release 变量修改为 7
vi /etc/yum.repos.d/docker-ce.repo
```
[鲲鹏916架构openEuler-arm64成功安装docker并跑通tomcat容器](https://my.oschina.net/openeuler/blog/4728664)

安装配置 EMQ X Broker
```shell
# 1. 获取 Docker 镜像
docker pull emqx/emqx:4.2.10
# 2. 启动 Docker 容器
docker run -d --name emqx -p 1883:1883 -p 8081:8081 -p 8083:8083 -p 8084:8084 -p 8883:8883 -p 18083:18083 emqx/emqx:4.2.10
```
推荐版本：version v4.2.10
其中端口说明：
| 端口  | 说明                  |
| ----- | --------------------- |
| 1883  | TCP 端口              |
| 18083 | Dashboard Web监控页面 |
| 8083  | Websocket 端口        |
| 8084  | Websocket/TLS 端口    |
| 8883  | TCP/TLS 端口          |

安装配置 Mysql
```shell
# 由于我们的 openEuler 服务器是 armv8 架构的若直接 pull Mysql 就会报错：
# 5.7: Pulling from library/mysql  
# no matching manifest for linux/arm65/v8 in the manifest list entries
# 因此需要 pull arm 架构的 Mysql
docker pull biarms/mysql
# 运行 Mysql 镜像
docker run -d -p 3310:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=youzc --name mysql01 biarms/mysql

# 配置 mysql 大小写不敏感
vi /home/mysql/conf/mysqld.cnf
# 添加如下语句
[mysqld]
lower_case_table_names=1
```

难点：
1. HopeEdgeOS 开发及运行环境
2. openEuler 开发及运行环境
3. MQTT 消息丢失问题
	引入 QoS 来保障消息的可靠性，在 QoS 等级的选择上
	[EMQ x Broker-消息重传](https://docs.emqx.cn/broker/v4.3/advanced/retransmission.html#%E5%9F%BA%E7%A1%80%E9%85%8D%E7%BD%AE)