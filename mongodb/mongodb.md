# 基础
默认情况下 MongoDB 启动后会初始化以下两个目录：
数据存储目录：/var/lib/mongodb
日志文件目录：/var/log/mongodb
通过
```shell
sudo mkdir -p /var/lib/mongo
sudo mkdir -p /var/log/mongodb
sudo chown `whoami` /var/lib/mongo     # 设置权限
sudo chown `whoami` /var/log/mongodb   # 设置权限
```
