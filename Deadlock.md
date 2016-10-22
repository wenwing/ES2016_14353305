##一. 实验原理
#####1. 死锁的四个必要条件
互斥条件：一个资源每次只能被一个进程使用
请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放
不剥夺条件：进程已获得的资源，在未使用完之前，不能强行剥夺
循环等待条件：若干进程之间形成头尾相接的循环等待资源关系
#####2. synchronized
当它用来修饰一个方法或者一个代码块的时候，能够保证在同一时刻最多只有一个线程执行该段代码。
当一个线程访问object的一个synchronized(this)同步代码块时，其他线程对object中所有其它synchronized(this)同步代码块的访问将被阻塞 
##二. 代码分析

class A和class B都含有synchronized修饰的函数last，当一个线程访问object的last()时，另一个线程对object的methodX()访问时会被阻塞。

    class A {
	  synchronized void methodA(B b) {
		 b.last();
	  }
	  synchronized void last() {
		 System.out.println("Inside A.last()");	
	  }
    }

    class B {
	  synchronized void methodB(A a) {
		a.last();
	  }
	  synchronized void last() {
		 System.out.println("Inside B.last()");
	  }
    }
类Deadlock的runnable，runnable是一个再后台默默运行的线程，当调到它执行时，运行run()中的语句。

    //实现一个Runnable子类
    class Deadlock implements Runnable {
	   A a = new A();
	   B b = new B();
	
	   //构造函数
	   Deadlock() {
		Thread t = new Thread(this);	//创建主线程t
		int count = 20000;
		t.start();	//线程t开始
		while(count-- > 0);
		a.methodA(b);
	   }
	   //runnable运行时调用的方法
	   public void run() {
		b.methodB(a);
	   }
	   //主函数
	   public static void main(String args[]) {
		new Deadlock();
	   }
    }
如下图所示，首先执行的是main函数中的new Deadlock()，在后台运行Deadlock的runnable子类，分别new一个类型为A的object a和类型为B的object b。

执行构造函数Deadlock()时，新建了一个线程t，并开始线程t，此时runnable被调度，执行函数run()中的语句。


![执行时间轴](http://upload-images.jianshu.io/upload_images/3176291-188f2df95af1ef22.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在函数run()中，object b访问synchronized修饰的函数methodB，调用传入的object a的函数last()，当计数变量count从20000变为0时，object a访问函数methodA。但object a的函数last()已经被访问，因此object a对函数methodA的访问，即对object b的函数last()的调用被阻塞。

##三. 实验结果分析
如下图所示，在跑第77次时停止，这个次数是随机的。

![在跑第77次时停](http://upload-images.jianshu.io/upload_images/3176291-e04d6167f330523d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样就能产生死锁的原因是synchronized这个关键字，是同步的意思。程序开了两个线程，如在代码分析里所示的，object a和object b在函数调用的过程中形成了一种头尾相接的循环等待资源关系，将object a, b上锁，两边同时等待解锁而陷入僵局，造成死锁。
