---
layout:     post
title:      "云服务器搭建记录"
subtitle:   "try my best to face problem."
date:       2019-09-30 01:00:00
author:     "Doni Daniel"
header-img: "img/post-bg-rwd.jpg"
catalog: true
tags:
    - 技术
    - 服务器
---


##框架与步骤设计
###物理与开发环境
<table>
	<tr>
		<th>配置种类</th><th>基础配置</th><th colspan=10>详细信息</th>
	</tr><tr align="center">
		<th rowspan=1>物理配置</th><td rowspan=1>腾讯云服务器</td>
		<td>1核</td><td>2G内存</td><td>50G云硬盘</td><td>1Mbps带宽</td>
	</tr><tr align="center">
		<th>操作系统</th><td>Ubuntu Server</td>
		<td>18.04.1 LTS</td><td>GNU/Linux</td><td>4.15.0-54-generic</td><td>x86_64</td>
	</tr>
</table>

###docker框架
<table>
	<tr>
		<td><b>Vue-Nginx 前端管理服务器</b>
			<table>
				<tr align=center><th>前端框架</th><th>静态页面处理</th>
				</tr><tr align="center"><td>Vue</td><td>Nginx</td>
				</tr>
			</table>
		</th>
		<td><b>Apache-Django 后台管理服务器</b>
			<table>
				<tr><th>动态部署</th><th>页面生成</th>
				</tr><tr><td>Apache</td><td>Django</td>
				</tr>
			</table>
		</td>
		<td><b>Mysql-Redis 数据库管理服务器</b>
			<table>
				<tr><th>快速缓存</th><th>主存</th>
				</tr><tr><td>Redis</td><td>Mysql</td>
				</tr>
			</table>
		</td>
	</tr>
</table>

###项目搭建基本框架步骤
####搭建Vue-Nginx服务器
1. 搭建Nginx服务器，实现简单页面浏览
2. 加入vue框架，并简单使用已经有的模板
3. 配置动静分离，将动态请求转至Apache-Django服务器(暂时实现/data的反向代理)

####搭建Apache-Django服务器
1. 搭建Apache服务器，将Django部署

####搭建Mysql-Redis服务器

##基本个人环境配置
###服务器编辑环境配置
|配置内容|执行命令|执行操作内容|备注事项|
|---|---|---|---|
|vim|apt-get install vim|\<确认\>|如无法下载, apt-get update|
||wget  http://files.cnblogs.com/ma6174/vimrc.zip||ma6174的主题|
||unzip vimrc.zip||使用ls -a进行查看.vimrc文件|
||vim ~/.vimrc|打开syntax on<br>修改新标题文件补全|具体配置根据个人情况进行配置|

###服务器用户配置
|配置内容|执行命令|执行操作内容|备注事项|
|:-:|---|---|---|
|用户密码|passwd [username]|<用户原密码><br><新密码><br><重复输入>|密码复杂度要求
|主机名|sudo vim /etc/cloud/cloud.cfg|preserve_hostname: false => preserve_hostname: true|Ubuntu18.04|
||sudo vim /etc/hostname|\<old_hostname\> => \<new_hostname\>|
||sudo vim /etc/hosts|\<old_hostname\> => \<new_hostname\>|
||sudo reboot||可以执行exit,重新连接|


##开发环境配置
###本机配置
|配置内容|执行命令|执行操作内容|备注事项|
|---|---|---|---|
|Anaconda|wget https://repo.anaconda.com/archive/Anaconda3-2018.12-Linux-x86_64.sh||Anaconda3 Linux版本|
||sudo bash Anaconda3-2018.12-Linux-x86_64.sh|<确认><br><安装路径选择><br><配件>||
||source ~/.bashrc||将conda加载入系统路径|
|Django|pip install django||安装django|
|ssl|sudo apt-get install openssl|||
||sudo apt-get install libssl-dev|||
|Docker|curl -fsSL https://get.docker.com \| sh||CentOS与Ubuntu通用|
|sysstat|sudo apt-get install sysstat||系统状态工具包
|vue|sudo apt install nodejs|
||sudo apt install npm|
||sudo npm install -g cnpm --registry=https://registry.npm.taobao.org|
||~~sudo cnpm install -g vue-cli~~||失败|
||npm install -g @vue/cli|

