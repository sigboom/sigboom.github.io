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
	<tr><th>配置种类</th><th>基础配置</th><th colspan="10">详细信息</th></tr>
	<tr align="center">
		<th rowspan="1">物理配置</th><td rowspan="1">腾讯云服务器</td>
		<td>1核</td><td>2G内存</td><td>50G云硬盘</td><td>1Mbps带宽</td>
	</tr><tr align="center">
		<th>操作系统</th><td>Ubuntu Server</td>
		<td>18.04.1 LTS</td><td>GNU/Linux</td><td>4.15.0-54-generic</td><td>x86_64</td>
	</tr>
</table>

###docker框架
<table>
	<tr align=center><th colspan="1">Public</td><th colspan="4">Docker net [daniel-net]</td></tr>
	<tr><td colspan="2"><b>Vue-Nginx 前端管理服务器</b>
		<table><tr align=center><th>前端框架</th><th>静态页面处理</th></tr><tr align="center"><td>Vue</td><td>Nginx</td></tr></table></td><td><b>Apache-Django 后台管理服务器</b>
		<table><tr><th>动态部署</th><th>页面生成</th></tr><tr><td>Apache</td><td>Django</td></tr></table></td><td colspan="2"><b>数据库服务器</b>
		<table><tr><th>缓存</th><td>Redis</td></tr><tr><th>磁盘</th><td>Mysql</td></tr></table></td></tr>
	<tr align=center><th>BASE</th><td>vue-nginx</td><td>apache-django</td><td>redis</td><td>mysql</td></tr>
	<tr><th colspan="5">各版本及实现功能</th></tr>
	<tr align=center><td>V1.0.1</td><td>支持vue</td><td>支持HCJS</td><td colspan="2">支持本地服务</tr>
	<tr align=center><td rowspan="2">V1.0.2</td><td colspan="2">自动化部署</td></tr>
	<tr align=center><td colspan="2">支持动静分离</td></tr>
	<tr align=center><td>V1.0.3</td><td>支持HTTPS</td><td>链接并调用数据库</td><tr>
</table>

###项目搭建基本框架步骤
####搭建Vue-Nginx服务器
1. 搭建Nginx服务器，实现简单页面浏览
2. 加入vue框架，并简单使用已经有的模板
3. 配置动静分离，将动态请求转至Apache-Django服务器(暂时实现/data的反向代理)
4. 实现并支持HTTPS

####搭建Apache-Django服务器
1. 搭建Apache服务器，将Django部署
2. 配置redis cache, 配置mysql数据库并进行访问

####搭建数据库服务器
#####Redis服务器
1. 搭建redis服务器，并联入内网

#####Mysql服务器
1. 建立初始化数据库，并联入内网

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
||npm install -g @vue/cli|
|svn|sudo apt install subversion||安装svn|

## autosort 项目内部环境配置
###network配置daniel-net
|配置内容|执行命令|执行操作内容|描述|
|---|---|---|---|
|network|docker network create -d bridge --subnet 172.25.0.0/16 daniel-net||创建网络|

###vue-nginx搭建
####封装 deploy.sh
1. 构建vue项目(参考vue命令)，将vue项目通过npm run build 打包
2. cp -r dist ..
3. 创建nginx.conf
4. 创建Dockerfile
5. cd .. && docker build -t vue:V1.0.0 .
6. sudo docker run -p 8849:80 -d --name vue-nginx vue:V1.0.0
7. docker network connect --ip 172.25.0.3 daniel-net vue-nginx
8. 配置文件
	- nginx.conf[http版]
		
		```nginx
		worker_processes auto;
		events {
    		worker_connections  1024;
		}
		http {
    		include       mime.types;
    		default_type  application/octet-stream;
    		sendfile        on;
  				keepalive_timeout  65;
    		client_max_body_size   20m;
    		server {
        		listen       80;
	        	server_name  www.longdb.com;# 这里换域名
	     		location / {
	        		root   /usr/share/nginx/html;
	        		index  index.html index.htm;
	        		try_files $uri $uri/ /index.html;
	        	}
	        	#error_page  404              /404.html;
	        	
	        	error_page   500 502 503 504  /50x.html;
	        	location = /50x.html {
	            	root   html;
	        	}
	       }
		}
		```
	- Dockerfile
	
		```Dockerfile
		FROM vue-nginx:BASE
		MAINTAINER daniel doni
		COPY dist/ /usr/share/nginx/html/
		COPY nginx.conf /etc/nginx/
		COPY Nginx/* /etc/nginx/
		RUN echo 'echo init OK!'
		```

