# logstash+shell实现自动监控并发邮件报告

不管是公司服务器运维还是自搭云主机，经常要登入看看磁盘、CPU、内存和进程运行情况。最近了解了数据处理管道工具logstash，发现它的强大。logstash可以采集各种数据源，转换处理后发给你需要的存储地点，类似于linux中的管道符`|`。logstash支持的数据源很丰富，文件、kafka、邮件，甚至github。我们用shell定期执行查看磁盘、CPU等服务器信息，写入log文件中，再配置logstash监控log文件，服务器信息发送邮件给自己邮箱。

![logstash](https://main.qcloudimg.com/raw/8dcab6bc4b83d84cf4b2008a4df1c8f8.png)

监控shell脚本：

```shell
#!bin/bash
#monitor the server performance 

df -lh > ./monitor.log
top -n 1 >> ./monitor.log
ps -ef | grep java >> ./monitor.log
echo "[collected done at "$(date +%F-%T)"]" >>./monitor.log
```

注意第一句日志写入是用`>`，是重新写入新的日志，之前的日志会清除。后面命令用`>>`追加写入。最后加上写入的时间。加时间戳不仅为了标记清楚日志信息的时间，还为logstash读取文件做好了标记。



监控日志有了，下面logstash要登场了。去[官网](https://www.elastic.co/cn/downloads/logstash)下载最新版本的logstash（特别注意logstash版本很多，不同版本的配置参数可能不同，支持的数据源的版本可能也不同），用下面命令解压在被监控的服务器中。

`tar -zxvf logstash-7.7.1.tar.gz`。进入logstash目录下，执行` bin/logstash -e 'input { stdin { } } output { stdout {} }'`测试一下logstash是否安装成功。在终端输入`hello`，会在屏幕输出封装的message。

```
{
       "message" => "hello",
    "@timestamp" => 2020-06-15T08:19:15.585Z,
      "@version" => "1",
          "host" => "localhost.localdomain"
}
```

安装完成后，我们配置一下`input`和`output`，按照需求输入是log文件，输出是邮件。logstash配置文件如下：

```
input{  file{
		#修改为log文件存放的路径
                path => "XXX/monitor.log"
                #输入的log信息是一行一行的，logstash把一行作为一个事件，
                #而log信息是多行一个事件，或者说执行一次shell脚本才触发一次事件。
                #这就要用到multiline编码，将多行合并为一个事件。
				codec => multiline {
				#以[collected done at正则匹配
				pattern => "^\[collected done at"
				#以满足正则匹配的行作为一次事件的标记
				negate => true
				#每行向下归并至标记的事件中
        what => "next"
					}	
        }
 }
output{
		email	{
		#smtp协议端口25
		port => 25
		#发件邮箱的地址作为用户名，修改为你自己的
		username => "auto_report_server@163.com"
		#邮箱开启smtp时生成的认证码，不是邮箱密码，修改为你自己的
		password => "XXX"
		#发件邮箱地址，修改为你自己的
		from => "auto_report_server@163.com"
		#邮件的标题
		subject => "自动巡检报告"
		#收件邮箱地址，修改为你自己的
		to => "auto_report_server@163.com"
		#发邮件协议
		via => "smtp"
		#smtp服务器域名
		domain => "smtp.163.com"
		#smtp服务器地址
		address => "smtp.163.com"
		#发送的邮件内容，message就是从input导入的内容
		body => "%{message}"
		#开启调试模式，如果遇到发送错误会有提示
		debug => true
		#认证方式
		authentication => "plain"

}	
}
```

输入输出配置文件命名为`test.conf`，在logstash目录下，执行` bin/logstash -f test.conf`。启动logstash过程可能比较慢，等到出现`[2020-06-15T16:19:16,516][INFO ][logstash.agent           ] Successfully started Logstash API endpoint {:port=>9600}`说明启动成功了。执行一下前面写的shell脚本试试。邮件收到了：

![自动发送邮件](https://github.com/yestolife/figurebed/blob/master/img/logstash_auto_email.png?raw=true)
