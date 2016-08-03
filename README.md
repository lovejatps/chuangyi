# chuangyi
ubuntu系统
/etc/apt/sources.list



centos 系统
查看yum库中的Java安装包。
◆输入：yum -y list java* 。
yum -y install java-1.7.0-openjdk* ，以yum库中java-1.7.0为例。 

#配置环境变量


使用vim /etc/profile编辑profile文件
在/etc/profile底部加入如下内容   
JAVA_HOME=/usr/java/jdk1.8.0_25    
PATH=$JAVA_HOME/bin:$PATH   
CLASSPATH=$JAVA_HOME/jre/lib/ext:$JAVA_HOME/lib/tools.jar    
export PATH JAVA_HOME CLASSPATH   

以上，环境变量配置完成。需要注意的是，PATH在配置的时候，一定要把$JAVA_HOME/bin放在前面，不然使用java命令时，系统会找到以前的java，再不往下找了。这样java这个可执行文件运行的目录其实不在$JAVA_HOME/bin下，而在其它目录下，会造成很大的问题。
还要注意，以前其它教程写的CLASSPATH=$JAVA_HOME/lib.tools.jar，不知道以前的版本是怎么样的，现在的版本是没有这样的jar包的。
最后使用source /etc/profile让profile文件立即生效。

#启动Tomcat8
 	
# /usr/local/tomcat/bin/startup.sh //启动tomcat  

Using CATALINA_BASE:   /usr/local/tomcat     
Using CATALINA_HOME:   /usr/local/tomcat      
Using CATALINA_TMPDIR: /usr/local/tomcat/temp      
Using JRE_HOME:        /usr/java/jdk1.8.0_60        
Using CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar       
Tomcat started.   
 
如果出现下面错误：   
Neither the JAVA_HOME nor the JRE_HOME environment variable is defined     
At least one of these environment variable is needed to run this program  
 
则要注意提前设置java路径   
在apache-tomcat-8.0.26/bin/setclasspath.sh中添加一下内容    
 	
export JAVA_HOME=/usr/java/jdk1.8.0_60       
export JRE_HOME=/usr/java/jdk1.8.0_60/jre        
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH        
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH   
 
# 防火墙开放8080端口   
  
增加8080端口到防火墙配置中，执行以下操作：        
 	
# vi /etc/sysconfig/iptables    
   
#增加以下代码        
 	
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT      
 
# 重启防火墙   
 	
# service iptables restart    
 
检验Tomcat8安装运行    
 
通过以下地址查看tomcat是否运行正常：       
http://192.168.11.52:8080/        
看到tomcat系统界面，说明安装成功！
 
停止Tomcat8    
  
 	
#  /usr/local/tomcat/bin/shutdown.sh   //停止tomcat   

# CentOS 6.5 安装 Nginx 

1、安装prce(重定向支持)和openssl(https支持，如果不需要https可以不安装。)      
yum -y install pcre*   
yum -y install openssl*    
CentOS 6.5 我安装的时候是选择的“基本服务器”，默认这两个包都没安装全，所以这两个都运行安装即可    

wget http://nginx.org/download/nginx-1.7.8.tar.gz   

tar -zxvf nginx-1.7.8.tar.gz     
cd nginx-1.7.8   


/configure --prefix=/usr/local/nginx-1.5.1 \    
--with-http_ssl_module --with-http_spdy_module \    
--with-http_stub_status_module --with-pcre   
如果没有error信息，就可以执行下边的安装了：  
make   
make install    
  
#4、开启nginx进程   

/usr/local/nginx-1.7.8/sbin/nginx   
重启或关闭进程：   
/usr/local/nginx-1.7.8/sbin/nginx -s reload   
/usr/local/nginx-1.7.8/sbin/nginx -s stop    
#5、关闭防火墙，或者添加防火墙规则就可以测试了。

service iptables stop     
或者编辑配置文件：  

vi /etc/sysconfig/iptables  
添加这样一条开放80端口的规则后保存：   

-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT   
重启服务即可:   

service iptables restart   
ok，，可以浏览器访问了。   


#用户yum安装版本太低！！！！！！所有我们使用以下手动安装 

摘要
CentOS 7 安装Nginx-1.9.2
「个人记录，肯定有理解错误和理解不透的地方，小白总是有个成长的过程，希望大家多多指教。」

#一、准备工作：

#1、安装必备工具：


$ yum -y install gcc gcc-c++ autoconf automake  

$ yum -y install zlib zlib-devel openssl openssl-devel pcre-devel   

$ sudo yum install -y zlib-devel  

说明：
pcre: 用来作地址重写的功能。   

zlib：nginx 的gzip模块,传输数据打包，省流量（但消耗资源）。   
 
openssl：提供ssl加密协议。   


#2、解压编译：

$ tar -zxvf nginx-1.9.2.tar.gz

$ cd nginx-1.9.2/

$ ./configure \

#上面的参数的作用可以通过--help来查看下文附加1中有提供

$ ./configure --help


  nginx path prefix: "/usr/local/nginx"
  nginx binary file: "/usr/local/nginx/sbin/nginx"
  nginx configuration prefix: "/usr/local/nginx/conf"
  nginx configuration file: "/usr/local/nginx/conf/nginx.conf"
  nginx pid file: "/usr/local/nginx/logs/nginx.pid"
  nginx error log file: "/usr/local/nginx/logs/error.log"
  nginx http access log file: "/usr/local/nginx/logs/access.log"
  nginx http client request body temporary files: "client_body_temp"
  
#3、安装:

$ make && make install

#4、启动：

$ nginx -c /etc/nginx/nginx.conf

