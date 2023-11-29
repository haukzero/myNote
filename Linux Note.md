# Linux Note
## 基础
- 打开终端快捷键: <mark>ctrl+alt+t</mark>
- 终端命令格式: <mark>command [-options] [parameter]</mark> (其中[]内代表可选项,可以省略)
	(多个options可只用一个"-")
- <mark>clear</mark>  //clear :清屏
- 查询命令帮助信息
	1)`<command> --help`
	2)`man <command>`		(man //manual)
- 相对路径,绝对路径

## 文件和目录
### 查看文件和目录
- <mark>ls</mark> //list :查看当前文件夹下的内容
	- 隐藏文件开头有".";显示隐藏文件:`ls -a`
	- 以列表方式显示文件的详细信息:`ls -l`
	- 配合-l以人性化的方式显示文件大小:`-h`
- <mark>pwd</mark>  //print work directory :查看当前所在文件夹
- "<mark>`.`</mark>"代表当前目录,“<mark>`..`</mark>”代表上一级目录
### 通配符
- `*`	:代表任意个数个字符
- `?`	:代表任意一个字符,至少一个
- `[]`	:表示可以匹配字符组的任意一个
- `[abc]`	:匹配a、b、c中的任意一个
- `[a-f]`	:匹配从a到f范围内的任意一个字符
### 切换文件夹:
<mark>cd[目录名]</mark>  //change directory
- `cd / cd~`	:切换回/home/users
- `cd .`		:保持在当前目录不变
- `cd ..`		:切换到上一级目录
- `cd -`		:在最近两次工作目录之间来回切换
### 创建与删除文件与目录
<font color=red>ATTENTION</font>:文件名与目录名不能重名!!!
- <mark>touch [文件名]</mark>  //touch :如果文件名不存在,新建文件;若存在,则修改文件的末次修改时间
- <mark>mkdir [目录名]</mark>  //make directory :创建目录
	- 递归创建目录:`mkdir -p <name_1>/<name_2>/.../`
- <mark>rm [文件名]</mark>  //remove :删除指定的文件名
	- 强制删除,忽略不存在的文件,无需提示:`rm -f`
	- 删除指定目录:`rm -r [目录名]`
		- rm -r *可用于删除所有文件和目录
### 拷贝与移动文件
-  <mark>tree [目录名]</mark>	//tree :以树状图列出文件目录结构
	- 只显示目录:`tree -d`
- <mark>cp [源文件] [目标文件]</mark>	//copy :复制文件或目录
	- 对已存在的目标文件直接覆盖,不提示:`cp -f`
	- 对已存在的目标文件直接覆盖,提示:`cp -i`(强烈建议使用)
	- 若给出的源文件是目录文件,则递归复制:cp -r
- <mark>mv [源文件] [目标文件]</mark>	//move :移动文件或目录/文件或目录重命名
	- 对已存在的目标文件直接覆盖,提示:`cp -i`(强烈建议使用)
### 查看文件内容
- <mark>cat [文件名]</mark>	//concatenate :查看文件内容、创建文件、文件合并、追加文件等功能,一次显示所有内容,适合内容较少的文本文件
	- 对非空输入行编号:`cat -b`(与 nl 命令等价)
	- 对输出的所有行编号:`cat -n`
- <mark>more [文件名]</mark>//more :分屏显示文件内容,一次只显示一页内容,适合内容较多的文本文件
- <mark>grep [搜索文本] [文件名]</mark>//grep :搜索文本文件内容
(当搜索文本在两个单词及以上时,文本应用" "括起来)
	- 显示匹配行和行号:`grep -n`
	- 显示不包含匹配文本的所有行(求反):`grep -v`
	- 忽略大小写:`grep -i`
	- 行首,搜寻以a为开头的行:`^a`
	- 行尾,搜寻以a为结尾的行:`a$`
### 打开文件并写入
<mark> gedit [文件名]</mark>
### C文件的编译
<mark>gcc C文件</mark>
- `gcc -E` :预处理指定的源文件,但不进行编译
- `gcc -S` :执行编译的源文件,但不进行汇编
- `gcc -c` :编译,汇编指定的源文件,但不进行链接
- `gcc -o` :指定生成的文件名  
e.g. gcc hello.c -o hello  (若不加-o,系统默认生成可执行文件a.out)
- 编译后执行:`路径+文件名 (其中"./"不可省略)`
### python文件的运行
<mark>python3 py文件</mark>
### 控制用户对文件的权限
<mark>chmod [mode] [file]</mark>//change mode ,其中mode可设定字符串<mark>[ugoa][+-=][rwx]</mark>
- `u`:owner/user$\quad$`g`:group$\quad$`o`:other$\quad$`a`:all
- `+`:授权$\quad$`-`:撤销权限$\quad$`=`:重设权限
- `r`:可读权限read	$\quad$`w`:可写权限write$\quad$`x`:可执行权限execute
- 八进制语法:	def <mark>r=4;w=2;x=1</mark>	
>
|  #  |    权限    | rwx |
| :-: | :--------: | :-: |
|  7  | 读+写+执行 | rwx |
|  6  |   读+写    | rw- |
|  5  |  读+执行   | r-x |
|  4  |   只读	   | r-  |
|  3  |  写+执行   | -wx |
|  2  |    只写    | -w- |
|  1  |   只执行   | -x  |
|  0  |   无权限   | --- |
		
- 递归修改文件权限:`chmod -R `
### 在终端中显示参数指定的文字
<mark>echo [文本]</mark>//echo,通常和重定向联合使用
- `echo [文本] > [文件名]`
- `echo [文本] >> [文件名]`(也可直接以这两个方法创建含目标文本的文件)
### 重定向
- "`>`":输出,会覆盖文件原有内容
- "`>>`":追加内容到已有文件的末尾
### 管道"|"
- 左边命令的输出作为右边命令的输入
- 常用管道命令:
    - 分屏显示内容:`more`
    - 在命令执行结果的基础上查询指定的文本:`grep`
    
## 远程操作
### 关机与重启
<mark>shutdown [-options] [time]</mark>
- 重启:<mark>shutdown -r</mark>
- 不指定选项和参数时,默认1分钟后关机,可用shutdown -c取消
- time选项:
    - `now`:24小时计时间为当天所设时间
    - `+<num>`表示在`<num>`分钟后关机
    
### 查看/配置网卡信息
- <mark>ifconfig	</mark>	//configure a network interface :查看/配置计算机当前的网卡配置信息 (现在基本不用)
- <mark>ip addr</mark> :ifconfig的取代版本
	- `ip addr show` :显示当前网络接口配置信息
	- `ip addr add` :添加一个新的ip地址
	- `ip addr del` :删除一个已有的ip地址
	- `ip link set` :配置网络接口
- <mark>ping [ip地址/域名]</mark>	//ping :检测到目标ip地址的连接状态是否正常
### 远程登陆和复制文件
有关SSH的配置信息都保存在 /home/user/.ssh 下
- <mark>ssh 用户名@ip</mark>//secure shell :远程连接
	- <mark>sudo</mark>可在连接完成后对连接机做强制性操作
- <mark>scp 用户名@ip</mark>:文件名或路径 用户名@ip:文件名或路径//secure copy :远程复制文件
	- options大致与cp类似
	- 若远程ssh服务器端口不是22,需要使用大写字母`-P`选项指定端口22,即中间插入`-P 22`
### 远程免密登录
- 配置公钥: <mark>ssh-keygen</mark>  ,之后一路回车
- 上传公钥到服务器: <mark>ssh-copy-id -p port user@remote</mark>
### *非对称加密算法
- 本地使用私钥对数据进行加密/解密,服务器用公钥对数据进行加密/解密
- 使用公钥加密的数据,需要使用私钥解密
- 使用私钥加密的数据,需要使用公钥解密
### 给远程机设置别名
在~/.ssh/config中追加内容:
```
Host <alias>
	HostName ip地址
	User 用户名
	Port 22
```

## 文件、用户、组管理
### 修改文件所有者
 <mark>`chown <newowner> (<newgroup> : ) 文件/目录`</mark>
### 组管理
- 修改文件/目录所在组: <mark>`chgrp <newgroup> 文件/目录`</mark>
	- 递归修改目录所在组:`chgrp -R `
- 添加组: <mark>`sudo groupadd 组名`</mark>
- 删除组: <mark>`sudo groupdel 组名`</mark>
- 查看组信息: <mark>`cat /etc/group`</mark>
### 用户管理
- 创建用户：<mark>`sudoku useradd -m -g 组名 用户名`</mark>
     	- `-m`:自动创建用户名目录
     	- `-g`:指定用户名所在组，否则创建组与用户名相同
- 设置密码：<mark>`sudoku passwd 用户名`</mark>
- 删除用户：<mark>`sudoku userdel 用户名`</mark>
	- `-r` 选项可删除对应家目录
- 创建用户后，相关信息保存在<mark>/etc/passwd</mark>中
### passwd文件
- 用户名
- 2)密码(x,表示加密的密码)
- UID(用户标识)
- GID(组标识)
- 用户全名或本地账号
- 家目录
- 登录使用的Shell,即登录后使用的终端命令  (Ubuntu默认为dash)
### 查看用户信息
- 查看用户UID和GID信息: <mark>id [用户名]</mark>
- 查看当前所有登录的用户列表: <mark>who</mark>
- 查看当前登录用户的账户名: <mark>whoami</mark>  //who am i
### 设置用户的主组／附加组和登录 Shell
<mark>usermod</mark>
- 主组:通常在新建用户时指定,在etc/passwd的第4列GID对应的组
	- 修改主组(passwd中的GID): `usermod - g 用户名`
