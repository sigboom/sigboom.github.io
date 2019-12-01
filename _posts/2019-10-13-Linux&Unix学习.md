---
layout: post
title: Linux&&Unix使用与命令
subtitle: "用好工具，方能提高效率"
date:  2019-10-13 12:00:00
author: "Doni Daniel"
header-img: "img/post-bg-prework.jpg"
catalog: true
tags: 
    - Linux
    - 技术
---

<h1>Linux && Unix</h1>

<table>
	<tr><th>Linux族</th><th>版本号</th></tr>
	<tr><td><a href="#ubuntu">Ubuntu</a></td><td>18.04.3</td></tr>
	<tr><td><a href="#centos">CentOS</a></td></tr>
	<tr><td>RedHat</td></tr>
</table>

<table>
	<tr><th>Unix族</th><th>版本号</th></tr>
	<tr><td><a href="#macos">MacOS Mojave</a></td><td>10.14.6</td></tr>
</table>

<h2>预定想法</h2>
- 总结Linux相关命令与操作，希望能掌握相关技术
- 该文章旨在总结命令与编辑操作知识，并做后期查阅
- 区别各个版本Linux的区别，了解Linux生态

<h2>Linux & Unix命令与操作</h2>

<table>
	<tr><th>类别</th><th>命令</th><th>意义</th><th>e.g.</th><th>效果</th></tr>
	<tr><th rowspan="5" >ENV</th><td>cat /etc/shells</td><td>查看可使用的shell</td><td>$ cat /etc/shells</td><td>[文件打印]</td></tr>
	<tr><td>$SHELL</td><td>当前的shell</td><td>$ echo $SHELL</td><td>/bin/bash</td></tr>
	<tr><td>$0</td><td>当前的shell(部分shell支持)</td><td>$ echo $0</td><td>-bash</td></tr>
	<tr><td>chsh</td><td>修改查看shell</td><td>$ chsh -l<br>$ chsh -s /bin/bash</td><td>[当前可用的shell]<br>[修改自己的shell为bash]</td></tr>
	<tr><td>bash</td><td>启动一个bash</td><td>$ bash</td><td>启动bash一个进程</td></tr>
	<tr><th rowspan="5" >IO</th><td>echo</td><td>在终端输出</td><td>$ echo abc</td><td>abc</td></tr>
	<tr><td>cat</td><td>打印文件内容</td><td>$ cat /etc/passwd</td><td>[文件内容]</td></tr>
	<tr><td>ls</td><td>列出文件夹下内容</td><td>$ ls -l /etc</td><td>[文件夹内容]</td></tr>
	<tr><td>vim</td><td>使用vim编辑器</td><td>$ vim test.cpp</td><td>[创建并编辑test.txt文件]</td></tr>
	<tr><td>nano</td><td>使用nano编辑器</td><td>$ nano test.txt</td><td>[创建并编辑test.txt文件]</td></tr>
	<tr><th rowspan="8" >USER</th><td>groups</td><td>列出有效用户组</td><td>$ groups</td><td>[有效用户组]</td></tr>
	<tr><td>newgrp</td><td>切换有效用户组</td><td>$ newgrp users</td><td>[切换到users用户组]</td></tr>
	<tr><td>passwd</td><td>设置用户密码(默认自己)</td><td>$ passwd [username]</td><td>设置用户密码</td></tr>
	<tr><td>chown</td><td>更改文件所有者与所有组</td><td>$ chown -R [group]:[user] [dir]</td><td>修改文件夹下所有文件所有者及所有组</td></tr>
	<tr><td>chmod</td><td>更改文件权限</td><td>$ chmod 744 [file]</td><td>更改文件属性为rwxr--r--</td></tr>
	<tr><td>finger</td><td>显示用户相关信息</td><td>$ finger [user]</td><td>[Login Name Directory...]</td></tr>
	<tr><td>chfn</td><td>修改finger信息</td><td>$ chfn</td><td>输入密码后直接修改</td></tr>
	<tr><td>id</td><td>UID/GID相关信息</td><td>$ id [user]</td><td>[UID GID group...]</td></tr>
	<tr><th rowspan="2" >TEXT</th><td>head</td><td>输出前几行</td><td>$ head -n 4</td><td>[前四行数据]</td></tr>
	<tr><td>tail</td><td>输出后几行</td><td>$ tail -n 4</td><td>[后四行数据]</td></tr>
	<tr><th rowspan="2">SEARCH</th><td>which</td><td>在$PATH找文件</td><td>$ ls -l $(which bash)</td><td>[文件信息]</td></tr>
	<tr><td>grep</td><td>根据关键词筛选</td><td>$ grep [str] [file1 ..]</td><td>[多文件内容筛选结果]</td></tr>
	<tr><th rowspan="8">JOBS</th><td>ps</td><td>查看进程状态</td><td>$ ps -l<br>$ ps aux</td><td>[当前bash下进程]<br>[系统所有进程]</td></tr>
	<tr><td>bg</td><td>后台暂停的任务变为执行</td><td>$ bg [num]</td><td align=center><b>-</b></td></tr>
	<tr><td>fg</td><td>后台任务放到前台</td><td>$ fg [num]</td><td align=center><b>-</b></td></tr>
	<tr><td>jobs</td><td>查看后台任务</td><td>$ jobs [-lrs]</td><td>[后台任务列表]</td></tr>
	<tr><td>kill</td><td>杀死进程</td><td>$ kill -9</td><td>[无视进程响应杀死进程]</td></tr>
	<tr><td>ctrl + c</td><td>终止前台进程</td><td align=center><b>-</b></td><td align=center><b>-</b></td></tr>
	<tr><td>ctrl + z</td><td>挂起前台进程到后台</td><td align=center><b>-</b></td><td align=center><b>-</b></td></tr>
	<tr><td>nohup</td><td>在终端机脱机工作（不支持bash命令）</td><td>$ nohup [order] &</td><td>[终端机后台运行，并输出到nohup.out]</td></tr>
