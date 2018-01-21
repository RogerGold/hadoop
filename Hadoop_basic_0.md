# Hadoop 基础
## 介绍 [百度百科](https://baike.baidu.com/item/Hadoop)
Hadoop是一个由Apache基金会所开发的分布式系统基础架构。
用户可以在不了解分布式底层细节的情况下，开发分布式程序。充分利用集群的威力进行高速运算和存储。
Hadoop实现了一个分布式文件系统（Hadoop Distributed File System），简称HDFS。HDFS有高容错性的特点，并且设计用来部署在低廉的（low-cost）硬件上；而且它提供高吞吐量（high throughput）来访问应用程序的数据，适合那些有着超大数据集（large data set）的应用程序。HDFS放宽了（relax）POSIX的要求，可以以流的形式访问（streaming access）文件系统中的数据。
Hadoop的框架最核心的设计就是：HDFS和MapReduce。HDFS为海量的数据提供了存储，则MapReduce为海量的数据提供了计算。

## 环境搭建
- Hadoop版本：Apache Hadoop 3.0.0
- Java 版本： Java 8
- 系统： Ubuntu14.04 64bit

### 使用PPA方式安装JDK8

   $ sudo add-apt-repository ppa:webupd8team/java
   $ sudo apt-get update
   $ sudo apt-get install oracle-java8-installer
   $ sudo apt-get install oracle-java8-set-default
    
    检测Java 8是否安装成功

   $ java -version

### 安装SSH
ssh 必须安装并且保证 sshd一直运行，以便用Hadoop 脚本管理远端Hadoop守护进程。

  $ sudo apt-get install ssh 
  $ sudo apt-get install rsync
  
免密码ssh设置,现在确认能否不输入口令就用ssh登录localhost:

  $ ssh localhost

如果不输入口令就无法用ssh登陆localhost，执行下面的命令：

  $ ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa 
  $ cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
  
### 安装Hadoop3.0.0

下载Hadoop3.0.0：[releases.html](http://hadoop.apache.org/releases.html)

解压所下载的Hadoop发行版。编辑 conf/hadoop-env.sh文件，至少需要将JAVA_HOME设置为Java安装根路径。
获取JAVA_HOME：

 $ echo $JAVA_HOME

打开conf/hadoop-env.sh文件，设置JAVA_HOME：


### 伪分布式模式的操作方法
配置conf/core-site.xml:
  
    <configuration>
            <property>
                 <name>hadoop.tmp.dir</name>
                 <value>/home/user/hadoop/tmp</value>
                 <description>Abase for other temporary directories.</description>
            </property>
            <property>
                 <name>fs.defaultFS</name>
                 <value>hdfs://localhost:9000</value>
            </property>
</configuration>

配置conf/hadoop-site.xml:

    <configuration>
            <property>
                 <name>dfs.replication</name>
                 <value>1</value>
            </property>
            <property>
                 <name>dfs.namenode.name.dir</name>
                 <value>/home/user/hadoop/tmp/dfs/name</value>
            </property>
            <property>
                 <name>dfs.datanode.data.dir</name>
                 <value>/home/user/hadoop/tmp/dfs/data</value>
            </property>
    </configuration>
    
### 执行
格式化一个新的分布式文件系统：

  $ bin/hadoop namenode -format

启动Hadoop守护进程：

  $ bin/start-all.sh

Hadoop守护进程的日志写入到 ${HADOOP_LOG_DIR} 目录 (默认是 ${HADOOP_HOME}/logs).
浏览NameNode的网络接口，它的地址默认为：http://localhost:9870/

启动完成后，可以通过命令 jps 来判断是否成功启动，若成功启动则会列出如下进程: “NameNode”、”DataNode” 和 “SecondaryNameNode”（如果 SecondaryNameNode 没有启动，请运行 sbin/stop-dfs.sh 关闭进程，然后再次尝试启动尝试）。
如果没有 NameNode 或 DataNode ，那就是配置不成功，请仔细检查之前步骤，或通过查看启动日志排查原因。

下次启动 hadoop 时，无需进行 NameNode 的初始化，只需要运行 ./sbin/start-dfs.sh 就可以。

停止Hadoop守护进程：

  $ bin/stop-all.sh
