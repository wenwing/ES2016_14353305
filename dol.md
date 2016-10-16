##一、实验原理
example将文件分为两大部分：
#####1.  src文件夹中按功能定义的各进程，包括生产者、消费者和处理模块
此部分的文件又分为两种：
.c文件以及.c文件对应的.h的文件，它们是框图的功能描述。.c文件中的`init`函数初始化模块，`fire`函数定义模块功能，每个模块都必须要有可能被执行无数次的`fire`函数，但`init`可以选择写或者不写。
因此，需要修改模块功能时，则修改`fire`函数的内容。
#####2. xml文件则是定义系统架构，即模块连接方式
进程定义如下：

    <process name="实现的模块的名字">
       <port type="input或者output" name="端口的名字（在.h文件里面）"/>
       <source type="c" location="模块名字.c"/>
    </process>
通道定义如下：

    <sw_channel type="fifo" size="缓冲区大小" name="通道名字">
       <port type="input" name="in"/>
       <port type="output" name="out"/>
       //通道一定有in, out两个端口
    </sw_channel>
定义各模块之间的连接，一条线对应两个connection：

    <connection name="名字">
       <origin name="模块或者通道的名字">
           <port name="端口名"/>
       </origin>
       <target name="模块或者通道的名字">
            <port name="端口名"/>
       </target>
    </connection>
##二、实验任务
###任务一
#####1.  题目
修改 example2，让 3个square模块变成 2个, tips: 修改xml的`iterator`
#####2. 分析
square模块是执行模块，要让执行模块数量改变，即改变系统架构，修改example的xml文件。
在example2的xml文件中首先定义了`variable N`为3

    <variable value="3" name="N"/>
并且通过`iteration`生成了N个square模块，即3个square模块，connection也是通过`iteration`生成的
    
    <iterator variable="i" range="N">
      <process name="square">
        <append function="i"/>
        <port type="input" name="0"/>
        <port type="output" name="1"/>
        <source type="c" location="square.c"/>
      </process>
    </iterator>

    
因此，只需要修改N的值，即可改变系统架构，使3个square模块变为2个square模块，并且connection也同时改变。
#####3. 实验结果

![result of example2](http://upload-images.jianshu.io/upload_images/3176291-1c18db9d1bb2ae7d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如图所示，generator产生0-19的整数，square对输入进行平方操作，有两个square模块，因此consumer输出的结果是0-19这20个整数的4次方。

![frame of example2](http://upload-images.jianshu.io/upload_images/3176291-09186b8649bd69bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
###任务二
#####1.  题目
修改example1，使其输出3次方数，tips：修改square.c
#####2. 分析
在不改变系统架构的前提下要将输出平方数更改为3次方数，即修改模块功能，修改`fire`函数。square.c的`fire`函数如下

    int square_fire(DOLProcess *p) {
      float i;

      if (p->local->index < p->local->len) {
        DOL_read((void*)PORT_IN, &i, sizeof(float), p);
        i = i*i;
        DOL_write((void*)PORT_OUT, &i, sizeof(float), p);
        p->local->index++;
      }

      if (p->local->index >= p->local->len) {
        DOL_detach(p);
        return -1;
      }

      return 0;
    }
square.c的核心代码是`i=i*i;`，使生成0-19的整数经过执行模块后变为平方数，因此只需要将`i=i*i*;`改为`i=i*i*i;`即可使平方数变为3次方数。
#####3. 实验结果
![result of example1](http://upload-images.jianshu.io/upload_images/3176291-d5f04d59cd2e5d6d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如图所示，generator产生0-19的整数，square对输入进行3次方操作，只有1个square模块，因此consumer输出的结果是0-19这20个整数的3次方。

![frame of example1](http://upload-images.jianshu.io/upload_images/3176291-2134f9328934da62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##3. 实验感想
这次的实验目的重在让我们理解按功能定义的各进程以及系统是如何架构的，理清楚这两个方面就能很快地完成实验。

但有一个需要注意的是，如果之前编译过的example，在修改后需要先删除原来生成的文件，再重新编译，重新编译的文件是不会覆盖原来编译的文件的。