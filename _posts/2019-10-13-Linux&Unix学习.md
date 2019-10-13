---
layout: post
title: Linux&Unix使用与命令
subtitle: "用好工具，方能提高效率"
date:  2019-10-13 12:00:00
author: "Doni Daniel"
header-img: "img/post-bg-prework.jpg"
catalog: true
tags: 
    - Linux
    - 技术
---

#Linux&Unix
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
	<tr><th rowspan=2 >IO</th><td>echo</td><td>在终端输出</td><td>$ echo abc</td><td>abc</td></tr>
	<tr><td>cat</td><td>打印文件内容</td><td>$ cat /etc/passwd</td><td>[文件内容]</td></tr>
	</table>
##Linux ^ Unix命令


<a name=ubuntu>
##Ubuntu
</a>
###写在前面的话
最先开始Linux就是从这个系统开始的，所以这应该是最亲切的Linux发行版了，相对的Linux相关使用在其他发行版了解的就不多了。以这个发行版为开始，充分了解Linux相关生态

###已使用的命令

<a name=macos>
##MacOS
</a>

