## Doker permission 

**1. 添加一般用户到docker用户组**

使一般用户使用docker时无需sudo权限 [link](https://blog.csdn.net/u013948858/article/details/78429954)

```bash
# 加入用户组
usermod -a -G docker <user-name>

# 查看docker用户组下的成员
cat /etc/group | grep docker

#重启docker服务
sudo systemctl restart docker

# 还是出现权限的问题。使用以下命令
sudo chmod a+rw /var/run/docker.sock
```

