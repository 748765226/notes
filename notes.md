# 大杂烩笔记

<!--more-->

## Linux

[TOC]



### 添加个人账户

- `sudo adduser tt` 
  
  它选项按默认即可，账户即添加成功
  
- `su ozh`
  输入密码后即可切换到新创建的账户

- `sudo cat /etc/passwd `

  最后面几行为可用账户

- `who `

  可查看当前登录的用户

- `sudo userdel -r tt`

  删除ozh用户，可能删除不完全关联的文件




### 给用户添加sudo权限

- 方法一

  `sudo vi etc/sudoers `

  找到这一 行：“root ALL=(ALL) ALL"在起下面添加"tt ALL=(ALL) ALL”

- 方法二

  `sudo usermod -a -G sudo tt`  

修改后的结果可以查看/etc/group，可以看到sudo这一栏中包含tt 



### 修改密码

- `sudo passwd tt`

  修改用户tt的密码



### tar压缩解压

- `tar -czvf  1.tar 1` 

  压缩当前目录下1文件，压缩后的名字为1.tar

  **如果压缩的文件指定目录的话，目录路径也会被压缩进去**

- `tar -xzvf  1.tar`

  解压1.tar压缩包



### 目录权限查看和修改

```
sudo chmod -R 777 tt

-R 是指级联应用到目录里的所有子目录和文件

777 是所有用户都拥有最高权限

权限数字对应权限组说明：

总共分为4部分

【文件或文件夹】【owner权限】【group权限】【others权限】

【文件是-，文件夹是d】【r/w/x相加】【r/w/x相加】【r/w/x相加】

Linux档案的基本权限就有九个，分别是owner/group/others三种身份各有自己的read/write/execute权限。

r 读权限read 4

w 写权限write 2

x 操作权限execute 1

-表示相应的权限还没有被授予

-rw------- (600) 只有所有者才有读和写的权限

-rw-r--r-- (644) 只有所有者才有读和写的权限，组群和其他人只有读的权限

-rwx------ (700) 只有所有者才有读，写，执行的权限

-rwxr-xr-x (755) 只有所有者才有读，写，执行的权限，组群和其他人只有读和执行的权限

-rwx--x--x (711) 只有所有者才有读，写，执行的权限，组群和其他人只有执行的权限

-rw-rw-rw- (666) 每个人都有读写的权限

-rwxrwxrwx (777) 每个人都有读写和执行的权限
```



### 查看当前操作系统等信息

- 查看当前操作系统版本信息

  `lsb_release -a`

  ` cat /etc/lsb-release`

  `  cat /etc/issue `

- 查看当前操作系统内核信息 

  `uname -a`

- 查看CPU信息：
  `grep name /proc/cpuinfo`
  `lscpu` 

- 查看内存

  `free -m`

- 查看文件占用内存

  `du -sh *`

-  查看硬盘信息 

  ` df -h`

  ` sudo fdisk -l `

- 查看pci总线

  `lspci #没有的话安装pciutils` 

- 查看GPU型号

  `lspci | grep -i nvidia`

- 查看NVIDIA驱动版本

  `sudo dpkg --list | grep nvidia-*`

  `cat /proc/driver/nvidia/version`



### 查看进程和端口

```
端口是 Linux 系统上特定进程之间逻辑连接的标识，包括物理端口和软件端口。由于 Linux 操作系统是一个软件，因此本文只讨论软件端口。软件端口始终与主机的 IP 地址和相关的通信协议相关联，因此端口常用于区分应用程序。大部分涉及到网络的服务都必须打开一个套接字来监听传入的网络请求，而每个服务都使用一个独立的套接字。

套接字是和 IP 地址、软件端口和协议结合起来使用的，而端口号对传输控制协议（TCP）和用户数据报协议（UDP）协议都适用，TCP 和 UDP 都可以使用 0 到 65535 之间的端口号进行通信。

以下是端口分配类别：

0 - 1023： 常用端口和系统端口
1024 - 49151： 软件的注册端口
49152 - 65535： 动态端口或私有端口

ps -aux | grep java #查看正在运行的java进程的所有状态

netstat -an | grep 9000 #查看9000端口是否被占用（listen占用）

netstat -tunlp|grep 9000 #查看9000端口被什么进程占用

kill -9 PID #杀死占用9000端口进程的PID -9强迫
```



