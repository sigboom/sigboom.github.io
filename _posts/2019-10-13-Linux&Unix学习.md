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

#Linux && Unix
<table>
	<tr><th>Linux族</th><th>版本号</th></tr>
	<tr><td><a href="#ubuntu">Ubuntu</a></td><td>18.04.3</td></tr>
	<tr><td>CentOS</td></tr>
	<tr><td>RedHat</td></tr>
</table>
<table>
	<tr><th>Unix族</th><th>版本号</th></tr>
	<tr><td><a href="#macos">MacOS Mojave</a></td><td>10.14.6</td></tr>
</table>

##预定想法
- 总结Linux相关命令与操作，希望能掌握相关技术
- 该文章旨在总结命令与编辑操作知识，并做后期查阅
- 区别各个版本Linux的区别，了解Linux生态

##Linux & Unix命令与操作
<table>
	<tr><th>类别</th><th>命令</th><th>意义</th><th>e.g.</th><th>效果</th></tr>
	<tr><th rowspan=3 >ENV</th><td>cat /etc/shells</td><td>查看可使用的shell</td><td>$ cat /etc/shells</td><td>[文件打印]</td></tr>
	<tr><td>$SHELL</td><td>当前的shell</td><td>$ echo $SHELL</td><td>/bin/bash</td></tr>
	<tr><td>$0</td><td>当前的shell(部分shell支持)</td><td>$ echo $0</td><td>-bash</td></tr>
	<tr><th rowspan=3 >IO</th><td>echo</td><td>在终端输出</td><td>$ echo abc</td><td>abc</td></tr>
	<tr><td>cat</td><td>打印文件内容</td><td>$ cat /etc/passwd</td><td>[文件内容]</td></tr>
	<tr><td>ls</td><td>列出文件夹下内容</td><td>$ ls -l /etc</td><td>[文件夹内容]</td></tr>
	<tr><th rowspan=3 >USER</th><td>groups</td><td>列出有效用户组</td><td>$ groups</td><td>[有效用户组]</td></tr>
	<tr><td>newgrp</td><td>切换有效用户组</td><td>$ newgrp users</td><td>[切换到users用户组]</td></tr>
	<tr><td>passwd</td><td>设置用户密码(默认自己)</td><td>$ passwd [username]</td><td>设置用户密码</td></tr>
	<tr><th rowspan=2 >TEXT</th><td>head</td><td>输出前几行</td><td>$ head -n 4</td><td>[前四行数据]</td></tr>
	<tr><td>tail</td><td>输出后几行</td><td>$ tail -n 4</td><td>[后四行数据]</td></tr>
</table>

<a name=ubuntu>
##Ubuntu
</a>
###写在前面的话
最先开始Linux就是从这个系统开始的，所以这应该是最亲切的Linux发行版了，相对的Linux相关使用在其他发行版了解的就不多了。以这个发行版为开始，充分了解Linux相关生态

###已使用的命令
<table>
	<tr><th>类别</th><th>命令</th><th>意义</th><th>e.g.</th><th>效果</th></tr>
	<tr><th rowspan=4>USER</th><td>useradd</td><td>添加新的用户</td><td>$ useradd [username]</td><td align=center><b>-</b></td></tr>
	<tr><td>usermod</td><td>修改用户信息</td><td>$ usermod -G users dmt</td><td>将dmt次要用户组设为users</td></tr>
	<tr><td>chage</td><td>修改密码参数</td><td>$ chage -l [username]</td><td>列出详细密码参数</td></tr>
	<tr><td>userdel</td><td>删除用户</td><td>$ userdel -r [username]</td><td align=center><b>-</b></td></tr>
</table>

<table>
	<tr><th colspan=10>文件结构表</th></tr>
	<tr><th rowspan=2>文件类别</th><th>文件</th><th colspan=4>文件意义</th><th>项意义</th><th>[分隔符]</th></tr>
	<tr><th colspan=7>文件项</th></tr>
	<tr><th rowspan=8>USER</th><td>/etc/passwd</td><td colspan=4>记录用户账号</td><td>账号</td><td>[:]</td></tr>
	<tr><td>账号名称</td><td>密码</td><td>UID</td><td>GID</td><td>用户信息说明列</td><td>主文件夹</td><td>Shell</td></tr>
	<tr><td>/etc/shadow</td><td colspan=4>记录用户密码</td><td>账号</td><td>[:]</td></tr>
	<tr><td>账号名称</td><td>密码</td><td>最近更改密码的日期</td><td>不可:需要被更动的天数</td></td><td>更改期限前警告:后宽限的天数</td><td>账号失效日期</td><td>保留</td></tr>
	<tr><td>/etc/group</td><td colspan=4>记录GID与组名</td><td>用户组</td><td>[:]</td></tr>
	<tr><td>用户组名称</td><td>密码</td><td>GID</td><td>支持的账号名</td></tr>
	<tr><td>/etc/gshadow</td><td colspan=4>记录用户组密码（创建组管理员）</td><td>用户组</td><td>[:]</td></tr>
	<tr><td>用户名</td><td>密码</td><td>用户组管理员账号</td><td>用户组所属账号</td></tr>
</table>


<a name=macos>
##MacOS
</a>

