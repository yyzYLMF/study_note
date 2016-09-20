#其他
##Author: Ryan yang
***
####1.查看linux网络状态
	netstat -npl
	n: 用数字形式显示端口号
	p: 显示程序的PID
	l: 表示处于监听状态
	
####2.快捷键
	a. phpStorm: ctrl + shift + n 在工程中查找文件；ctrl + shift + r 全工程中搜索
	b. sublime: F12查找函数定义；ctrl + - 返回上一处
	c. IDEA: 与phpStorm类似，Ctrl + 点击 可以跳转，Ctrl + Alt + <- 返回上一层；ctrl + shift + n 在工程中查找文件；ctrl + shift + r 全工程中搜索
	
####3.测试延迟网站
<http://www.speedtest.net>

####4.socket设置recv超时时间注意
	a. 不用在前面通过fcntl(new_fd ,F_SETFL, O_NONBLOCK)来设置非阻塞
	b. 时间结构要使用 struct timeval timeout={5,0} //5秒 
	
####5.linux对对文本文件按列排序
sort -n -t $'\t' -k 1,1 file_name > log 其中-n是以数字排序，'\t'是以TAB分割，1,1是第一列

####6.一般启动后台程序命令为 nohup /usr/local/php/bin/php -f modc.php olymanager >> /data/webdev/log/nohup/olymanager.log 2>&1 &
	a. 开头的nohup和结尾的&目的是为了让程序像守护进程一样，关闭终端后能继续执行
	b. 2>&1意思是将错误输出重定向到标准输出上，而标准输出又被存入olymanager.log中，从而错误输出和标准输出都被存入olymanager.log中（一般0表示标准输入，1表示标准输出，2表示错误输出）
	c. 一般使用./test > log 只有一个>，而这里使用>>，区别在于使用一个>是重新写入（覆盖掉原来的），而使用两个>>是追加写入，在原来log后面写入