### Screen

```
screen -S xxx #创建一个screen，按住Ctrl，依次按a+d暂离会话，依次按a+c创建子会话，a+2,切换会话

screen -ls #查看所有创建会话

screen -r xxx #恢复会话

有时在恢复screen时会出现There is no screen to be resumed matching ****，screen -d xxx

exit退出会话


```



### 配置文件profile和.bashrc

```
profile 用于登录， bashrc 用于交互。 /etc/profile /etc/bash.bashrc 用于所有用户 ~/.profile ~/.bashrc 用于用户自己的配置

遇见问题：linux下如何设置环境变量

假如现在安装JDK，需要配置环境变量，然后执行如下命令：
export JAVA_HOME=/xxx/xxx
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=$JAVA_HOME/lib:$JAVA_HOME/jre/lib

如果想要让环境变量不是临时的，那么上面的export 命令需要写入配置文件，
1.写入 /etc/profile
2.或写入~/.profile
3.或写入 ~/.bashrc
```



https://www.cnblogs.com/qcloud1001/p/9273549.html

### apt包管理器

- 想要安装的文件xxx的网络资源全程

  `apt search xxx `

- 卸载xxx

  `apt remove XXX`

- 查询已安装包 sudo apt remove --purge dock.io//针对性删除

  `dpkg -l | grep xxx`

- ifconfig找不到

  `sudo apt install net-tools`

- 安装fuser

  `apt-get install  psmisc `

### 查看防火墙状态

- `sudo ufw status`



### 关闭防火墙

- `sudo ufw enable/disable`



### Hostname

- 临时修改主机名

  `hostname xxx ` 

-  永久修改主机名

  `hostnamectl set-hostname xxx`



### GIT

```
git push origin（远程库名称）xx:xx(本地分支：远程分支) 

git pull origin（远程库名称）xx:xx(远程分支：本地分支)

git merge origin/master(将名为origin的远程库的master分支合并到当前分支)

git clone -b xxx（分支名） xxxx（远程库地址）

git stash 暂时丢掉

git stash pop
```



### conda

```
conda config --set auto_activate_base false #退出默认base

conda create -n xxx python==3.x #创建环境

conda env remove -n env-name #删除指定环境

conda env list #列出环境列表

conda install xxx #安装xxx包

conda remove xxx #删除指定包

conda update package-name #更新指定包

conda list #列出所有包

conda search xxx #搜索指定包

conda activate  xxx #进入某个环境

conda deactivate #退出
```



### 添加conda源

```
conda config --show 显示默认配置

conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/

conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/

conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/

conda config --set show_channel_urls yes

conda config --remove-key channels 还原默认源
```



### conda安装过程出现PackagesNotFoundError

- `anaconda search -t conda torch`