```sh
container=vue-nginx
image=vue

if [ ! -n "$1" ];then
	echo project need a tag
else
	tag=$1
	running=`docker ps | grep $container`
	exist=`docker ps -a | grep $container`
	if [ "$running" ]; then
		docker stop $container && docker rm $container
	elif [ "$exist" ];then
		docker rm $container
	fi
	old=`docker images | grep $image | grep $tag`
	if [ "$old" ];then
		docker rmi $image:$tag
	fi
	docker build -t $image:$tag .
	docker run --name $container -d -p 80:80 -p 443:443 $image:$tag
	docker network connect --ip 172.25.0.3 daniel-net $container
fi
```

####添加SSL模块[http->https]
1. 确认ssl模块

	```sh
	nginx -V | grep with-http_ssl_module
	```
2. 修改nginx.conf[https版]
	
	```nginx
	worker_processes auto;
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
			listen  443 ssl;
	        server_name  sigboom.cn;
			root   /usr/share/nginx/html;
			#charset koi8-r;
	        #access_log  logs/host.access.log  main;
			
			ssl_certificate /etc/nginx/1_sigboom.cn_bundle.crt;
			ssl_certificate_key /etc/nginx/2_sigboom.cn.key;
	        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
			ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
			ssl_prefer_server_ciphers on;
			ssl_session_timeout 5m;
	
			location / {
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
			
			# proxy Python to Apache listening on 172.25.0.3:80
			location /data {
				proxy_pass http://172.25.0.4:80;
			}
	
			location ~* \.(html|htm)$ {
				root /usr/share/nginx/html/static;
				expires 10d; 
			}
	
			location ~* \.(gif|jpg|jpeg|bmp|png|tiff|tif|ico|wmf|js)$ {
				root /usr/share/nginx/html/static;
				expires 10d; 
			}	
		}
	    server {
	        listen       80;
	        server_name  sigboom.cn;# 这里换域名
	        return 301 https://$server_name$request_uri;
	    }
	}
	```

####前端页面搭建

#####使用vue-cli进行搭建框架
1. 开放前端开发端口，使用vue的热更新方便前端开发

	```js
	//修改config/index.js
	dev:{
	host: '0.0.0.0', //所有ip均可访问
	post: 6170, //开放端口
	}
	//修改build/webpack.dev.conf.js
	devServer: {
		disableHostCheck:true, //添加该行
	}
	```
2. 使用vue router进行页面跳转，组件化编程
	<table>
		<tr><th>组件结构</th></tr>
		<tr><td>Header</td></tr>
		<tr><td rowspan="2">Main</td><td rowspan="2">Index</td><td>Sign\_in</td></tr>
		<tr><td>Sign\_up</td></tr>
		<tr><td>Footer</td></tr>
	</table>
