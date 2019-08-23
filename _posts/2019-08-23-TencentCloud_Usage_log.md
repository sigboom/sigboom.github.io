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
|Docker|sudo apt-get install docker|||
|Apache|sudo apt-get install apache2|||
