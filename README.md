###一、DOL框架描述
The distributed operation layer (DOL) is a framework that enables the (semi-) automatic mapping of 

applications onto the multiprocessor SHAPES architecture platform. The DOL consists of basically 

three parts:
* DOL Application Programming Interface
* DOL Functional Simulation
* DOL Mapping Optimization

###二、DOL安装笔记
######在虚拟机中打开terminal，输入以下指令
####1. 安装一些必要的环境（ubuntu为例）
     
    $  sudo apt-get update
    $  sudo apt-get install ant
    $  sudo apt-get installopenjdk-7-jdk
    $  sudo apt-get install unzip
####2. 直接用命令行下载文件到虚拟机中

    $  sudo wget http://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.1.tgz
    $  sudo wget http://www.tik.ee.ethz.ch/~shapes/downloads/dol_ethz.zip
####3.解压文件

    $  mkdir dol				       //新建dol的文件夹
    $  unzipdol_ethz.zip -d dol	    //将dolethz.zip解压到 dol文件夹中
    $  tar -zxvf systemc-2.3.1.tgz	 //解压systemc
####4.编译systemc

    $  cdsystemc-2.3.1			    //解压后进入systemc-2.3.1的目录下
    $  mkdir objdir			       //新建一个临时文件夹objdir
    $  cdobjdir				       //进入该文件夹objdir
    $  ../configure CXX=g++--disable-async-updates	//运行configure
运行后得到以下结果
![](http://upload-images.jianshu.io/upload_images/3176291-93eef87738aa9f1b.JPG?imageMogr2/auto-

orient/strip%7CimageView2/2/w/1240)


    $  sudo make install			//编译
    $  pwd					      //输出当前所在路径，需记录当前的工作路径
路径如下
![](http://upload-images.jianshu.io/upload_images/3176291-27507c299ebf0694.JPG?imageMogr2/auto-

orient/strip%7CimageView2/2/w/1240)

####5.编译dol

    $  cd../dol				    //进入dol的文件夹
找到下面这段话，修改build_zip.xml文件

><propertyname="systemc.inc" value="pwd输出的工作路径/include"/>
><propertyname="systemc.lib" value="pwd输出的工作路径/lib-linux/libsystemc.a"/>
    
    $  ant -f build_zip.xml all	//编译
若成功会显示build successful
![](http://upload-images.jianshu.io/upload_images/3176291-6e276d4a9c85ba5a.JPG?imageMogr2/auto-

orient/strip%7CimageView2/2/w/1240)
####6.至此完成DOL的开发环境配置

    $  cdbuild/bin/main			  //进入build/bin/mian路径下
    $  ant -frunexample.xml -Dnumber=1	//运行第一个例子
若成功会显示build successful
![](http://upload-images.jianshu.io/upload_images/3176291-198413c45e637492.JPG?imageMogr2/auto-

orient/strip%7CimageView2/2/w/1240)
###三、实验心得
这次的配置只要跟着实验文档就能比较顺利地完成，但比较波折的是用Markdown写实验报告。坦诚地说Markdown

语法并不复杂，很快就能上手，但在书写平台的选择上比较反复。
在最初，我选择了即时覆盖原文本渲染方式的typora，但typora只能以代码单独分行的形式呈现，并且添加图片

比较麻烦，需要先将图片上传到图床中，再将生成的图片链接添加到插入图片的语法中。在用typora完成实验报

告之后觉得效果差强人意，因此又试了简书。
就这两者来说，我认为简书更方便一些，能支持代码模块化，图片直接拖入即可插入，之后的实验报告也会选择

在简书上面书写。