## docneaten 项目内部环境配置
###Anaconda 虚拟环境下配置
|配置内容|执行命令|路径|备注事项|
|---|---|---|---|
|python3|conda create -n docneaten python=3.6.8||
|pip|pip install django||
|django|django-admin startproject docneaten|/home|

###Docker配置
|配置内容|执行命令|执行操作内容|备注事项|
|---|---|---|---|
|Apache|sudo docker run -d --name daniel-web -p 8080:80 -v /home/docneaten/:/home/docneaten/ httpd|创建apache容器|-d 在后台运行|
|Mysql|sudo docker run --name mysql-web -p 3307:3306 -v /home/mysqldata:/home/mysqldata -e MYSQL_ROOT_PASSWORD=6940588666035 -d mysql/mysql-server:5.7|创建mysql容器|
|redis|docker run -d --name redis-web -p 6379:6379 redis --requirepass "6940588666035"|创建redis容器|
|Django|sudo docker pull library/django:1.10.4-python3||
|ssl|docker cp \<ssl\_files\_path\> \<container\_name\>:\<apache2\_path\>/cert/||将ssl的cert文件放到apache目录下
||sudo docker exec -it daniel-web /bin/bash||进入container|
||vim /user/local/apache2/conf/httpd.conf|/ssl 去掉注释(0 x)|添加ssl模块(确认LoadModule \*/mod_ssl.so)|
||vim /user/local/apache2/conf/extra/httpd-ssl.conf|\<VirtualHost 0.0.0.0:443><br>DocumentRoot "/var/www/html"<br>ServerName www.domain.com #填写证书名称<br>ServerAdmin root@domain.com<br>SSLEngine on #启用 SSL 功能<br>SSLCertificateFile /etc/httpd/ssl/2\_www.domain.com.crt #证书文件的路径<br>SSLCertificateKeyFile /etc/httpd/ssl/3\_www.domain.com.key #私钥文件的路径<br>SSLCertificateChainFile /etc/httpd/ssl/1\_root_bundle.crt #证书链文件的路径<br>\</VirtualHost>|
||vim /user/local/apache2/conf/httpd.conf|/rew 去掉注释(0 x)|http跳转https模块(确认LoadModule \*/mod_rewrite.so)|
|||\<Directory "/var/www/html"\> <br># 新增<br>RewriteEngine on<br>RewriteCond %{SERVER_PORT} !\^443$ <br>RewriteRule ^(.*)?$ https://%{SERVER_NAME}%{REQUEST_URI} [L,R]<br>\</Directory>||
||apachectl restart||重启服务？？？|
|docker-compose|sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose|
||chmod +x /usr/local/bin/docker-compose|

####network配置daniel-net
|配置内容|执行命令|执行操作内容|描述|
|---|---|---|---|
|network|docker network create -d bridge --subnet 172.25.0.0/16 daniel-net||创建网络|
||docker network connect daniel-net vue-nginx||将vue-nginx连入网络|
||docker network connect daniel-net daniel-web||将daniel-web连入网络|
||docker network disconnect bridge daniel-web||将daniel-web断开网络|