- 附加组:在etc/group中最后一列表示该组的用户列表,用于指定用户的附加权限
	- 修改附加组:`usermod -G 组 用户名`
	    - 设置了用户的附加组之后,需要重新登录才能生效!
	- 修改用户登录Shell: `usermod -s /bin/bash 用户名`
	- 默认使用useradd添加的用户是没有权限使用sudo以root身份执行命令的,可使用命令
```
usermod -G sudo 用户名
```
$\qquad\quad\space\space\space$将用户添加到 sudo 附加组中
### 查看执行命令所在位置
<mark>which 命令名</mark>
(<font color=red>重要！！！</font>)
### *bin与sbin
- <mark>/bin</mark>	$\quad$//binary :二进制执行文件目录,主要用于具体应用
- <mark>/sbin</mark>$\quad$//system binary :系统管理员专用的二进制代码存放目录,主要用于系统管理
- <mark>/usr/bin</mark>	$\quad$	//user comamnds for applications :后期安装的一些软件
- <mark> /usr/sbin</mark>$\quad$//super user commands for applications :超级用户的一些管理软件
### 切换用户
- <mark>`su - 用户名`</mark> :切换用户,并切换目录
	- “`-`”可切换到用户家目录,否则保持位置不变
	- su 不接用户名可切换到root,但不推荐,因为不安全
