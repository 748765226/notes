#### 1.添加个人账户

- cd path
- adduser **ozh**
  设置密码，其它选项按默认即可，账户即添加成功
- su ozh
  输入密码后即可切换到新创建的账户

#### 2.给用户添加sudo权限

​	编辑 /etc/sudoers
​	sudo visudo，进入编辑模式，找到这一 行：“root ALL=(ALL) ALL"在起下面添加"用户名 ALL=(ALL) ALL”
​	sudo vim /etc/sudoers 即可看到成功添加 ozh ALL=(ALL) ALL

   或者sudo usermod -a -G sudo hduser

- sudo usermod -a -G sudo tt（新用户）
- 修改后的结果可以查看/etc/group，可以看到sudo这一栏中包含tt

#### 3.修改密码

在超级用户权限下，输入sudo passwd ozh，然后两次输入密码，即可修改密码。



**压缩**tar -czvf test.tar.gz a.c   //压缩 a.c文件为test.tar.gz

**解压**tar -xzvf test.tar.gz 



**$ sudo chmod -R 777 某一目录**
-R 是指级联应用到目录里的所有子目录和文件
777 是所有用户都拥有最高权限



**Screen**

screen -S xxx //创建一个screen，按住Ctrl，依次按a+d暂离会话，依次按a+c创建子会话，a+2,切换会话

screen -ls //查看所有创建会话

screen -r xxx //恢复会话

有时在恢复screen时会出现There is no screen to be resumed matching ****，screen -d xxx

exit退出会话



**etc/.bashrc和etc/profile 针对所有用户，home/。。针对特定用户**



sudo apt-get install python3-venv

python3 -m venv venv//创建

source venv/bin/activate//进入环境

deactivate//退出



mv  A/B C/D //移动A中的B文件到C目录下的D文件



**容器介绍**

https://www.cnblogs.com/qcloud1001/p/9273549.html

https://www.cnblogs.com/bethal/p/5942369.html



apt search xxx //想要安装的文件xxx的网络资源全程



apt remove XXX//卸载xxx

dpkg -l | grep xxx//查询已安装包 sudo apt remove --purge dock.io//针对性删除





sudo ufw status **查看防火墙状态**

sudo ufw enable/disable



/etc/hostname  主机名

/etc/hosts



df -h //查看磁盘使用情况

du -sh *//查看文件占用空间大小



**GIT**

git push origin（远程库名称）xx:xx(本地分支：远程分支) 

git pull origin（远程库名称）xx:xx(远程分支：本地分支)

git merge origin/master(将名为origin的远程库的master分支合并到当前分支)

git clone -b xxx（分支名） xxxx（远程库地址）

git stash 暂时丢掉

git stash pop



**conda**

conda config --set auto_activate_base false 退出默认base

conda create -n xxx python==3.x 创建环境

conda env remove -n env-name 删除指定环境

conda env list 列出环境列表

conda install xxx 安装xx包

conda remove xxx 删除指定包

conda update package-name 更新指定包

conda list 列出所有包

conda search xxx 搜索指定包

conda activate  xxx 进入某个环境

conda deactivate 退出

**添加conda源**

conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/

conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/

conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/

conda config --set show_channel_urls yes

pip3 install pyqt5 -i https://pypi.tuna.tsinghua.edu.cn/simple  指定清华源

pip3 --default-timeout=1000 install -U matplotlib 让延迟检测时间变长

pip3 --default-timeout=1000  install -U pyqt5 -i   https://pypi.tuna.tsinghua.edu.cn/simple http://mirrors.aliyun.com/pypi/simple/

解决pip下载速度慢 https://blog.csdn.net/fatfatmomo/article/details/81184119

Linux下，修改 ~/.pip/pip.conf (没有就创建一个文件夹及文件。文件夹要加“.”，表示是隐藏文件夹)

[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple

[install]
trusted-host=mirrors.aliyun.com

apt-get install  psmisc #安装fuser

**Tmux**

会话 session 窗口 window 窗格 

http://www.ruanyifeng.com/blog/2019/10/tmux.html

tmux new -s new-session #创建新会话

tmux detach或者按下ctrl+b d 分离会话

tmux attach -t name-session#恢复会话

tmux switch -t name-session #切换会话

tmux ls #显示所有会话

tmux kill-session -t  name-session #杀死会话

tmux rename -t name-session new-session-name # 重命名

**高级tmux**

tmux split-window # 划分上下窗格

tmux split -h # 划分左右窗格 

tmux select-pane -U -D -L -R #上下左右切换鼠标光标

tmux swap-pane -U -D -L -R # 上下左右切换窗格位置

**tmux 快捷键**

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

**tmux流程**

tmux 新建一个会话， ctrl+b $ 重命名当前会话， ctrl+b % 左右划分 窗格 ，ctrl+b “ 上下划分窗格， ctrl+b c 新建窗口，ctrl+b ， 重命名窗口， ctrl+b d 离开当前会话，tmux ls 显示所有会话，tmux attach -t 会话名，ctrl+b x 关闭当前窗格 ctrl+b w列表选择会话、窗口、窗格。

**curl待学...**

**安装detectron包**

pip3 install detectron2==0.1.3-f  https://dl.fbaipublicfiles.com/detectron2/wheels/cu101/torch1.5/index.html 

 

**Ubuntu16.04桌面突然卡住**

(1)Ubuntu有6个tty终端，按住Ctrl+Alt+F1可以进入tty1终端，（同理Ctrl+Alt+F2，F3等可以进入其他的tty1终端，这里我们只需要进入一个tty终端就能解决问题）。

(2)进入tty终端后先输入你的用户名和密码登录。

(3)执行命令注销桌面重新登陆：

sudo pkill Xorg

https://blog.csdn.net/hautxuhaihu/article/details/78924926 

**Vim高级用法**

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