####daniel-web虚拟机配置\<httpd:latest\>
|配置内容|执行命令|执行操作内容|备注事项|
|---|---|---|---|
|apt-get|apt-get update|apt-get -y upgrade|
||apt-get install wget||安装wget|
||apt-get install vim|mv /home/.../.vimrc ~/|从服务器拷贝.vimrc文件|
||apt-get install gcc make||安装python的前导包
||apt-get install build-essential libncursesw5-dev libssl-dev libgdbm-dev libc6-dev libsqlite3-dev tk-dev||安装python的前导包
||apt-get install libreadline-dev||解决在python3命令行交互模式下上下左右键，屏幕上会打印字符|
||apt-get install libapache2-mod-wsgi-py3||实现apache与django的连接
|python3|tar -zxvf 4.5.15.tar.gz|
||./configure && make && make install|
||ln -s /usr/local/bin/python3 /usr/local/bin/python||制作软连接|
||ln -s /usr/local/bin/pip3 /usr/local/bin/pip||制作软连接|
|pip|pip install --upgrade pip||更新pip|
||pip install django|
||pip install pymysql|
|mod_swgi.so|确保存在该文件|ls /usr/local/apache2/modules/ \| grep swgi.so|没有需要下载|
||vim /usr/local/apache2/conf/httpd.conf|(200G o) LoadModule wsgi_module modules/mod_swgi.so|
||vim /home/docneaten/docneaten/wsgi.py|import sys<br>sys.path.append('/usr/local/lib/python3.6/site-packages')<br>curPath = os.path.abspath(os.path.dirname(__file__))<br>rootPath = os.path.split(curPath)[0]<br>sys.path.append(rootPath)|导入django与该项目的路径|
||vim /home/docneaten/docneaten/setting.py|ALLOWED_HOSTS = ['*']|修改并允许所有主机进行访问|

||vim /etc/apache2/apache2.conf|(G o) ServerName localhost:80<br>WSGIPythonPath /var/www/botmail<br>WSGIPythonHome \<python_path>||
||cd mods-enabled/|
||vim wsgi_module.load|LoadModule wsgi_module /usr/lib/apache2/modules/mod_wsgi.so|


###django-web虚拟机配置\<django-web_image:v2>
|配置内容|执行命令|执行操作内容|备注事项|
|---|---|---|---|
|curl|apt-get install curl|
|git|apt-get install git|
|xz|官网下载||
||tar -jxvf xz.tar.bz2|
||./config --prefix=/opt/gnu/xz|生成Makefile|
||make|
||make install|
||vim ~/.bashrc|export PATH=/opt/gnu/xz/bin/:$PATH$|导入路径|
|pyenv|curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer \| bash|
||curl -O https://www.python.org/ftp/python/3.6.4/Python-3.6.4.tar.xz|
||mkdir ~/.pyenv/cache||
||mv Python-3.6.4.tar.xz ~/.pyenv/cache/|
||vim ~/.pyenv/plugins/python-build/share/python-build/3.6.4|if has\_tar\_xz\_support; then<br>&nbsp;&nbsp;&nbsp;&nbsp;install\_package "Python-3.6.4" "/root/.pyenv/cache/Python-3.6.4.tar.xz" ldflags_dirs standard verify_py36 copy_python_gdb ensurepip<br>else ...|注意安装依赖|
||pyenv install 3.6.4||
|pip|pip install --upgrade pip|更新pip版本
|django|./pip install django==2.1.7||/usr/local/bin|
|mysql|pip install pymysql|
||vim \_\_init\_\_.py|import pymysql<br>pymysql.install\_as\_MySQLdb()|
||vim ~/.pyenv/versions/3.6.4/lib/python3.6/site-packages/django/db/backends/mysql/base.py|#if version < (1, 3, 13):<br>#    raise ImproperlyConfigured('mysqlclient 1.3.13 or newer is required; you have %s.' % Database.\_\_version\_\_)|
||vim ~/.pyenv/versions/3.6.4/lib/python3.6/site-packages/django/db/backends/mysql/operations.py|decode -> encode|
|django|python manage.py makemigrations|
||python manage.py migrate|
||python manage.py runserver|