</table>

<table>
	<tr><th colspan="10">ps 进程状态结构表</th></tr>
	<tr><th>F:进程标志</th><td>4:权限为root</td><td>1:仅可执行fork无法exec</td></tr>
	<tr><th>S:进程状态</th><td>R:正在运行</td><td>S:正在睡眠可被唤醒=</td><td>D:不可被唤醒(可能IO)</td><td>T:停止</td><td>Z:僵尸</td></tr>
	<tr><th>唯一标志</th><td>UID:拥有者</td><td>PID:进程号</td><td>PPID:父进程号</td></tr>
	<tr><th rowspan="2">资源</th><td>C:CPU使用率</td><td>TTY:终端机号</td><td>TIME:用掉CPU的时间</td></tr>
	<tr><td>PRI/NI:优先级</td><td>ADDR/SZ/WCHAN:内存</td></tr>
	<tr><th>CMD:命令</th></tr>
</table>

<a name=ubuntu>
<h2>Ubuntu</h2>
</a>

<h3>写在前面的话</h3>
最先开始Linux就是从这个系统开始的，所以这应该是最亲切的Linux发行版了，相对的Linux相关使用在其他发行版了解的就不多了。以这个发行版为开始，充分了解Linux相关生态

<h3>已使用的命令</h3>

<table>
	<tr><th>类别</th><th>命令</th><th>意义</th><th>e.g.</th><th>效果</th></tr>
	<tr><th rowspan="8">USER</th><td>useradd</td><td>添加新的用户</td><td>$ useradd [username]</td><td align=center><b>-</b></td></tr>
	<tr><td>usermod</td><td>修改用户信息</td><td>$ usermod -G users dmt</td><td>将dmt次要用户组设为users</td></tr>
	<tr><td>chage</td><td>修改密码参数</td><td>$ chage -l [username]</td><td>列出详细密码参数</td></tr>
	<tr><td>userdel</td><td>删除用户</td><td>$ userdel -r [username]</td><td align=center><b>-</b></td></tr>
	<tr><td>groupadd</td><td>新建用户组</td><td>$ groupadd [group]</td><td align=center><b>-</b></td></tr>
	<tr><td>groupmod</td><td>修改用户组信息</td><td>$ groupmod -g 201 -n [name] [group]</td><td>将[group]名设为[name],gid设为201</td></tr>
	<tr><td>groupdel</td><td>删除用户组</td><td>$ groupdel [groupname]</td><td align=center><b>-</b></td></tr>
	<tr><td>gpasswd</td><td>管理用户组管理员</td><td>$ gpasswd [-ad] [user] [group]</td><td align=center><b>-</b></td></tr>
</table>

<h3>文件结构表</h3>

<table>
	<tr><th colspan="10">文件结构表</th></tr>
	<tr><th rowspan="2">文件类别</th><th>文件</th><th colspan="4">文件意义</th><th>项意义</th><th>[分隔符]</th></tr>
	<tr><th colspan="7">文件项</th></tr>
	<tr><th rowspan="8">USER</th><td>/etc/passwd</td><td colspan="4">记录用户账号</td><td>账号</td><td>[:]</td></tr>
	<tr><td>账号名称</td><td>密码</td><td>UID</td><td>GID</td><td>用户信息说明列</td><td>主文件夹</td><td>Shell</td></tr>
	<tr><td>/etc/shadow</td><td colspan="4">记录用户密码</td><td>账号</td><td>[:]</td></tr>
	<tr><td>账号名称</td><td>密码</td><td>最近更改密码的日期</td><td>不可:需要被更动的天数</td></td><td>更改期限前警告:后宽限的天数</td><td>账号失效日期</td><td>保留</td></tr>
	<tr><td>/etc/group</td><td colspan="4">记录GID与组名</td><td>用户组</td><td>[:]</td></tr>
	<tr><td>用户组名称</td><td>密码</td><td>GID</td><td>支持的账号名</td></tr>
	<tr><td>/etc/gshadow</td><td colspan="4">记录用户组密码（创建组管理员）</td><td>用户组</td><td>[:]</td></tr>
	<tr><td>用户名</td><td>密码</td><td>用户组管理员账号</td><td>用户组所属账号</td></tr>
</table>

<h2>CentOS</h2>

<table>
	<tr><th>类别</th><th>特有命令</th><th>意义</th><th>e.g.</th></tr>
	<tr><th rowspan="1">USER</th><td>passwd --stdin [user]</td><td>从标准输入设置密码</td><td>echo password | passwd --stdin user</td></tr>
</table>

<a name=macos>
<h2>MacOS</h2>
</a>

<table>
	<tr><th>类别</th><th>命令</th><th>依赖</th><th>意义</th><th>e.g.</th></tr>
	<tr><th rowspan="1">NET</th><td>mail</td><td>msmtp<br>~/.mailrc<br>~/.msmtp</td><td>使用mail发邮件</td><td>mail -s [title] [user@mail]</td></tr>
</table>