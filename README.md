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