###vue-nginx搭建
1. 构建vue项目(参考vue命令)，将vue项目通过npm run build 打包
2. cp -r dist ..
3. 创建nginx.conf
	
	```nginx
	worker_processes auto;
	#error_log  logs/error.log;
	#error_log  logs/error.log  notice;
	#error_log  logs/error.log  info;
	#pid        logs/nginx.pid;
	events {
    	worker_connections  1024;
	}
	http {
    	include       mime.types;
    	default_type  application/octet-stream;
    	#log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    	#                  '$status $body_bytes_sent "$http_referer" '
    	#                  '"$http_user_agent" "$http_x_forwarded_for"';
    	#access_log  logs/access.log  main;
    	sendfile        on;
    	#tcp_nopush     on;
    	#keepalive_timeout  0;
   		keepalive_timeout  65;
    	#gzip  on;
    	client_max_body_size   20m;
    	server {
        	listen       80;
        	server_name  www.longdb.com;# 这里换域名
        	#charset koi8-r;
        	#access_log  logs/host.access.log  main;
     	location / {
        	root   /usr/share/nginx/html;
        	index  index.html index.htm;
        	try_files $uri $uri/ /index.html;
        	}
        	#error_page  404              /404.html;
        	# redirect server error pages to the static page /50x.html
        	#
        	error_page   500 502 503 504  /50x.html;
        	location = /50x.html {
            	root   html;
        	}
        	# proxy the PHP scripts to Apache listening on 127.0.0.1:80
        	#
        	#location ~ \.php$ {
        	#    proxy_pass   http://127.0.0.1;
        	#}
        	# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        	#
        	#location ~ \.php$ {
        	#    root           html;
        	#    fastcgi_pass   127.0.0.1:9000;
        	#    fastcgi_index  index.php;
        	#    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        	#    include        fastcgi_params;
        	#}
        	# deny access to .htaccess files, if Apache's document root
        	# concurs with nginx's one
        	#
        	#location ~ /\.ht {
        	#    deny  all;
        	#}
    	}
    	# another virtual host using mix of IP-, name-, and port-based configuration
    	#
    	#server {
    	#    listen       8000;
    	#    listen       somename:8080;
    	#    server_name  somename  alias  another.alias;
    	#    location / {
    	#        root   html;
    	#        index  index.html index.htm;
    	#    }
    	#}
	}
	```
4. 创建Dockerfile
	
	> FROM nginx <br>
	> MAINTAINER longdb <br>
	> \# 将dist文件中的内容复制到 /usr/share/nginx/html/ 这个目录下面<br>
	> COPY dist/  /usr/share/nginx/html/ <br>
	> COPY nginx.conf /etc/nginx/nginx.conf <br>
	> RUN echo 'echo init ok!!'

5. cd .. && docker build -t vue:V1.0.0 .
6. sudo docker run -p 8849:80 -d --name vue-nginx vue:V1.0.0

####以上命令已封装到 deploy.sh

	
##命令使用
###本地命令
<table>
	<tr>
		<th>实现功能</th>
		<th>命令</th>
	</tr>
	<tr>
		<td>登录远程服务器</td>
		<td><code>ssh ${username}@${public_IP}</code></td>
	</tr>
</table>

###服务端命令
####系统命令
|命令|参数|参数意义|命令意义|
|---|---|---|---|
|reboot|||重启服务器|
|htop|||查看系统情况|
|systemctl|start docker||开启docker服务|
||status docker||查看状态|
|apachectl|start\|restart\|stop||开启\|重启\|停止Apache服务|
|curl|icanhazip.com|访问特定网址|获取公网IP|

####Docker命令
**命令均以docker开头，必要时请添加权限提升命令sudo**

<table>
	<tr>
		<th>命令</th><th>参数</th><th>参数意义</th><th>命令意义</th>
	</tr><tr>
		<td>--version</td><td></td><td></td><td>查看docker版本</td>
	</tr><tr>
		<td>run [image_name]</td><td>-dit | -d</td><td>在后台运行</td><td>从镜像启动容器</td>
	</tr><tr>
		<td></td><td>--name [container]</td><td>命名容器</td>
	</tr><tr>
		<td></td><td>--net [net]</td><td>定义容器网络</td>
	</tr><tr>
		<td></td><td>--ip [IP]</td><td>定义容器IP</td>
	</tr><tr>
		<td></td><td>-p [server_port]:[container_port]</td><td>定义端口映射</td>
	</tr><tr>
		<td></td><td>-v [server_path]:[container_path]</td><td>定义挂载映射</td>
	</tr><tr>
		<td>exec [container] [order]</td><td>-it</td><td>以终端方式运行</td><td>在容器中运行命令</td>
	</tr><tr>
		<td>search [image_name]</td><td></td><td></td><td>查找镜像文件</td>
	</tr><tr>
		<td>pull [image]</td><td></td><td></td><td>拉取镜像文件</td>
	</tr><tr>
		<td>rename [container] [new_container]</td><td></td><td></td><td>容器重命名</td>
	</tr><tr>
		<td>ps</td><td>-a</td><td>查看所有状态</td><td>默认为运行的容器</td>
	</tr><tr>
		<td>start | restart | stop | rm [container]</td><td></td><td></td><td>启动 | 重启 | 停止 | 删除 容器</td>
	</tr><tr>
		<td>rmi [image_name]</td><td></td><td></td><td>删除镜像</td>
	</tr><tr>
		<td>commit [container] [new_image]</td><td>-m [commit_msg]</td><td>提交标签</td><td>从容器创建镜像</td>
	</tr><tr>
		<td></td><td>-a [author]</td><td>标记创建者</td>
	</tr>