- <mark>`exit`</mark> :退出当前登录账户

## 系统信息
### 系统信息
- 时间和日期
	- <mark>date</mark> :查看系统时间
	- <mark>cal</mark> :查看日历,选项-y可查看一年的日历
	- <mark>ncal</mark>
- 磁盘和目录空间
	- <mark>df</mark>	//disk free :显示磁盘剩余空间
		- `df -h`
	- <mark>du 目录名</mark>  //disk usage :显示目录下的文件大小
		- `du -h`
- 进程信息
	- <mark>ps [aux]</mark>	//process status :查看进程的详细情况
		- `a` :显示终端上的多余进程,包括其他用户的进程
		- `u` :显示进程的详细状态
		- `x` :显示没有控制终端的进程
	- <mark>top</mark> :动态显示运行中的进程并排序
		- 退出:`q` / `ctrl + c`
	- <mark>kill [-9] 进程代号</mark> :终止指定代号的进行,`-9`选项表示强制终止
### 查找文件
<mark>find [路径] -name "<已知的部分名字或扩展名>"</mark>
- 若省略路径,表示在当前目录下查找

## 软件相关
### 软链接
<mark>ln -s 被链接的源文件 链接文件</mark>
- 无`-s`选项的是硬链接
- 作用:建立文件的软链接,类似windows下的快捷方式
- 两文件占用相同大小的硬盘空间,工作中几乎不会建立文件的硬链接
- 源文件要用绝对路径!!!
### 打包
- 打包文件 : <mark>`tar -cvf 打包文件.tar 被打包的文件/路径`</mark>
<font color=red>ATTENTION</font>:打包不会压缩!!!
- 解包文件 : <mark>`tar -xvf 打包文件.tar`</mark>
- tar常用选项说明:
	- `c`:生成档案文件,创建打包文件
	- `x`:解开档案文件
	- `v`:列出归档解档的详细过程,显示进度
	- `f`:指定档案文件名称,f后面一定是.tar文件,所以必须放在选项最后
### 压缩/解压
- <mark>`gzip`</mark>
	- 压缩tar打包后的文件,扩展名一般用`.tar.gz`
	- tar有选项`-z`可以调用gzip,从而可以方便的实现压缩和解压缩的功能
	- 压缩文件:<mark>`tar -zcvf 打包文件.tar.gz 被压缩的文件/路径`</mark>
	- 解压文件:<mark>`tar -zxvf 打包文件.tar.gz`</mark>
	- 解压缩到指定路径:<mark>`tar -zxvf 打包文件.tar.gz -C 目标路径`</mark>
- <mark>`bzip2`</mark>
	- 压缩tar打包后的文件,扩展名一般用`.tar.bz2`
	- tar有选项`-j`可以调用bzip2
	- bzip2压缩/解压的命令格式与gzip类似
### 软件管理
apt$\quad$ //Advanced Packaging Tool
- 安装: <mark>`sudo apt install 软件包`</mark>
- 卸载: <mark>`sudo apt remove 软件名`</mark>
- 更新已安装的包: <mark>`sudo apt upgrade`</mark>  (所有)
