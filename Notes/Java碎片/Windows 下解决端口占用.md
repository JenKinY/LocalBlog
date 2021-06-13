```shell
# 查看端口情况
❯ netstat -ano|findstr  1099
  TCP    127.0.0.1:1098         127.0.0.1:1099         ESTABLISHED     6012
  TCP    127.0.0.1:1099         127.0.0.1:1098         ESTABLISHED     6012

# 终止程序
❯ taskkill -f -pid 6012
成功: 已终止 PID 为 6012 的进程。
```