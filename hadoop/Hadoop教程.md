---
title: Hadoop
date: 2020-11-15 15:15:15
categories: Hadoop
tags: [Hadoop,Java,ssh]
---

# Hadoop教程

<!--more-->

本教程只记录个人安装Hadoop时的心得以及所踩的坑，若有不对之处欢迎指出交流！

本教程所需文件和工具教程都放到我的[repo](https://github.com/748765226/hadoop)，有任何问题随时欢迎在评论区留言以及右下角即时通信。

**注意！！！需要hadoop和jdk安装包的话，需要点进去具体资源进行下载，直接下载我的分支会出现问题。**


### 问题总结：

1.Hadoop 2.x namenode界面访问端口默认是50070

   Hadoop 3.x namenode界面访问端口默认是9870

2.关于JAVA的环境变量以及`JAVA_HOME=xxx`一定要配置对，这里是解压后的地址。如果是通过apt安装的，则需要自行去找一下安装目录，一般默认是`/usr/local/jvm/xxx`。

3.Hadoop的环境变量也要根据自己的安装版本和安装目录来修改。

4.Hadoop的四个主要配置文件代码格式要严格按照xml文件格式，并且也要根据具体的Hadoop版本和安装目录进行修改。

5.如果上述情况避开了，还遇到namenode或者datanode打不开的问题，请大家移步安装目录下logs找到对应节点的日志下拉到最底下，找到错误自行百度或者评论区留言。

**！！！终极大招，不太推荐使用，除非实在解决不了配置问题，又不想重搭环境，并且可以承担之前跑的数据丢失的情况的话，可以执行 `rm -rf hadoop_tmp` 和 `rm -rf logs`，hadoop_tmp为第二部分第二小节创建的目录。**



### Hadoop Java对应版本号

- Apache Hadoop 3.x 版本 现在只支持 Java 8

- Apache Hadoop 从2.7.x 到 2.x 版本支持Java 7 and 8

  

### 本次教程使用的是Java 8和hadoop 2.9.2，总共分为四个部分，Java安装，hadoop单节点安装，ssh免密通信以及hadoop多节点安装



### 第一部分：Java安装教程

**下载JDK 8**        

[JDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)

如果上面链接失效或者无法登陆，请移步[JDK 8](https://media.githubusercontent.com/media/748765226/hadoop/main/jdk-8u271-linux-x64.tar.gz)下载

 ![](https://i.loli.net/2020/11/16/YoRk2V7CP5UXEDz.png) 



**将下载好的Java 8压缩包传进服务器，推荐使用sftp命令或者软件Xftp**

![](https://i.loli.net/2020/11/16/MY5UQVd8ykNuvLs.png) 



**进行解压**

![](https://i.loli.net/2020/11/16/KP27owXC6UmWJZf.png) 



**设置环境变量**

1.在终端输入 `cd` 进入~目录，修改环境变量，输入`vi .bashrc` 

![](https://i.loli.net/2020/11/16/jI7VPwJuT2DLCmt.png) 

2.点击键盘 `i` 或者`insert` 在末行添加下列

```
export JAVA_HOME=/root/hadoop-2.9.2
export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin
#（！！！注意：JAVA_HOME的路径是你实际解压后的JDK的路径，千万别写错了）
```

3.点击键盘`ESC`，输入`:wp `退出并保存文件

![](https://i.loli.net/2020/11/16/7uyQSRsGNYaoAXp.png) 

4.终端输入`source .bashrc` #让设置的变量马上生效，否则得重启生效

 ![](https://i.loli.net/2020/11/16/ORxdSFcW2sYIhp6.png) 

**查看Java版本**

![](https://i.loli.net/2020/11/16/Yi6T3NJOWfthgIc.png) 



### 第二部分：Hadoop单节点安装教程

**下载hadoop2.9.2**

 [Hadoop下载](http://ftp.cuhk.edu.hk/pub/packages/apache.org/hadoop/common/)

![](https://i.loli.net/2020/11/16/HthS1TafLkOlBze.png) 

![](https://i.loli.net/2020/11/16/O4Q8NaeYPgbkK5p.png) 



**同理，利用Xftp 将下载好的文件上传到服务器**

![](https://i.loli.net/2020/11/16/kNra1E6F4tsXUjq.png) 



**输入`tar -xzvf hadoop-2.9.2.tar.gz`**

 ![](https://i.loli.net/2020/11/16/A2mcVxaRQe76sGC.png) 



**配置hadoop**

1.首先创建namenode和datanode目录

2.分别输入 `mkdir -p hadoop_tmp/hdfs/namenode `

​				   ` mkdir -p hadoop_tmp/hdfs/datanode `

![img](https://i.loli.net/2020/11/16/6uUMZC1VpRyLlI8.png) 



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

![](https://i.loli.net/2020/11/16/sJwmpYCGlt3TEfd.png) 

4.键盘`ESC`输入`:wq`保存退出，并且输入`source ~/.bashrc`使其生效

5.输入`vi etc/hadoop/hadoop-env.sh` 修改JAVA_HOME

`  export JAVA_HOME=/root/hadoop-2.9.2`  

#（！！！注意：JAVA_HOME的路径是你实际解压后的JDK的路径，千万别写错了）

![](https://i.loli.net/2020/11/16/2nyCsK4vkjqYpHI.png) 



**更新hadoop配置文件（四个文件）**

1.终端进入`cd ~/hadoop-2.9.2`(hadoop解压后的地址) 修改`vi etc/hadoop/core-site.xml`

![](https://i.loli.net/2020/11/16/aKDMLRBiNZCzf9J.png) 

2.插入下面代码

```
#Add those lines in the configuration section
<property>
  <name>fs.default.name</name>
  <value>hdfs://localhost:9000</value>
</property>
```

![](https://i.loli.net/2020/11/16/4AkaQDSVRPoGrvY.png) 

**如果上述vi修改麻烦可以在本地修改后通过xftp传到对应目录下覆盖**

![](https://i.loli.net/2020/11/16/bqVUe4ioChr3vI1.png) 

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

![](https://i.loli.net/2020/11/16/bl4APa1xcqD8LSn.png) 

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

![](https://i.loli.net/2020/11/16/pikKlZ3sdJXmaFb.png) 

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

 ![](https://s3.ax1x.com/2020/11/16/Dk1R3T.png) 



**禁用**   

**PS：为何要禁用**（参考[wiki](https://cwiki.apache.org/confluence/display/HADOOP2/HadoopIPv6)）

**输入 `sudo vi /etc/sysctl.conf  `插入下列**

```
#Append the following lines at the end of the file
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
```

![](https://i.loli.net/2020/11/16/fZW1xQ3bpSMlsuE.png) 



**格式化namenode**

终端输入 `hdfs namenode -format `

 ![](https://s3.ax1x.com/2020/11/16/Dk3nVs.png) 



**开启hadoop分布式文件系统**

终端输入` start-dfs.sh `

如果没有设置ssh 密钥对的话要输入密码登录，想要免密登录见下个部分 ![](https://s3.ax1x.com/2020/11/16/Dk3ZrQ.png) 



**开启mapreduce**

终端输入` start-yarn.sh  `



**开启作业历史服务**

终端输入` mr-jobhistory-daemon.sh start historyserver `



**验证是否成功**（成功看到7个节点开启才算成功，如果某个节点或者任务没开启请移步日志）

 ![](https://s3.ax1x.com/2020/11/16/Dk3AxS.png) 





### 第三部分：免密登录

1.首先在master服务器上输入`ssh-keygen -t rsa`生成SSH秘钥对

 ![](https://s3.ax1x.com/2020/11/16/Dk3VKg.png) 

2.还是在master上给.ssh文件夹设置权限，否则等下使用ssh连接时报错

终端输入`chmod 600 ~/.ssh`

3.讲master的公钥复制到slave3上

终端执行`scp .ssh/id_rsa.pub root@47.97.9.116 /root/.ssh/authorized_keys`

需要输入slave3的登录密码 ![](https://s3.ax1x.com/2020/11/16/Dk3ebj.png) 

至此，执行ssh slave3的公网ip比如（ssh 47.8.9.0）就可以实现免密登录到slave3了

4.在master服务器上修改hosts以及hostname

终端执行`hostname master`后重新连接服务器就可以看到主机名的变化

终端修改/etc/hosts 

 ![](https://s3.ax1x.com/2020/11/16/Dk3uan.png) 

5.执行ssh slave3或者ssh slave3的公网ip都可以实现从master免密登录到slave3（归功于上一步）



### 第四部分：Hadoop多节点搭建

**多节点的搭建基本上是建立在单节点的基础之上。主要修改hadoop的四个主要配置文件和以及ssh免密登录**



在master节点上操作

1.修改 core-site.xml 输入 `vi etc/hadoop/core-site.xml` 更新localhost为master

```
<configuration>   
	<property>     
		<name>fs.default.name</name>     
		<value>hdfs://master:9000</value>   
	</property> 
</configuration> 
```

![image-20201122191212389](F:\Users\Administrator\Desktop\notes\hadoop\1.png)



2.修改 hdfs-site.xml  输入`vi etc/hadoop/hdfs-site.xml` 更新`dfs.replication`的value值为4

```

```

![image-20201122191140199](F:\Users\Administrator\Desktop\notes\hadoop\2.png)



3.修改 yarn-site.xml  输入`vi etc/hadoop/yarn-site.xml` 添加下列

```
<property>       
    <name>yarn.resourcemanager.resource-tracker.address</name>
    <value>master:8025</value>        
</property>  
<property>       
    <name>yarn.resourcemanager.scheduler.address</name>       
    <value>master:8035</value> 
</property>       
<property>       
    <name>yarn.resourcemanager.address</name>       
    <value>master:8050</value> 
</property> 
<property>             
	<name>yarn.nodemanager.resource.memory-mb</name>             
	<value>2048</value>      
</property> 
```

![image-20201122191440003](F:\Users\Administrator\Desktop\notes\hadoop\3.png)



4.修改 mapred-site.xml 修改`vi etc/hadoop/mapred-site.xml`添加下列

```
<property>     
    <name>mapreduce.job.tracker</name>     
    <value>master:5431</value> 
</property> 
<property>             
	<name>mapred.framework.name</name>             
	<value>yarn</value>      
</property>
```

![image-20201122191639638](F:\Users\Administrator\Desktop\notes\hadoop\4.png)



5.修改 masters 输入`vi etc/hadoop/masters` 添加 master

![image-20201122191655945](F:\Users\Administrator\Desktop\notes\hadoop\5.png)



6.修改 slaves `vi etc/hadoop/slaves`   添加 slave1 slave2 slave3

![image-20201122191706661](F:\Users\Administrator\Desktop\notes\hadoop\6.png)



7.修改主机名 `hostnamectl sethostname master` 注：在slave节点就对应改成slave 1，slave2，slave3

​	之后重新ssh登录就可以看到了



8.修改/etc/hosts 输入`vi /etc/hosts`    添加如下 

```
xxxx  master # xxxx对应master的内网ip
xxxx  slave1 # slave1的内网ip
xxxx  slave2 # slave2的内网ip
xxxx  slave3 # slave3的内网ip
```

![image-20201122192326785](F:\Users\Administrator\Desktop\notes\hadoop\7.png)

**！！！这里有个大坑，这里的ip一定要用内网，别问为什么，并且要将原来的内网ip注释掉**



9.生成ssh密钥对，输入`ssh-keygen -t rsa` 如果之前执行过可以忽略

接着执行 `cp ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys`



10.执行`rm -rf hadoop_tmp`将之前创建的namenode和datanode删除

至此，master上的修改已经全部结束。接下来部署从节点的配置，下面有两种方式：

第一种是将master节点保存成镜像，然后再利用此镜像创建三个slave实例（推荐此方式，操作看截图）

![image-20201122192037164](F:\Users\Administrator\Desktop\notes\hadoop\8.png)

![image-20201122192047130](F:\Users\Administrator\Desktop\notes\hadoop\9.png)

![image-20201122192054381](F:\Users\Administrator\Desktop\notes\hadoop\10.png)



第二种，从头分别搭建三个单节点（略过）



11.1如果master节点和slave节点已经全部创建成功（第一种方式创建），则在master节点依次输入

`hdfs namenode -format`

`start-dfs.sh`

`start-yarn.sh`



11.2如果master节点和slave节点已经全部创建成功（第二种方式创建），则需要将master生成的公钥信息`~/.ssh/authorized_keys`分别复制到三个从节点的`~/.ssh/authorized_keys`

执行`scp ~/.ssh/id_rsa.pub root@slave的公网ip:~/.ssh/authorized_keys` 参考第三部分免密登录



12.最后在master节点和slave节点上分别输入jps，查看结果。下图为正确结果，如果缺少则配置错误

![image-20201122192500788](F:\Users\Administrator\Desktop\notes\hadoop\11.png)



### 第五部分：词频统计(以下操作都在master节点进行)

1.输入`wget https://github.com/hupili/agile-ir/raw/master/data/Shakespeare.tar.gz`下载小说集

如果下载太慢，可以[下载](https://github.com/hupili/agile-ir/raw/master/data/Shakespeare.tar.gz)到本地，然后通过xftp传进去服务器



2.输入`tar-zxvf Shakespeare.tar.gz`



3.上传文件到hadoop文件系统上

执行`hadoop dfs -copyFromLocal /root/hadoop-2.9.2/data /data`

本条指令执行将本地/root/hadoop-2.9.2/下的data 复制到hadoop文件系统的 根目录/下

![image-20201122192621843](F:\Users\Administrator\Desktop\notes\hadoop\12.png)



4.查看上传结果`hadoop dfs -ls /data`

![image-20201122192750245](F:\Users\Administrator\Desktop\notes\hadoop\13.png)

5.创建执行的脚本`vi word_count.sh` 输入以下

```
hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.2.jar wordcount /data /result
```



6.执行wordcout，查看结果

`bash word_count.sh`

![image-20201122192916216](F:\Users\Administrator\Desktop\notes\hadoop\14.png)

![image-20201122193012344](F:\Users\Administrator\Desktop\notes\hadoop\15.png)

7.分别执行

`hadoop dfs -ls /result`

`hadoop dfs -cat /result/part-r-00000`

![image-20201122193229349](F:\Users\Administrator\Desktop\notes\hadoop\16.png)



8.查看web

首先将端口打开

![image-20201122193506698](F:\Users\Administrator\Desktop\notes\hadoop\17.png)

![image-20201122193554184](F:\Users\Administrator\Desktop\notes\hadoop\18.png)

![image-20201122193632566](F:\Users\Administrator\Desktop\notes\hadoop\19.png)

![image-20201122193726503](F:\Users\Administrator\Desktop\notes\hadoop\20.png)

最后在浏览器输入`公网ip:端口号`即可查看结果

![image-20201122194137721](F:\Users\Administrator\Desktop\notes\hadoop\21.png)





问题：

1.如果遇到

![image-20201122194519953](F:\Users\Administrator\Desktop\notes\hadoop\22.png)

则 修改要保存的路径`vi word_count.sh`

![image-20201122194626817](F:\Users\Administrator\Desktop\notes\hadoop\23.png)

或者执行`hadoop dfs -rm -r /result` 将hadoop文件系统生成/result删除，就不会报错了

2.指定map和reduce的数量

输入`vi word_count.sh` 添加` -D mapred.reduce.tasks=10`指定reduce的数量，具体见[这](http://hadoop.apache.org/docs/stable1/streaming.html#Streaming+Command+Options)和[这](http://blog.itpub.net/15498/viewspace-2136139/)

`vi etc/hadoop/hdfs-site.xml` 值调大就可能减小map数量，调小就可能增大map数量

```
<property> 
	<name>dfs.block.size</name> 
	<value>256000000</value>    
</property>
```

但map的数量直接用这样的形式更改不一定生效，具体见[这](https://blog.csdn.net/lylcore/article/details/9136555)

完整命令`hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.2.jar wordcount -D mapred.reduce.tasks=10 /largefile /result2 `

3.如何进行自定map和reduce函数

这里很详细就不再赘述，参考[这](http://hadoop.apache.org/docs/stable1/streaming.html#Streaming+Command+Options)

