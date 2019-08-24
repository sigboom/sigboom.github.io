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
|vim|wget  http://files.cnblogs.com/ma6174/vimrc.zip||ma6174的主题|
||unzip vimrc.zip||使用ls -a进行查看.vimrc文件|
||vim ~/.vimrc|打开syntax on<br>修改新标题文件补全|具体配置根据个人情况进行配置|

##开发环境配置
|配置内容|执行命令|执行操作内容|备注事项|
|---|---|---|---|
|Anaconda|wget https://repo.anaconda.com/archive/Anaconda3-2018.12-Linux-x86_64.sh||Anaconda3 Linux版本|
||sudo bash Anaconda3-2018.12-Linux-x86_64.sh|<确认><br><安装路径选择><br><配件>||
||source ~/.bashrc||将conda加载入系统路径|
||conda create -n \<env\_name\> python=\<version\_num\>||python版本控制|
||conda activate autosortsys||激活环境|
|Django|pip install django||安装django|
||django-admin startproject autosortsys||创建工程|
|Docker|curl -fsSL https://get.docker.com \| sh||CentOS与Ubuntu通用|
||systemctl start docker||systemctl status docker查看状态|
||docker --version||19.03.1|
||sudo docker ps [-a]||查看容器基本情况(默认为运行的容器)|
||sudo docker stop \<container\_name\>||停止容器|
||sudo docker rm \<container\_name\>||删除容器|
||sudo docker rmi remove \<image\_name\>||删除镜像|
|Apache|sudo docker run -dit --name \<container\_name\> -p \<server\_port\>:80 -v \<server\_path\>:/usr/local/apache2/htdocs/ httpd:2.4|端口\<server\_port\>-（重定向）->80|设置\<container\_name\>|
|||\<server\_path>-（映射）->"/usr/local/apache2/htdocs/"|使用\<public\_IP\>:\<server\_port\>/\<file_name>访问|

##简单命令
|命令|参数|$1|意义|
|---|---|---|---|
|curl|icanhazip.com|public_IP|获取公网IP|
