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

### 预览截图
![Snipaste_2023-12-29_18-27-42](https://github.com/qf0129/jumpman/assets/17614422/a98a31e2-aeb9-4e00-8600-9f44b8b16343)
![Snipaste_2023-12-29_18-29-03](https://github.com/qf0129/jumpman/assets/17614422/471f6806-7dc6-4593-a095-31529201dfff)
![Snipaste_2023-12-29_18-29-51](https://github.com/qf0129/jumpman/assets/17614422/37ac1523-6160-4dc8-9dd5-60046af742f3)
![Snipaste_2023-12-29_18-32-30](https://github.com/qf0129/jumpman/assets/17614422/f51c2327-072d-4d66-9a1f-3c754f4708fc)
![Snipaste_2023-12-29_18-33-03](https://github.com/qf0129/jumpman/assets/17614422/6ac41279-63c1-48d2-b189-ae98a933fbac)
![Snipaste_2023-12-29_18-34-32](https://github.com/qf0129/jumpman/assets/17614422/801fd9d2-5a55-480b-ac01-77fc4f46d44d)


### 安装运行
- 使用docker启动
  ```
  docker run --restart=always -d -p 8080:80 qf0129/jumpman:latest  
  ```  
  浏览器打开`http://ip:8080`即可访问， 默认账号admin，密码admin
  
### 支持rdp、vnc
- 默认支持ssh，如需连接rdp、vnc协议，需要再启动一个Apache的guacd服务
  ```
  docker run --restart=always -d -p 4822:4822 guacamole/guacd
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
  docker run --restart=always -d -p 8080:80 -v /opt/jumpman/config.ini:/config.ini qf0129/jumpman:latest
  ```
