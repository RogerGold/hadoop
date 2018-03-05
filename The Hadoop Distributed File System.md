# The Hadoop Distributed File System
Hadoop分布式文件系统(HDFS)用来存储大的数据集。Hadoop最重要的一个特征是数据分块，通过上千台
服务器来并行的计算，Hadoop集群通过简单的增加服务器来增加计算能力， 存储容量和I/O带宽。

HDFS把文件系统的元数据和应用数据分开存储，把元数据存在一个专用的服务器上，叫做NameNode。
应用数据存在其他服务器上，叫做DataNodes,所有服务器基于TCP协议连接和通信。

HDFS不依赖数据保护机制如RAID（独立冗余磁盘阵列）来让数据持久化，而是复制文件内容放在多个DataNodes上，
在确保数据持久性的同时，也增加了数据的传输带宽。

## NameNode
HDFS的命名空间是文件目录的层次结构，NameNode通过inodes来代表文件目录，inodes记录了很多属性，如权限，修改和
访问时间，命名空间和磁盘空间配额。文件的内容被分成了很多块(block)（通常每个块是128M，这个可以自己配置），文件的每一个内容块
都被单独的复制到多个DataNodes（通常是3个，这个可以自己配置），NameNode维护命名空间树和块与DataNode的映射关系，
每个集群都有一个NameNode，但可以有上千个DataNode和上千个HDFS客户端，每个DataNode可以同时执行多个应用任务。

## Image and Journal
inodes和块定义的命名系统的元数据列表被叫做Image，NameNode让整个命名空间Image留在RAM中，image持续记录保存在NameNode的
本地文件系统中被叫做checkpoint，NameNode对HDFS的修改记录在一个预写式日志（write-ahead log）中被叫做Journal。该日志保存在
本地文件系统中，块的副本位置不是checkpoint的一部分。


每个客户端发起的业务被记录到Journal中，Journal文件会刷新和同步然后确认发送给客户端，NameNode永远不会改变checkpoint。

## DataNode
在启动过程中每个DataNode连接到NameNode并且执行握手，握手的目的是验证命名空间ID和DataNode的版本。如果没有和NameNode的匹配，
DataNode会自动关闭。

命名空间ID在格式化后会分配给HDFS文件系统实例，命名空间ID持久地存储在集群的所有节点上。具有不同命名空间ID的节点将不能加入群集，
从而保护文件系统的完整性。一个新初始化的没有任何命名空间ID的NameNode允许加入集群和接收集群的命名空间ID。

握手成功后，DataNode登记到NameNode，DataNode会持久的保存自己唯一的存储ID，存储ID是DataNode的内部识别标志，通过存储ID，即使
DataNode通过不同的IP和端口重启也能被识别出，当DataNode第一次登记到NameNode时会被分配存储ID并且永远不会改变。

一个DataNode标识自己存储的block副本,通过block报告发送给NameNode，block报告包含了每个block的block ID，block副本大小等，
第一个block报告在DataNode登记后会马上发出，随后的block报告每小时发送一次。

在正常操作期间， DataNode会每隔3秒发一个心跳包给NameNode来报告自己的状态，如果NameNode在10分钟内没有收到DataNode的心跳包，
NameNode就会认为这个DataNode服务不可用，然后会把这个不可用的DataNode上的block副本在别的可用的DataNode上再创建一份。

DataNode发出的心跳包夜包含了总存储容量，使用量，正在传输的数据量，这些数据用于NameNode的内存块分配和负载平衡决策。

NameNode不直接发送请求到DataNode，而是通过回复心跳发送指令到DataNode，指令包括将块复制到其他节点、删除本地块副本、
重新注册和发送最新block报告，关闭节点。

这些命令对于维护整个系统的完整性非常重要，因此即使在大集群上保持心跳频繁也是非常重要的。
NameNode可以处理每秒数以千计的心跳而不影响其它节点的操作


ref: [The Hadoop Distributed File System](http://www.aosabook.org/en/hdfs.html)