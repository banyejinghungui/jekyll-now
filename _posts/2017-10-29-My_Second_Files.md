---
layout: post
title: My Second Blog
---

## 互联项目组
* 健康城市（艾嘉健康）34.100~34.149
* 艾嘉就医 34.150~34.199

## 互联基础架构
> nginx(34.24) -> nginx(100, 150) -> H5(131,181) -> API(104,151) -> 文件服务器（109,159), mysql（148,198），mongodb(147,197)

## 发布顺序

> 注：如果是就医，需要确认ranch是否要同步发布

- [ ] 1. 执行数据库语句------执行svn上sql.dll语句（先执行create.sql创建表格，再执行其他文件）
- [ ] 2. 同步图片 ------将svn上upload文件夹下的所有图片上传到相应部门的资源服务器的对应路径下
- [ ] 3. 执行发布脚本 ------登入正式环境远程发布系统（121.43.185.107），连接api, 输入路径：就医（/soft/deploy_dir/deploy.sh），健康 （/soft/deploy_dir_ih/deploy_ih.sh） ,一键发布程序,h5(/soft/bin/deploy_h5.sh)
- [ ] 4. 图片资源封版------将svn上的包含sql.dll的文件夹封版
- [ ] 5. 图片资源创建新SQL文件夹
- [ ] 6. 程序封版------将svn上server/java下的zoe-ijhealth和zoe-service中的trunk封版，打包到tags中
- [ ] 7. 写总结报告，包括发布时间，发布目的，发布遇到的问题和解决方法。
--------------------------------------------------------------------------------


项目名称 | nginx        |API（Tomcat） | H5（node.js）| 文件服务器
---      |---           |---           |---           |---         
艾嘉就医 | 192.168.1.1  | 192.168.1.3  | 192.168.1.4  | 192.168.1.2
艾嘉健康 | 192.168.1.11 | 192.168.1.13 | 192.168.1.14 | 192.168.1.12
健康城市 | 192.168.1.11 | 192.168.1.23 | 192.168.1.24 | 192.168.1.12


--------------------------------------------------------------------------------
## nginx
### 修改配置文件
> vim /usr/nginx/conf/nginx.conf

### 重启nginx
> /usr/nginx/sbin/nginx -s reload

### 启动nginx
> /usr/nginx/sbin/nginx

--------------------------------------------------------------------------------
## Tomcat
### 配置文件位置
> /tomcat/conf/server.xml
> 注：如果要修改端口，需要修改三个地方，分别是：

 <Server port="8171" shutdown="SHUTDOWN">                   ---> 这个是关闭端口，一般百位数是8，
 
<Connector port="8071" protocol="HTTP/1.1"                        ---> 这个是主要端口

<Connector port="8871" protocol="AJP/1.3" redirectPort="8443" />    --->这个端口一般百位数是1

> 注：如果要加软链接，需要添加以下节点  
<Context docBase="/home/www/zoe-rus-api" path="" reloadable="false">  
    <Resources allowLinking="true"/>  
</Context>

### 脚本位置
> /soft/bin  
> deploy.sh  一键发布  
> display_ports_status.sh 测试心跳  
> replace_daofiles.sh  替换dao文件  
> startupall.sh  所有服务启动  

### 启动tomcat
> cd /soft/tomcat8_doc_8081  
> /bin/startup.sh  

### 查看日志
> tail -f /soft/tomcat8_doc_8081/logs/catalina.out    
> tail -100 /soft/tomcat8_doc_8081/logs/catalina.out    

--------------------------------------------------------------------------------
## nodejs
> 注：新项目需要到 项目下的 module_node 文件夹中运行：npm install  安装插件

### 配置文件位置
> /soft/conf

### 脚本位置
> /soft/bin  
> deploy_ih_h5.sh  一键发布    
> replace_files.sh  替换配置文件   
> startupall_h5.sh  开启所有服务   

### 项目位置
> /home/zoenet-webapp

### PM2命令
#### 启动项目 
> cd /home/zoenet-webapp/zoenetmobile  
> pm2 start "/usr/local/bin/npm" --name "zoe-ops" -- start 或者 pm2 start bin/www -i max --name zoenetmobile 

#### 重启项目
> pm2 restart zoenetmobile

#### 停止项目
> pm2 stop zoenetmobile 

#### 删除项目
> pm2 delete zoenetmobile

#### 查看日志
> pm2 logs zoenetmobiles

#### 查看服务状态
> pm2 list zoenetmobile

--------------------------------------------------------------------------------
## 文件服务器
### 图片存放位置
> /soft/files

### 健康城市
> /soft/files/hc