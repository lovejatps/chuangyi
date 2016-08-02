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