</table>

#####Docker Management Commands
<table>
	<tr>
		<th>命令</th>
		<th>参数</th><th>子参数</th>
		<th>子参数意义</th><th>参数意义</th>
	</tr>
	<tr>
		<td>network</td><td>ls</td><td></td><td></td><td>列出本机网络</td>
	</tr><tr>
		<td></td><td>create [net]</td><td> -d bridge | host | none</td><td>网络模式</td><td>创建网络</td>
	</tr><tr>
		<td></td><td></td><td>--subnet [IP_head]/[IP_room]</td><td>设定网段</td>
	</tr><tr>
		<td></td><td>connect [net] [container]</td><td></td><td></td><td>将容器加入网络</td>
	</tr><tr>
		<td></td><td>disconnect [net] [container]</td><td></td><td></td><td>将容器断开网络</td>
	</tr><tr>
		<td></td><td>inspect [net]</td><td></td><td></td><td>查看网络内情况</td>
	</tr><tr>
		<td></td><td>rm [net]</td><td></td><td></td><td>删除网络</td>
	</tr>

</table>

###mysql命令
|级别|类别|命令|辅助命令|目的|
|:-:|---|---|---|---|
|系统|登录|mysql -h\<IP\> --port=\<server\_port\> -u\<user\_name> -p||登录数据库|
|权限|用户管理|grant all privileges on \*.\* to '\<user\_name>'@'\<IP\>' identified by '\<password\>' with grant option;|flush privileges;|添加\<user\_name\>允许\<IP\>以\<password\>登录|
|||drop user zhangsan@'[% \<IP\>]';||删除用户|
||数据库|show databases;||打印数据库|
|数据库|表|show tables;||打印表格信息|
|表|查询|show columns from \<table\_name\>；||打印表头信息|
|||select \<key\_name\> \<alias\>, \<key\_name\> from \<table\_name\> where \<condition\>||打印表内信息|

###pyenv命令
|命令|意义|
|---|---|
|pyenv global 3.6.4|切换当前系统环境|

###conda命令
|命令|意义|
|---|---|
|conda create -n \<env\_name\> python=\<version\_num\>|python版本控制|
|conda activate autosortsys|激活环境|
|conda remove -n \<env\_name\> --all|删除环境|

##django命令
|命令|意义|路径要求|
|---|---|---|
|django-admin startproject \<pro\_name\>|创建一个django工程|
|python manage.py runserver \<host\_ip\>:\<host\_port\>|开启简易web程序|工程目录下|
|python manage.py startapp app_name|在工程目录下创建app|工程目录下|

##vue命令
|命令|意义|备注|
|---|---|---|
|sudo vue init webpack <pro_name>|创建vue项目|
|sudo cnpm install|安装项目依赖|在项目目录下|
|sudo npm run build|打包项目，生成dist目录|在项目目录下|


##其他问题
|问题|现象|解决方法|
|---|:-:|---|
|系统Python版本问题|不支持高版本操作|安装高版本并配置系统软连接|
||步骤：|cd Python-3.6.4/<br>./configure --prefix=\<sys_path=\usr\local\Python-3.6.4\><br>make && make install<br>ln -s <new_version_file> <old_version_file=/usr/local/bin/>|


