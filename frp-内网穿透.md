## frp 内网穿透 （限SSH 连接）

### 1. 准备

- 内部设备（位于校园网内部）
- 具有公网ip的VPS一台 （阿里云服务器）
- frp 软件

### 2. 服务器端（VPS）

- 下载配套frp软件至服务器端   [this](https://github.com/fatedier/frp/releases)
- frps.ini (使用默认设置即可) bind_port = 7000
- 启动 ` ./frps -c ./frps.ini`

### 3. 内网控制端

- 下载配套frp软件

- frpc.ini:

  ```bash
  # frpc.ini
  [common]
  server_addr = x.x.x.x
  server_port = 7000 # 与bind_port 一致
  
  [ssh]
  type = tcp
  local_ip = 127.0.0.1
  remote_port = 6000
  ```

- 启动` ./frpc -c ./frpc.ini`

### 4. ssh访问内网机器

`ssh -oPort=6000 test@x.x.x.x`   

- 6000 是frpc.ini中的remote_port
- test是内网机器的用户名
- x.x.x.x是VPS的ip地址



**重要：！！！**

***1. 一定要开启VPS的 6000和 7000端口，即 bind_port和 remote_port都要开启，VPS端的。***

***2. 后台运行命令 ` nohup ./frpc -c ./frpc.ini &`***





