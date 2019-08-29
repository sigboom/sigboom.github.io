#学生云服务器使用记录
##基本操作与配置（个人使用 mac iTerm2）
###服务器属性
|云平台|内核|内核版本|操作系统版本|版本号|
|---|---|---|:-:|---|
|腾讯云|GNU/Linux|4.15.0-54-generic x86_64|Ubuntu|18.04.1|
###登录服务器
```sh
ssh <username>@<public_IP>
<初始密码>
```
##基本个人环境配置
###服务器用户配置
|配置内容|执行命令|执行操作内容|备注事项|
|:-:|---|---|---|
|用户密码|passwd [username]|<用户原密码><br><新密码><br><重复输入>|密码复杂度要求
|主机名|sudo vim /etc/cloud/cloud.cfg|preserve_hostname: false => preserve_hostname: true|Ubuntu18.04|
||sudo vim /etc/hostname|\<old_hostname\> => \<new_hostname\>|
||sudo vim /etc/hosts|\<old_hostname\> => \<new_hostname\>|
||sudo reboot||可以执行exit,重新连接|

###服务器编辑环境配置
|配置内容|执行命令|执行操作内容|备注事项|
|---|---|---|---|
|vim|apt-get install vim|\<确认\>|如无法下载, apt-get update|
||wget  http://files.cnblogs.com/ma6174/vimrc.zip||ma6174的主题|
||unzip vimrc.zip||使用ls -a进行查看.vimrc文件|
||vim ~/.vimrc|打开syntax on<br>修改新标题文件补全|具体配置根据个人情况进行配置|

##开发环境配置
###本机配置
|配置内容|执行命令|执行操作内容|备注事项|
|---|---|---|---|
|Anaconda|wget https://repo.anaconda.com/archive/Anaconda3-2018.12-Linux-x86_64.sh||Anaconda3 Linux版本|
||sudo bash Anaconda3-2018.12-Linux-x86_64.sh|<确认><br><安装路径选择><br><配件>||
||source ~/.bashrc||将conda加载入系统路径|
||conda create -n \<env\_name\> python=\<version\_num\>||python版本控制|
||conda activate autosortsys||激活环境|
|Django|pip install django||安装django|
||django-admin startproject autosortsys||创建工程|
|ssl|sudo apt-get install openssl|||
||sudo apt-get libssl-dev|||
|Docker|curl -fsSL https://get.docker.com \| sh||CentOS与Ubuntu通用|
||systemctl start docker||systemctl status docker查看状态|
||docker --version||19.03.1|
||sudo docker ps [-a]||查看容器基本情况(默认为运行的容器)|
||sudo docker restart \<container\_name\>||重启容器|
||sudo docker stop \<container\_name\>||停止容器|
||sudo docker rm \<container\_name\>||删除容器|
||sudo docker rmi remove \<image\_name\>||删除镜像|
||sudo docker exec -it \<container\_name\> /bin/bash||登录container，并进入bash|



###Docker配置
|配置内容|执行命令|执行操作内容|备注事项|
|---|---|---|---|
|Apache|sudo docker run -dit --name \<container\_name\> -p \<server\_port\>:80 -v \<server\_path\>:/usr/local/apache2/htdocs/ httpd:2.4|端口\<server\_port\>-（重定向）->80|设置\<container\_name\>|
|||\<server\_path>-（挂载）->"/usr/local/apache2/htdocs/"|使用\<public\_IP\>:\<server\_port\>/\<file_name>访问|
|Mysql|docker run --name \<container\_name\> -p \<server\_port\>:3306 -v \<server\_path\>:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=\<password\> -d mysql/mysql-server:5.7|端口\<server\_port\>-（重定向）->80|设置密码\<password\>|
|||\<server\_path>-（挂载）->"/var/lib/mysql"|\<server\_path\>必须是绝对路径|
||chcon -Rt svirt\_sandbox\_file\_t \<server\_path\>||读写目录有冲突时使用|
||docker exec -it \<container\_name\> mysql -uroot -p\<password\>|输入sql命令|接入容器|
||mysql -h\<IP\> -p --port=\<server\_port\> -u\<user\_name\> -p|\<password\>|注意添加开启的container端口|
|Django|sudo docker pull library/django:1.10.4-python3||
|~~yum~~|~~sudo apt-get install yum~~||Ubuntu 14.0+不支持yum(未配置源)|
|ssl|docker cp \<ssl\_files\_path\> \<container\_name\>:\<apache2\_path\>/cert/||将ssl的cert文件放到apache目录下
||sudo docker exec -it daniel-web /bin/bash||进入container|
||vim /user/local/apache2/conf/httpd.conf|/ssl 去掉注释(0 x)|添加ssl模块(确认LoadModule \*/mod_ssl.so)|
||vim /user/local/apache2/conf/extra/httpd-ssl.conf|\<VirtualHost 0.0.0.0:443><br>DocumentRoot "/var/www/html"<br>ServerName www.domain.com #填写证书名称<br>ServerAdmin root@domain.com<br>SSLEngine on #启用 SSL 功能<br>SSLCertificateFile /etc/httpd/ssl/2\_www.domain.com.crt #证书文件的路径<br>SSLCertificateKeyFile /etc/httpd/ssl/3\_www.domain.com.key #私钥文件的路径<br>SSLCertificateChainFile /etc/httpd/ssl/1\_root_bundle.crt #证书链文件的路径<br>\</VirtualHost>|
||vim /user/local/apache2/conf/httpd.conf|/rew 去掉注释(0 x)|http跳转https模块(确认LoadModule \*/mod_rewrite.so)|
|||\<Directory "/var/www/html"\> <br># 新增<br>RewriteEngine on<br>RewriteCond %{SERVER_PORT} !\^443$ <br>RewriteRule ^(.*)?$ https://%{SERVER_NAME}%{REQUEST_URI} [L,R]<br>\</Directory>||
||apachectl restart||重启服务？？？|

###django-web虚拟机配置
|配置内容|执行命令|执行操作内容|备注事项|
|---|---|---|---|
|curl|apt-get install curl|
|git|apt-get install git|
|pyenv|curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer \| bash|

##网络命令
|命令|参数|$1|意义|
|---|---|---|---|
|curl|icanhazip.com|public_IP|获取公网IP|
##mysql命令
|级别|类别|命令|辅助命令|目的|
|:-:|---|---|---|---|
|系统|用户|grant all privileges on \*.\* to '\<user\_name>'@'\<IP\>' identified by '\<password\>' with grant option;|flush privileges;|添加\<user\_name\>允许\<IP\>以\<password\>登录|
|||drop user zhangsan@'[% \<IP\>]';||删除用户|
||数据库|show databases;||打印数据库|
|数据库|表|show tables;||打印表格信息|
|表|查询|show columns from \<table\_name\>；||打印表头信息|
|||select \<key\_name\> \<alias\>, \<key\_name\> from \<table\_name\> where \<condition\>||打印表内信息|

##其他问题
|问题|现象|解决方法|
|---|---|---|
|pip版本问题|WARNING: You are using pip version 19.1.1, however version 19.2.1 is available.|python -m pip install --upgrade pip -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com