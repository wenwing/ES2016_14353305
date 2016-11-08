##一、ROS（Robot operation system）
ROS是一个开放的标准平台，它提供了一系列的软件框架和工具以帮助软件开发者创建机器人应用软件，底层提供硬件驱动，软件层面支持通用的文件格式。
##二、ROS安装笔记
在安装ROS前，需要先明确Ubuntu的版本，在虚拟机中打开terminal，输入以下指令

    sudo lsb_release -a
可以看到版本信息如下，Ubuntu版本为14.04.4
![](http://upload-images.jianshu.io/upload_images/3176291-cdf34105f2cd92e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####2.1 Configure your Ubuntu repositories

####2.2 Setup your sources.list
ROS Jade只支持Trusty (14.04), Utopic (14.10) and Vivid (15.04)的版本，而ROS Kinetic只支持Wily (Ubuntu 15.10), Xenial (Ubuntu 16.04) and Jessie (Debian 8)的版本，因此在安装之前一定要明确Ubuntu的版本

    sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
####2.3 Set up your keys

    sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116
####2.4 Installation
######A. 更新
update到最新版本的Debian软件包

    sudo apt-get update
如果是14.04版本的Ubuntu，不要安装以下的包，会摧毁X服务器，我的Ubuntu版本为14.04，因此以下一句命令行可以忽略

    sudo apt-get install xserver-xorg-dev-lts-utopic mesa-common-dev-lts-utopic libxatracker-dev-lts-utopic libopenvg1-mesa-dev-lts-utopic libgles2-mesa-dev-lts-utopic libgles1-mesa-dev-lts-utopic libgl1-mesa-dev-lts-utopic libgbm-dev-lts-utopic libegl1-mesa-dev-lts-utopic
通过安装下面的包解决依赖性的问题

    sudo apt-get install libgl1-mesa-dev-lts-utopic
#####B. 安装
之后，依次输入命令行进行安装。

Desktop-Full Install:

    sudo apt-get install ros-jade-desktop-full
ROS-Base: (Bare Bones)：

    sudo apt-get install ros-jade-ros-base
Individual Package:

    sudo apt-get install ros-jade-slam-gmapping
最后，找到可用的软件包

    apt-cache search ros-jade
####2.5 Initialize rosdep
在使用ROS之前要对rosdep进行初始化，rosdep可以让你更方便安装你想要编译的系统依赖源和运行ROS的一些核心组件

    sudo rosdep init
    rosdep update
####2.6 Environment setup
方便每次新的shell启动时，ROS环境变量自动添加到bash会话中

    echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc
    source ~/.bashrc
如果有多个ROS分配安装，~/.bashrc是setup.bash当前使用版本的唯一来源，如果要将环境变为当前的shell，执行以下命令行

    source /opt/ros/jade/setup.bash
####2.7 Getting rosinstall
安装rosinstall工具，rosinstall是ROS中常用的命令行工具，它可以让你很容易通过一个命定行，在ROS软件包树中下载源

    sudo apt-get install python-rosinstall
##三、实验结果
当以上的实验步骤都没有报错，能成功到达2.7，看到以下的信息，代表ROS的安装完成，可以进行cartographer的测试

![](http://upload-images.jianshu.io/upload_images/3176291-42cc4d2d89935f10.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##四、实验感想
跟往常一样，安装笔记还是能比较快完成的，基本就是命令行输入。但是比较重要的一点是要安装什么、用什么命令行安装。在原网页处已经写得比较清楚了，但因为是英文的关系，看起来不如中文方便而已。

这一次的安装，关键的问题是Ubuntu的版本信息，在一开始一定要清楚自己的Ubuntu是什么版本，然后按Ubuntu的版本安装对应的ROS。