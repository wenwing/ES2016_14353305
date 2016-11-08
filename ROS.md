﻿##һ��ROS��Robot operation system��
ROS��һ�����ŵı�׼ƽ̨�����ṩ��һϵ�е������ܺ͹����԰�����������ߴ���������Ӧ��������ײ��ṩӲ���������������֧��ͨ�õ��ļ���ʽ��
##����ROS��װ�ʼ�
�ڰ�װROSǰ����Ҫ����ȷUbuntu�İ汾����������д�terminal����������ָ��

    sudo lsb_release -a
���Կ����汾��Ϣ���£�Ubuntu�汾Ϊ14.04.4
![](http://upload-images.jianshu.io/upload_images/3176291-cdf34105f2cd92e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####2.1 Configure your Ubuntu repositories

####2.2 Setup your sources.list
ROS Jadeֻ֧��Trusty (14.04), Utopic (14.10) and Vivid (15.04)�İ汾����ROS Kineticֻ֧��Wily (Ubuntu 15.10), Xenial (Ubuntu 16.04) and Jessie (Debian 8)�İ汾������ڰ�װ֮ǰһ��Ҫ��ȷUbuntu�İ汾

    sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
####2.3 Set up your keys

    sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116
####2.4 Installation
######A. ����
update�����°汾��Debian�����

    sudo apt-get update
�����14.04�汾��Ubuntu����Ҫ��װ���µİ�����ݻ�X���������ҵ�Ubuntu�汾Ϊ14.04���������һ�������п��Ժ���

    sudo apt-get install xserver-xorg-dev-lts-utopic mesa-common-dev-lts-utopic libxatracker-dev-lts-utopic libopenvg1-mesa-dev-lts-utopic libgles2-mesa-dev-lts-utopic libgles1-mesa-dev-lts-utopic libgl1-mesa-dev-lts-utopic libgbm-dev-lts-utopic libegl1-mesa-dev-lts-utopic
ͨ����װ����İ���������Ե�����

    sudo apt-get install libgl1-mesa-dev-lts-utopic
#####B. ��װ
֮���������������н��а�װ��

Desktop-Full Install:

    sudo apt-get install ros-jade-desktop-full
ROS-Base: (Bare Bones)��

    sudo apt-get install ros-jade-ros-base
Individual Package:

    sudo apt-get install ros-jade-slam-gmapping
����ҵ����õ������

    apt-cache search ros-jade
####2.5 Initialize rosdep
��ʹ��ROS֮ǰҪ��rosdep���г�ʼ����rosdep������������㰲װ����Ҫ�����ϵͳ����Դ������ROS��һЩ�������

    sudo rosdep init
    rosdep update
####2.6 Environment setup
����ÿ���µ�shell����ʱ��ROS���������Զ���ӵ�bash�Ự��

    echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc
    source ~/.bashrc
����ж��ROS���䰲װ��~/.bashrc��setup.bash��ǰʹ�ð汾��Ψһ��Դ�����Ҫ��������Ϊ��ǰ��shell��ִ������������

    source /opt/ros/jade/setup.bash
####2.7 Getting rosinstall
��װrosinstall���ߣ�rosinstall��ROS�г��õ������й��ߣ����������������ͨ��һ�������У���ROS�������������Դ

    sudo apt-get install python-rosinstall
##����ʵ����
�����ϵ�ʵ�鲽�趼û�б����ܳɹ�����2.7���������µ���Ϣ������ROS�İ�װ��ɣ����Խ���cartographer�Ĳ���

![](http://upload-images.jianshu.io/upload_images/3176291-42cc4d2d89935f10.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##�ġ�ʵ�����
������һ������װ�ʼǻ����ܱȽϿ���ɵģ������������������롣���ǱȽ���Ҫ��һ����Ҫ��װʲô����ʲô�����а�װ����ԭ��ҳ���Ѿ�д�ñȽ�����ˣ�����Ϊ��Ӣ�ĵĹ�ϵ���������������ķ�����ѡ�

��һ�εİ�װ���ؼ���������Ubuntu�İ汾��Ϣ����һ��ʼһ��Ҫ����Լ���Ubuntu��ʲô�汾��Ȼ��Ubuntu�İ汾��װ��Ӧ��ROS��