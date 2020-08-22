# homestead

- 使用 mongodb

```bash

# 配置
# 配置 mongodb
修改 Homestead.yaml，添加 mongodb: true 配置
# 配置端口
send: 27017
to: 27017
vagrant reload --provision

# 启动
sudo service mongodb start
sudo service mongodb restart
sudo service mongodb status

# 打开 shell
mongo
# 查看接口是否打开
netstat -tulpn

# GUI
使用 robo 3t 打开
初始化安装后，无需密码即可连接

```