- `conda install -c https://conda.anaconda.org/  搜索结果 +空格+torch` [参考](https://blog.csdn.net/webmater2320/article/details/105230350/)



### pip

```
pip3 install pyqt5 -i https://pypi.tuna.tsinghua.edu.cn/simple  指定清华源

pip3 --default-timeout=1000 install -U matplotlib 让延迟检测时间变长

pip3 --default-timeout=1000  install -U pyqt5 -i   https://pypi.tuna.tsinghua.edu.cn/simple 
```



### [Ubuntu18.04下 sudo pip与sudo pip3 修改路径指向](https://blog.csdn.net/e_xiaoyu/article/details/102846313)



### [解决pip下载速度慢](https://blog.csdn.net/fatfatmomo/article/details/81184119)

```
Linux下，修改 ~/.pip/pip.conf (没有就创建一个文件夹及文件。文件夹要加“.”，表示是隐藏文件夹)

[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple

[install]
trusted-host=mirrors.aliyun.com
```



### [Tmux](http://www.ruanyifeng.com/blog/2019/10/tmux.html)

```
会话 session 窗口 window 窗格 

tmux new -s new-session #创建新会话

tmux detach或者按下ctrl+b d 分离会话

tmux attach -t name-session#恢复会话

tmux switch -t name-session #切换会话

tmux ls #显示所有会话

tmux kill-session -t  name-session #杀死会话

tmux rename -t name-session new-session-name # 重命名
```



### 高级tmux

```
tmux split-window # 划分上下窗格

tmux split -h # 划分左右窗格 

tmux select-pane -U -D -L -R #上下左右切换鼠标光标

tmux swap-pane -U -D -L -R # 上下左右切换窗格位置
```



### tmux 快捷键

- `Ctrl+b d`：分离当前会话。
- `Ctrl+b s`：列出所有会话。
- `Ctrl+b %`：划分左右两个窗格。
- `Ctrl+b "`：划分上下两个窗格。
- `Ctrl+b <arrow key>`：光标切换到其他窗格。`<arrow key>`是指向要切换到的窗格的方向键，比如切换到下方窗格，就按方向键`↓`。
- `Ctrl+b x`：关闭当前窗格。
- `Ctrl+b z`：当前窗格全屏显示，再使用一次会变回原来大小。
- `Ctrl+b q`：显示窗格编号。
- `Ctrl+b !`：将当前窗格拆分为一个独立窗口。
- `Ctrl+b c`：创建一个新窗口，状态栏会显示多个窗口的信息。
- `Ctrl+b <number>`：切换到指定编号的窗口，其中的`<number>`是状态栏上的窗口编号。
- `Ctrl+b w`：从列表中选择窗口。
- `Ctrl+b ,`：窗口重命名。
- `Ctrl+b $`：重命名当前会话。



### tmux流程

tmux 新建一个会话， ctrl+b $ 重命名当前会话， ctrl+b % 左右划分 窗格 ，ctrl+b “ 上下划分窗格， ctrl+b c 新建窗口，ctrl+b ， 重命名窗口， ctrl+b d 离开当前会话，tmux ls 显示所有会话，tmux attach -t 会话名，ctrl+b x 关闭当前窗格 ctrl+b w列表选择会话、窗口、窗格。



### Vim高级用法

```
1)一些常用的Vim配置，在~/.vimrc中

syntax on    支持语法高亮

set nu      显示行号
set nonu    不显示行号

set ai      设置自动缩进

set shiftwidth=4  设置自动缩进 4 个空格, 当然要设自动缩进先.

set sts=4      即设置 softtabstop 为 4. 输入 tab 后就跳了 4 格.

set tabstop=4    实际的 tab 即为 4 个空格, 而不是缺省的 8 个.

set expandtab    在输入 tab 后, vim 用恰当的空格来填充这个 tab.

:set hls 打开搜索高亮

:set nohls 取消搜索高亮 

:set list ： 显示特殊字符

:set nolist 

2）查找 

/xxx(?xxx) 表示在整篇文档中搜索匹配xxx的字符串, / 表示向下查找, ? 表示向上查找.查找到以后, 再输入 n 查找下一个匹配处, 输入 N 反方向查找.

3)  移动

w(e) 移动光标到下一个单词. b 移动光标到上一个单词.
0 移动光标到本行最开头. ^ 移动光标到本行最开头的字符处. $ 移动光标到本行结尾处.
向前向后翻页 ctrl+f 和 ctrl+b.
% 跳转到相配对的括号.
  G(shift+g)   - go to the last line in the vim editor (文件的末尾)  1G - goto line number 1(文件的开始) 20G - goto line number 20
```





### [显卡，显卡驱动,nvcc, cuda driver,cudatoolkit,cudnn]( https://zhuanlan.zhihu.com/p/91334380 )

**CUDA Toolkit** 包括 **1.Compiler**: CUDA-C和CUDA-C++编译器`NVCC`  **2.Tools**: 提供一些像`profiler`,`debuggers`等工具  **3.Libraries**: 下面列出的部分科学库和实用程序库  **4.CUDA Samples**: 演示如何使用各种CUDA和library API的代码示例  **5.CUDA Driver**: 运行CUDA应用程序需要系统至少有一个**具有CUDA功能的GPU**和**与CUDA工具包兼容的驱动程序**  

![img](https://pic1.zhimg.com/80/v2-fc8c720a858b6c2583b09f0228c4b3e0_720w.jpg)

而显卡GPU driver功能上等价于cuda toolkit里面的cuda driver。但cuda有两个API Runtime API和Driver API ，这两个API都有对应的CUDA版本 ， 

用于支持**driver API**的必要文件(如`libcuda.so`)是由**GPU driver installer**安装的。`nvidia-smi`就属于这一类API。  

用于支持**runtime API**的必要文件(如`libcudart.so`以及`nvcc`)是由**CUDA Toolkit installer**安装的。（**CUDA Toolkit Installer有时可能会集成了GPU driver Installer**）。`nvcc`是与CUDA Toolkit一起安装的CUDA compiler-driver tool，它只知道它自身构建时的CUDA runtime版本。它不知道安装了什么版本的GPU driver，甚至不知道是否安装了GPU driver。  

如果driver API和runtime API的CUDA版本不一致可能是因为使用的是单独的（显卡）GPU driver installer，而不是CUDA Toolkit installer里的GPU driver installer。 

 ![img](https://pic3.zhimg.com/80/v2-a1f1d9f699697a8e05979abf749fbeae_720w.jpg) 

```
检测pytorch在当前cuda版本中是可用
import torch 
print(torch.cuda.is_available())

torch.cuda.get_device_name(0)
```

## docker

###  [docker](https://www.cnblogs.com/qcloud1001/p/9273549.html) 教程



```
docker image ls  \# 列出本机的所有 image 文件 

docker image rm [image_name] \# 删除 image文件 

docker container run [image_name]  会从一个镜像文件中生成一个容器实例,并且运行它

docker container run命令具有自动抓取 image 文件的功能。如果发现本地没有指定的 image 文件，就会从仓库自动抓取。docker image pull命令并不是必需的步骤。 

 docker container ls  # 查看当前运行容器 

 docker container ls --all   \# 列出本机所有容器，包括终止运行的容器 

 终止运行的容器文件依然会占用硬盘空间,可以使用docker container rm [container_id]命令删除 



以下命令使用 ubuntu 镜像**启动**一个容器，参数为以命令行模式进入该容器：

**docker run -it ubuntu /bin/bash**

-it : 容器的shell会映射到当前本地的shell,你在本机窗口输入的命令会传入到容器中 

**查看**所有的容器（**包括已停止**的）命令如下：

**docker ps -a**

使用 **docker start** **重启一个已停止**的容器：

**docker start container_id**

希望 docker 的服务是在**后台运行**的，我们可以过 **-d** 指定容器的运行模式。

**docker run -itd --name ubuntu-test ubuntu /bin/bash**

 加了 **-d** 参数默认不会进入容器，想要进入容器需要使用指令 **docker exec** 

 **停止容器** 

**docker stop <容器 ID>**

在使用 **-d** 参数时，容器启动后会进入后台。此时想要进入容器，可以通过以下指令进入：

- **docker attach**(粘贴到当前终端)
- **docker exec**：推荐大家使用 docker exec 命令，因为此退出容器终端，不会导致容器的停止。



如果要**导出本地某个容器**，可以使用 **docker export** 命令。

 **docker export 1e560fca3906 > ubuntu.tar**  这样将导出容器快照到本地文件 



可以使用 **docker import** 从容器快照文件中再导入为镜像，以下实例将快照文件 ubuntu.tar 导入到镜像 test/ubuntu:v1:

 cat docker/ubuntu.tar | **docker import - test/ubuntu:v1**



下面的命令可以**清理掉所有处于终止状态的容器**。

**docker container prune**
```

### 

## Ubuntu 

### [Ubuntu sudo update与upgrade的作用及区别](https://blog.csdn.net/beckeyloveyou/article/details/51352426 )



### [Ubuntu18.04下更改apt源为阿里云源](https://blog.csdn.net/zhangjiahao14/article/details/80554616)



### [解决ubuntu分辨率问题](https://blog.csdn.net/simmonloyld/article/details/87393775)



### [Ubuntu16.04桌面突然卡住](https://blog.csdn.net/hautxuhaihu/article/details/78924926)

```
(1)Ubuntu有6个tty终端，按住Ctrl+Alt+F1可以进入tty1终端，（同理Ctrl+Alt+F2，F3等可以进入其他的tty1终端，这里我们只需要进入一个tty终端就能解决问题）。

(2)进入tty终端后先输入你的用户名和密码登录。

(3)执行命令注销桌面重新登陆：

sudo pkill Xorg
```



### 安装detectron包

```
pip3 install detectron2==0.1.3-f  

https://dl.fbaipublicfiles.com/detectron2/wheels/cu101/torch1.5/index.html 
```



### curl待学...

