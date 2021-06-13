[官方文档](https://jupyterhub.readthedocs.io/en/stable/quickstart-docker.html)

```shell
# 拉取并运行镜像
docker run -p 8000:8000 -d --name jupyterhub  -v /opt/jupyterhub/jupyterhub:/srv/jupyterhub -v /opt/jupyterhub/home:/home --restart=always jupyterhub/jupyterhub jupyterhub
# 进入容器
docker exec -it jupyterhub bash
# 创建用户
useradd skyzc
# 设置用户密码
passwd skyzc

# 更新 pip
pip install --upgrade pip
# 更新jupyterhub
pip install --upgrade jupyterhub
# 默认docker没有安装notebook,不执行这条命令，无法使用jupyterhub
pip install notebook --upgrade

# 此时可以用浏览器打开 ip:8000 端口看到登录界面
# 但是会进入会失败，需要配置权限
chmod -R 777 /home
cd /home
mkdir skyzc
chown skyzc:skyzc skyzc -R
# 刷新界面即可正常创建文件

```