3. 安装vuex 登录token
	
	[Vuex配置](https://segmentfault.com/a/1190000015637039)
4. 安装axios与后端交互


###Apache-Django虚拟机配置[django]
1. 封装制作一级镜像[apache-django:BASE | REBASE]
	<table>
		<tr align=center><th colspan="10">基础镜像httpd:latest</th></tr>
		<tr><th>配置内容</th><th>执行命令</th></tr>
		<tr><td>更新apt-get</td><td>apt-get -y upgrade && apt-get update</td></tr>
		<tr><td rowspan="4">python3前导包</td><td>apt-get install -y wgte gcc make build-essential</td></tr>
		<tr><td>apt-get install -y  libncursesw5-dev libssl-dev </td></tr>
		<tr><td>apt-get install -y libgdbm-dev libc6-dev libsqlite3-dev</td></tr>
		<tr><td>apt-get install -y tk-dev libreadline-dev</td></tr>
		<tr align=center><td>apache链接django包</td><td>apt-get install -y libapache2-mod-wsgi-py3</td></tr>
		<tr align=center><td rowspan="6">安装python3</td><td>wget https://www.python.org/ftp/python/3.6.8/Python-3.6.8.tar.xz</td></tr>
		<tr><td>tar xvf Python-3.6.8.tar.xz && cd Python-3.6.8</td></tr>
		<tr><td>./configure && make && make install</td></tr>
		<tr><td>rm -rf Python-3.6.8*</td></tr>
		<tr><td>ln -s /usr/local/bin/python3 /usr/local/bin/python</td></tr>
		<tr><td>ln -s /usr/local/bin/pip3 /usr/local/bin/pip</td></tr>
		<tr><td rowspan="2">pip安装django</td><td>pip install --upgrade pip</td></tr>
		<tr><td>pip install django pymysql django-redis</td></tr>
		<tr><td>获取mod\_wsgi.so</td><td>cp mod_wsgi.so /usr/local/apache2/modules/</td></tr>
		<tr align=center><th colspan="10">生成镜像apache-django:BASE</th></tr>
	</table>
	镜像Dockerfile
	
	```Dockerfile
	FROM httpd:latest
	RUN apt-get -y upgrade && apt-get update
	RUN apt-get install -y wget gcc make build-essential libncursesw5-dev libssl-dev libgdbm-dev libc6-dev libsqlite3-dev tk-dev libreadline-dev libapache2-mod-wsgi-py3
	RUN wget https://www.python.org/ftp/python/3.6.8/Python-3.6.8.tar.xz
	RUN tar xvf Python-3.6.8.tar.xz
	RUN cd Python-3.6.8 && ./configure && make && make install
	RUN rm -rf /usr/local/apache2/Python-3.6.8*
	RUN ln -s /usr/local/bin/python3 /usr/local/bin/python
	RUN ln -s /usr/local/bin/pip3 /usr/local/bin/pip
	RUN pip install --upgrade pip
	RUN pip install django pymysql django-redis cryptography
	COPY mod_wsgi.so /usr/local/apache2/modules/
	```

2. 制作Dockerfile
	
	```Dockerfile
	FROM apache-django:BASE
	MANTAINER "daniel <sigboom@163.com>"
	COPY httpd.conf /usr/local/apache2/conf/
	COPY autosort.conf /usr/local/apache2/conf/extra/
	COPY mysql-change/*.py /usr/local/lib/python3.6/site-packages/django/db/backends/mysql/
	```
	
	1. httpd.conf与autosort.conf配置<br>
		httpd.conf修改内容
		
		```sh
		# 200行添加
		LoadModule wsgi\_module modules/mod_swgi.so
		
		#末尾添加
		ServerName 172.25.0.4:80
		include conf/extra/autosort.conf
		```
		autosort.conf
		
		```sh
		WSGISocketPrefix /var/run/wsgi
		<VirtualHost *:80>
			# ServerName sigboom.cn:80   
			# RewriteEngine On
			# RewriteRule ^/(d-media|media|examples|screenshots)($|(\/(.*))) /app/project/$0 [L]
	
			DocumentRoot /autosort
			DirectoryIndex html/index.html
	
			WSGIScriptAlias / /autosort/autosort/wsgi.py
			<Directory /autosort/autosort>
				<Files wsgi.py>
					Require all granted
				</Files>
				AllowOverride none
				Require all denied
			</Directory>
			Alias /static/ /autosort/static/  
			<Directory /autosort/static>
				Require all granted
			</Directory>
			
			<Directory /autosort/>
				Order Allow,Deny   
				allow From All  
				Options Indexes FollowSymLinks
			</Directory>
		
			WSGIProcessGroup autosort
			WSGIApplicationGroup %{GLOBAL}
			#如果存在虚拟环境
			#	WSGIDaemonProcess autosort python-path=/usr/local/lib:/usr/local/bin 
			WSGIDaemonProcess autosort 
			CustomLog /autosort/logs/access.log combined
			ErrorLog /autosort/logs/error.log
		</VirtualHost>
		```
	2. 修改配置autosort/autosort/wsgi.py
		
		```py
		import sys
		sys.path.append('/usr/local/lib/python3.6/site-packages')
		curPath = os.path.abspath(os.path.dirname(__file__))
		rootPath = os.path.split(curPath)[0]
		sys.path.append(rootPath)
		```
	3. 修改autosort/autosort/setting.py
		
		```py
		# 允许所有主机进行访问
		ALLOWED_HOSTS = ['*']
		```
	4. 创建配置的相关文件夹
		
		```sh
		mkdir static
		mkdir logs
		python manage.py startapp data
		```
	5. mysql-change下文件
		
		```py
		#base.py注释掉下面两行
		if version < (1, 3, 13):
		raise ImproperlyConfigured('mysqlclient 1.3.13 or newer is required; you have %s.' % Database.__version__)
		#operations.py修改decode
		decode -> encode
		```
	
3. 封装deploy.sh
	
	```sh
	container=apache-django
	image=django
	path=/home/daniel/Apache-Django

	if [ ! -n "$1" ];then
		echo project need a tag.
	else
		running=`docker ps | grep $container`
		exist=`docker ps -a | grep $container`
		if [ "$running" ]; then
			docker stop $container && docker rm $container 
		elif [ "$exist" ];then
			docker rm $container
		fi
		old=`docker images | grep $image | grep $1`
		if [ "$old" ];then
			docker rmi $image:$1
		fi
		docker build -t $image:$1 .
		docker run --name $container --net daniel-net --ip 172.25.0.4 -v $path/autosort:/autosort -d $image:$1
	fi
	```

###redis虚拟机配置[redis-data]
1. 配置Dockerfile

	```Dockerfile
	FROM redis:BASE
	```
2. 配置deploy.sh

	```sh
	container=redis
	image=redis-data
	IP=172.25.0.2
	CMD=redis-server

	if [ ! -n "$1" ];then
		echo project need a tag
	else
		running=`docker ps | grep $container`
		exist=`docker ps -a | grep $container`
		if [ "$running" ]; then
			docker stop $container && docker rm $container 
		elif [ "$exist" ];then
			docker rm $container
		fi
		old=`docker images | grep $image | grep $1`
		if [ "$old" ];then
			docker rmi $image:$1
		fi
		docker build -t $image:$1 .
		docker run --name $container --net daniel-net --ip $IP -d $image:$1 $CMD
	fi

	```
	
###mysql虚拟机配置[mysql-data]
1. 配置Dockerfile
	1. 创建初始数据库
		
		```sql
		create database `autosort_db` default character set utf8 collate utf8_general_ci;
		 
		use autosort_db;
		/* 
		DROP TABLE IF EXISTS `user`;
		
		CREATE TABLE `user` (
		 `id` bigint(20) NOT NULL,
		 `email` varchar(255) DEFAULT NULL,
		 `first_name` varchar(255) DEFAULT NULL,
		 `last_name` varchar(255) DEFAULT NULL,
		 `username` varchar(255) DEFAULT NULL,
		 PRIMARY KEY (`id`)
		) ENGINE=InnoDB DEFAULT CHARSET=latin1;
		*/
		```
	2. mysql初始化时将会执行/docker-entrypoint-initdb.d下sql文件
	
	```Dockerfile
	FROM mysql:BASE
	COPY init-sql/*.sql /docker-entrypoint-initdb.d/
	```
2. 配置deploy.sh

	```sh
	container=mysql
	image=mysql-data
	IP="--ip 172.25.0.5"
	Net="--net daniel-net"
	ENV="-e MYSQL_ROOT_PASSWORD=password"
	CMD=""
	
	if [ ! -n "$1" ];then
		echo project need a tag
	else
		running=`docker ps | grep $container`
		exist=`docker ps -a | grep $container`
		if [ "$running" ]; then
			docker stop $container && docker rm $container 
		elif [ "$exist" ];then
			docker rm $container
		fi
		old=`docker images | grep $image | grep $1`
		if [ "$old" ];then
			docker rmi $image:$1
		fi
		docker build -t $image:$1 .
		docker run --name $container $Net $IP -d $ENV $image:$1 $CMD
	fi

	```

##命令使用
###本地命令
<table>
	<tr><th>实现功能</th><th>命令</th></tr>
	<tr><td>登录远程服务器</td><td><code>ssh ${username}@${public_IP}</code></td></tr>
</table>

####Github命令
<table>
	<tr><th colspan="10">命令均以git开头</th></tr>
	<tr><th>命令</th><th>参数</th><th>参数意义</th><th>命令意义</th></tr>
	<tr><td>status</td><td></td><td></td><td>查看本地仓库状态</td></tr>
	<tr><td>add [File]</td><td>--all</td><td>所有文件</td><td>将文件添加至缓存区</td></tr>
	<tr><td>commit</td><td>{-m [tag]}</td><td>添加提交tag</td><td>提交改变给本地仓库</td></tr>
	<tr><td rowspan="2">push</td><td>-f</td><td>强制提交</td><td rowspan="2">将本地仓库推送至Github服务端</td></tr>
	<tr><td>-u [branch]</td><td>推送到指定分支<br>默认[origin master]</td></tr>
	<tr><td>pull</td><td></td><td></td><td>从Github服务端拉取最新版本</td></tr>
	<tr><td rowspan="2">log</td><td>--oneline</td><td>简化默认的输出</td><td>查看历史版本</td></tr>
	<tr><td>--pretty=[mode]</td><td>自定义输出的信息</td><td>git log --pretty=oneline</td></tr>
	<tr><td>reset</td><td>--hard [git-ID]</td><td>重置索引与工作树</td><td>回退到历史版本</td></tr>
</table>

###服务端命令
####系统命令
|命令|参数|参数意义|命令意义|
|---|---|---|---|
|tar|zxvf \| zcvf \| czvp\| -f |解压缩\|压缩|压缩|
|split|-b 45m [tar-file] [split-name]||文件切分|
|reboot|||重启服务器|
|htop|||查看系统情况|
|systemctl|start docker||开启docker服务|
||status docker||查看状态|
|apachectl|start\|restart\|stop||开启\|重启\|停止Apache服务|
|curl|icanhazip.com|访问特定网址|获取公网IP|
|nmap|-Pn -p[port] [IP]||查看端口状态与服务信息|
|nohup|[command] &|将程序以忽略挂起信号的方式运行起来|终端无输出运行命令（ssh退出也不中断）|
|nohup|[command] > [file] 2>&1 &|运行起来后输出到指定文件|后台运行命令（ssh退出也不中断）|
|ctrl + z|||可以将一个正在前台执行的命令放到后台，并且处于暂停状态。
|fg|||将后台任务切换到前台执行
|bg|[num]|将选中的命令调出|将一个在后台暂停的命令，变成在后台继续执行
|jobs|-l|显示所有任务的PID|查看后台运行的状态


####Docker命令
**命令均以docker开头，必要时请添加权限提升命令sudo**

<table>
	<tr><th>命令</th><th>参数</th><th>参数意义</th><th>命令意义</th></tr>
	<tr><td>--version</td><td></td><td></td><td>查看docker版本</td></tr>
	<tr><td>run [image_name]</td><td>-dit | -d</td><td>在后台运行</td><td>从镜像启动容器</td></tr>
	<tr><td></td><td>--name [container]</td><td>命名容器</td></tr>
	<tr><td></td><td>--net [net]</td><td>定义容器网络</td></tr>
	<tr><td></td><td>--ip [IP]</td><td>定义容器IP</td></tr>
	<tr><td></td><td>-p [server_port]:[container_port]</td><td>定义端口映射</td></tr>
	<tr><td></td><td>-v [server_path]:[container_path]</td><td>定义挂载映射</td></tr>
	<tr><td>exec [container] [order]</td><td>-it</td><td>以终端方式运行</td><td>在容器中运行命令</td></tr>
	<tr><td>search [image_name]</td><td></td><td></td><td>查找镜像文件</td></tr>
	<tr><td>pull [image]</td><td></td><td></td><td>拉取镜像文件</td></tr>
	<tr><td>rename [container] [new_container]</td><td></td><td></td><td>容器重命名</td></tr>
	<tr><td>ps</td><td>-a</td><td>查看所有状态</td><td>默认为运行的容器</td></tr>
	<tr><td>start | restart | stop | rm [container]</td><td></td><td></td><td>启动 | 重启 | 停止 | 删除 容器</td></tr>
	<tr><td>rmi [image_name]</td><td></td><td></td><td>删除镜像</td></tr>
	<tr><td>commit [container] [new_image]</td><td>-m [commit_msg]</td><td>提交标签</td><td>从容器创建镜像</td></tr>
	<tr><td></td><td>-a [author]</td><td>标记创建者</td><td></td></tr>
	<tr><td>tag [image_id] [image]:[image_tag]</td><td></td><td></td><td>修改镜像标签</td></tr>
</table>

#####Docker Management Commands
<table>
	<tr><th>命令</th><th>参数</th><th>子参数</th><th>子参数意义</th><th>参数意义</th></tr>
	<tr><td>network</td><td>ls</td><td></td><td></td><td>列出本机网络</td></tr>
	<tr><td></td><td>create [net]</td><td> -d bridge | host | none</td><td>网络模式</td><td>创建网络</td></tr>
	<tr><td></td><td></td><td>--subnet [IP_head]/[IP_room]</td><td>设定网段</td>
	</tr><tr><td></td><td>connect [net] [container]</td><td></td><td></td><td>将容器加入网络</td></tr>
	<tr><td></td><td>disconnect [net] [container]</td><td></td><td></td><td>将容器断开网络</td></tr>
	<tr><td></td><td>inspect [net]</td><td></td><td></td><td>查看网络内情况</td></tr>
	<tr><td></td><td>rm [net]</td><td></td><td></td><td>删除网络</td></tr>
</table>

#####Dockerfile
<table>
	<tr><th>关键词</th><th>意义</th></tr>
	<tr><td>FROM [image]</td><td>从该镜像生成镜像</td></tr>
	<tr><td>MAINTAINER [author [email]]</td><td>为镜像添加作者(email)</td></tr>
	<tr><td>COPY [File][container-path]</td><td>从本机拷贝文件到容器</td></tr>
</table>



<table>

<tr>

</table>

####mysql命令
<table>
	<tr><th>级别</th><th>类别</th><th>命令</th><th>参数</th><th>参数意义</th><th>目的</th></tr>
	<tr><td rowspan="8">系统</td><td rowspan="3">登录</td><td rowspan="3">mysql   </td><td bgcolor="yellow">-u[user] -p</td><td>以[user]登录</td><td rowspan="3">登录数据库</td></tr>
	<tr><td>-h[IP]</td><td>服务器IP默认localhost</td></tr>
	<tr><td>--port=[server_port]</td><td>服务器端口默认3306</td></tr>
	<tr><td rowspan="4">用户管理</td><td colspan="3">create user '[user]' identified by '[passwd]';</td><td>创建用户[user]</td></tr>
	<tr><td colspan="3">grant all privileges on *.* to '[user]'@'[IP]' identified by '[passwd]' with grant option;</td><td rowspan="2">授权[user]允许[IP]以[passwd]管理所有表并刷新</td></tr>
	<tr><td colspan="3">flush privileges;</td></tr>
	<tr><td colspan="3">drop user [user]@[IP];</td><td>删除用户</td></tr>
	<tr><td>数据库</td><td colspan="3">show databases;</td><td>打印数据库列表</td></tr>
	<tr><td rowspan="1">数据库</td><td>表</td><td colspan="3">show tables;</td><td>打印表格信息</td></tr>
	<tr><td rowspan="4">表</td><td rowspan="4">查询</td><td>show columns</td><td bgcolor="yellow">from [table]</td><td></td><td>打印表头信息</td></tr>
	<tr><td colspan="3">desc student</td><td>打印表结构</td></tr>
	<tr><td rowspan="2">select [key] [alias], [key]</td><td bgcolor="yellow">from [table]</td><td></td><td rowspan="2">打印表内信息</td></tr>
	<tr><td>where [condition]</td><td>约束条件</td></tr>
	<tr><th>备注</th><td bgcolor="yellow">必要参数</td></tr>
</table>

####pyenv命令
|命令|意义|
|---|---|
|pyenv global 3.6.4|切换当前系统环境|

####Anaconda命令
|命令|意义|
|---|---|
|conda create -n \<env\_name\> python=\<version\_num\>|python版本控制|
|conda activate autosortsys|激活环境|
|conda remove -n \<env\_name\> --all|删除环境|

####django命令
|命令|意义|路径要求|
|---|---|---|
|django-admin startproject \<pro\_name\>|创建一个django工程|
|python manage.py runserver \<host\_ip\>:\<host\_port\>|开启简易web程序|工程目录下|
|python manage.py startapp app_name|在工程目录下创建app|工程目录下|
|python manage.py makemigrations [app]&&<br> python manage.py migrate [app]|更新[app]数据库模型(默认为all)|工程目录下|

####vue命令
|命令|意义|备注|
|---|---|---|
|vue init webpack <pro_name>|创建vue项目|
|cnpm install|安装项目依赖|在项目目录下|
|npm run build|打包项目，生成dist目录|在项目目录下|


##其他问题
|问题|现象|解决方法|
|---|:-:|---|
|系统Python版本问题|不支持高版本操作|安装高版本并配置系统软连接|
||步骤：|cd Python-3.6.4/<br>./configure --prefix=\<sys_path=\usr\local\Python-3.6.4\><br>make && make install<br>ln -s <new_version_file> <old_version_file=/usr/local/bin/>|
|https服务开启问题|服务器显示服务无效|同时监听80端口与443端口|
|https服务资源加载问题|所有页面内部资源404|https服务页面所有资源的请求形式为https://资源URL，注意修改客户端文件结构与服务端文件结构的对应|
|django加载mysql|运行有错误||
||vim \_\_init\_\_.py|import pymysql<br>pymysql.install\_as\_MySQLdb()|
||vim /usr/local/lib/python3.6/site-packages/django/db/backends/mysql/base.py|#if version < (1, 3, 13):<br>#    raise ImproperlyConfigured('mysqlclient 1.3.13 or newer is required; you have %s.' % Database.\_\_version\_\_)|
||vim /usr/local/lib/python3.6/site-packages/django/db/backends/mysql/operations.py|decode -> encode|
|CSRF verification failed|403错误|axios.interceptors.request.use((config) =>{config.headers['X-Requested-With'] = 'XMLHttpRequest';let regex = /.*csrftoken=([^;.]*).*$/; // 用于从cookie中匹配 csrftoken值config.headers['X-CSRFToken'] = document.cookie.match(regex) === null ? null : document.cookie.match(regex)[1];return config});|
|检查端口被占用|无法监听端口|$ **sudo** netstat -tunpl \| grep 80<br>$ **sudo** kill [pid]|
|nginx配置文件检测|docker搭建nginx服务器时无法启动|docker run -it [images]<br>nginx -t|


