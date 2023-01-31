## Jumpman
Jumpman是一个简单轻量的堡垒机系统，支持SSH、RDP、VNC等协议。

### 主要功能
- 网页连接主机（SSH、RDP、VNC协议）
- 主机分组管理
- 用户分组管理
- 分组授权
- 用户临时授权
- 审计日志
- 操作回放

### 在线体验
- 地址 <a href="https://jump.1fe.cc" target="_blank">jump.1fe.cc</a>   
- 账号test 密码test

### 安装运行
- 使用docker启动
  ```
  docker run -itd -p 8080:80 qf0129/jumpman:latest  
  ```  
  浏览器打开`http://ip:8080`即可访问， 默认账号admin，密码admin  

- 如需连接rdp、vnc协议，需要再启动一个Apache的guacd服务
  ```
  sudo docker run --restart=always -d -p 4822:4822 guacamole/guacd
  ```
### 配置
- 默认使用sqlite数据库
- 可新建如下配置文件`/opt/jumpman/config.ini`，并复制以下内容修改配置
  ```ini
  [service]
  Addr=:8080
  ; debug|release|test
  RunMode=debug  
  ; debug|info|warning|error|fatal|panic ..
  LogLevel=debug
  ReadTimeOut=60
  WriteTimeOut=60

  [database]
  Db=mysql
  DbUri=jumpman:jumpman@tcp(127.0.0.1:3306)/jumpman?charset=utf8mb4&parseTime=True&loc=Local

  [app]
  JwtSecret=SECRET_KEY
  JwtExpireSecond=86400

  [guacd]
  GuacdHost=172.17.0.1
  GuacdPort=4822
  GuacdSaveDir=data/recording
  ```
- Docker运行时挂载配置文件
  ```
  sudo docker run -itd -p 8080:80 -v /opt/jumpman/config.ini:/config.ini qf0129/jumpman:latest
  ```
