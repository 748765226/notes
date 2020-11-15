---
title: Hadoop
date: 2020-11-15 15:15:15
categories: Hadoop
tags: [Hadoop,Java,ssh]
---

# Hadoop教程

<!--more-->

教程不易，请各位大佬麻烦点点Github关注，所需文件和工具可与本人进行联系（评论区或者右下角通信）。此外，建议所有命令建议手敲，本教程根据本人的理解将安装所遇到的坑与心得进行步骤详解，有不对的地方欢迎各位大佬指出！

### Hadoop Java对应版本号

- Apache Hadoop 3.x 版本 现在只支持 Java 8

- Apache Hadoop 从2.7.x 到 2.x 版本支持Java 7 and 8

  

**本次教程使用的是Java 8和hadoop 2.9.2，总共分为三步，第一步Java安装，hadoop单节点安装，ssh免密通信以及hadoop多节点安装**



### 第一步：Java安装教程

**参考：**https://www.jianshu.com/p/f000e05f3512

![image-20201115193755113](F:\Users\Administrator\Desktop\notes\hadoop\JavaSE8.png)





**将下载好的Java 8压缩包传进服务器，推荐使用sftp命令或者软件Xftp**

![image-20201115200442674](F:\Users\Administrator\Desktop\notes\hadoop\Xftp.png)



**进行解压**

![image-20201115200627333](F:\Users\Administrator\Desktop\notes\hadoop\解压java8压缩包.png)



**设置环境变量**

1.在终端输入 `cd` 进入~目录，修改环境变量，输入`vi .bashrc` 

![image-20201115203243201](F:\Users\Administrator\Desktop\notes\hadoop\修改java环境变量.png)

2.点击键盘 `i` 或者`insert` 在末行添加下列

```
export JAVA_HOME=/usr/local/java/jdk1.8.0_191
export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin
#（！！！注意：JAVA_HOME的路径是你实际解压后的JDK的路径，千万别写错了）
```

3.点击键盘`ESC`，输入`:wp `退出并保存文件

![image-20201115203430227](F:\Users\Administrator\Desktop\notes\hadoop\添加java环境变量.png)

4.`source .bashrc` #让设置的变量马上生效，否则得重启生效

![image-20201115203528255](F:\Users\Administrator\Desktop\notes\hadoop\java环境变量生效.png)

**查看Java版本**

![image-20201115203631216](F:\Users\Administrator\Desktop\notes\hadoop\查看java版本.png)



### Hadoop单节点安装教程

**下载hadoop2.9.2**

![image-20201115205552339](F:\Users\Administrator\Desktop\notes\hadoop\hadoop下载.png)

![image-20201115205643091](F:\Users\Administrator\Desktop\notes\hadoop\hadoop2.9.2.png)



**同理，利用Xftp 将下载好的文件上传到服务器**

![image-20201115205851988](F:\Users\Administrator\Desktop\notes\hadoop\上传hadoop.png)



**输入`tar -xzvf hadoop-2.9.2.tar.gz`**

![image-20201115210120128](F:\Users\Administrator\Desktop\notes\hadoop\解压hadoop.png)



**配置hadoop**

1.首先创建namenode和datanode目录

2.分别输入 `mkdir -p hadoop_tmp/hdfs/namenode `

​				   ` mkdir -p hadoop_tmp/hdfs/datanode `

![image-20201115210543729](F:\Users\Administrator\Desktop\notes\hadoop\创建namenode和datanode.png)



**配置hadoop环境变量**

1.终端输入`vi ~/.bashrc`

2.首先修改JAVA_HOME，由于上面配置java环境变量已经设置，所以将其更改一下，这里很关键，一定要操作对

3.插入下列代码配置hadoop环境变量

```
export HADOOP_HOME=/root/hadoop-2.9.2
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib"
```

![image-20201115211406499](F:\Users\Administrator\Desktop\notes\hadoop\hadoop环境变量配置.png)

4.键盘`ESC`输入`:wq`保存退出，并且输入`source ~/.bashrc`使其生效

5.输入`vi etc/hadoop/hadoop-env.sh` 修改JAVA_HOME

`  export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64 `

或者`  export JAVA_HOME=/root/hadoop-2.9.2`

![image-20201115215119206](F:\Users\Administrator\Desktop\notes\hadoop\hadoop.png)

**更新hadoop配置文件（四个文件）**

1.终端进入`cd ~/hadoop-2.9.2`(hadoop解压后的地址) 修改`vi etc/hadoop/core-site.xml`

