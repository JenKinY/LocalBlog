### 0x01. Docker 安装

```shell
# 拉取最新镜像
docker pull emqx/emqx-edge
# 启动容器并映射端口
docker run -d --name emqxedge -p 1883:1883 -p 18083:18083 emqx/emqx-edge
```

#### 故障处理：

1. 需要配置仓库源。Docker 运行 hello-world 报错：

   ```shell
   [root@localhost ~]# docker run hello-world
   Unable to find image 'hello-world:latest' locally
   latest: Pulling from library/hello-world
   256ab8fe8778: Pulling fs layer 
   docker: error pulling image configuration: Get https://production.cloudflare.docker.com/registry-v2/docker/registry/v2/blobs/sha256/a2/a29f45ccde2ac0bde957b1277b1501f471960c8ca49f1588c6c885941640ae60/data?verify=1612123752-GPtkarH7fifvJzzLKmL7IsAMSjQ%3D: x509: certificate has expired or is not yet valid.
   See 'docker run --help'.
   ```

   需要修改 Docker 仓库源：

   ```shell
   # 没有就创建一个daemon.json
   vi /etc/docker/daemon.json 
   
   # 写入下面的代码：
   { 
   "registry-mirrors": ["https://rvof9czj.mirror.aliyuncs.com"] 
   }
   # :wq 保存退出
   
   # 重载这个配置
   systemctl daemon-reload
   # 重启 Docker
   systemctl restart docker
   # 查看是否生效，输出下面的内容则表示成功
   [root@localhost ~]# docker info | grep "Registry Mirrors" -A 1
   Registry Mirrors:
    https://rvof9czj.mirror.aliyuncs.com/
   ```

2. 需要校准系统时间。由于我是在本地树莓派上，因此有时候时间会不准确，所以报了这个错误：

   ```shell
   [root@localhost ~]# docker run hello-world
   Unable to find image 'hello-world:latest' locally
   docker: Error response from daemon: Get https://registry-1.docker.io/v2/: net/http: request canceled (Client.Timeout exceeded while awaiting headers).
   See 'docker run --help'.
   ```

   手动校准时间即可：

   ```shell
   [root@localhost ~]# date -s "20210201 03:29:00"
   Mon Feb  1 03:29:00 CST 2021
   ```

3. 程序启动后，外部无法访问：

   首先可以去排查一下本地防火墙设置，由于此处系统使用的是 iptables

   