![image-20201115212003747](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201115212003747.png)

2.插入以下

```
#Add those lines in the configuration section
<property>
  <name>fs.default.name</name>
  <value>hdfs://localhost:9000</value>
</property>
```

![image-20201115212527973](F:\Users\Administrator\Desktop\notes\hadoop\修改core-site.xml.png)

**如果上述vi修改麻烦可以在本地修改后通过xftp传到对应目录下覆盖**

![image-20201115212929054](F:\Users\Administrator\Desktop\notes\hadoop\xftp上传hadoop配置文件.png)

**剩下三个文件同理这里就不一一赘述，贴上需要修改的文件和代码**

第二个文件： hdfs-site.xml  

注：截图红框地址的刚才创建的namenode和datanode的地址，记得修改成自己的

```
#Add those lines in the configuration section
<property>
      <name>dfs.replication</name>
      <value>1</value>
 </property>
 <property>
      <name>dfs.namenode.name.dir</name>
      <value>file:/home/hduser/hadoop_tmp/hdfs/namenode</value>
 </property>
 <property>
      <name>dfs.datanode.data.dir</name>
      <value>file:/home/hduser/hadoop_tmp/hdfs/datanode</value>
 </property>
```

![image-20201115214025444](F:\Users\Administrator\Desktop\notes\hadoop\hdfs-site.png)

第三个文件： yarn-site.xml 

```
#Add those lines in the configuration section
<property>
      <name>yarn.nodemanager.aux-services</name>
      <value>mapreduce_shuffle</value>
</property>
<property>
      <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
      <value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>

```

![image-20201115213656328](F:\Users\Administrator\Desktop\notes\hadoop\yarn-site.png)

第四个文件： mapred-site.xml 

这个文件需要先进入`cd hadoop-2.9.2`执行`cp etc/hadoop/mapred-site.xml.template mapred-site.xml `

然后添加下列代码

```
#Add those lines in the configuration section
<property>
      <name>mapreduce.framework.name</name>
      <value>yarn</value>
</property>
```

![image-20201115213738597](F:\Users\Administrator\Desktop\notes\hadoop\mapred-site.png)



**禁用**   

**PS：为何要禁用**（参考wiki：https://cwiki.apache.org/confluence/display/HADOOP2/HadoopIPv6）

**输入 `sudo vi /etc/sysctl.conf  `插入下列**

```
#Append the following lines at the end of the file
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
```

![image-20201115205232721](F:\Users\Administrator\Desktop\notes\hadoop\禁用IPv6.png)



**格式化namenode**

终端输入 `hdfs namenode -format `

![image-20201115222214199](F:\Users\Administrator\Desktop\notes\hadoop\格式化namenode.png)



**开启hadoop分布式文件系统**

终端输入` start-dfs.sh `

如果没有设置ssh 密钥对的话要输入密码登录，想要免密登录见下个部分

![image-20201115222516500](F:\Users\Administrator\Desktop\notes\hadoop\hdfs.png)



**开启mapreduce**

终端输入` start-yarn.sh  `



**开启作业历史服务**

终端输入` mr-jobhistory-daemon.sh start historyserver `



**验证是否成功**（成功看到7个节点开启才算成功，如果某个节点或者任务没开启请移步日志）

![image-20201115224056805](F:\Users\Administrator\Desktop\notes\hadoop\单节点验证.png)





**实现免密登录**

1.首先在master服务器上输入`ssh-keygen -t rsa`生成SSH秘钥对

![image-20201115225957720](F:\Users\Administrator\Desktop\notes\hadoop\masterssh.png)

2.还是在master上给.ssh文件夹设置权限，否则等下使用ssh连接时报错

终端输入`chmod 600 ~/.ssh`

3.讲master的公钥复制到slave3上

终端执行`scp .ssh/id_rsa.pub root@47.97.9.116 /root/.ssh/authorized_keys`

需要输入slave3的登录密码

![image-20201115230808919](F:\Users\Administrator\Desktop\notes\hadoop\ssh1.png)

至此，执行ssh slave3的公网ip比如（ssh 47.8.9.0）就可以实现免密登录到slave3了

4.在master服务器上修改hosts以及hostname

终端执行`hostname master`后重新连接服务器就可以看到主机名的变化

终端修改/etc/hosts 

![image-20201115231615671](F:\Users\Administrator\Desktop\notes\hadoop\hosts.png)

5.执行ssh slave3或者ssh slave3的公网ip都可以实现从master免密登录到slave3（归功于上一步）

