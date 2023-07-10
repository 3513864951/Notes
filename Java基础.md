## Java基础之——线程

> Java使用 `java.lang.Thread` 类代表线程，**所有的线程对象都必须是 Thread 类或其子类的实例**。每个线程的作用是完成一定的任务，实际上就是执行一段程序流即一段顺序执行的代码。Java 使用线程执行体来代表这段程序流。

### 1、什么是进程？什么是线程？

进程是:一个应用程序（1个进程是一个软件）。

线程是：一个进程中的执行场景/执行单元。

注意：**一个进程可以启动多个线程。**

### 2、进程和线程是什么关系？

**进程**：可以看做是现实生活当中的公司。

**线程**：可以看做是公司当中的某个员工。

==**注意：**进程A和进程B的 **内存独立不共享**。==

**eg.**
			魔兽游戏是一个进程
			酷狗音乐是一个进程
			这两个进程是独立的，不共享资源。

#### 线程A和线程B是什么关系？

**在java语言中：**

​	线程A和线程B，堆内存 和 方法区 内存共享。但是 栈内存 独立，一个线程一个栈。

**eg.**
			假设启动10个线程，会有10个栈空间，每个栈和每个栈之间，互不干扰，各自执行各自的，这就是多线程并发。

**eg.**
			火车站，可以看做是一个**进程**。
			火车站中的每一个售票窗口可以看做是一个**线程**。
			我在窗口1购票，你可以在窗口2购票，你不需要等我，我也不需要等你。所以多线程并发可以提高效率。

​	**进程**可以理解为就是一个软件，**线程**则就是这个软件中的一些功能，多个线程同时工作，才能使得进程工作。

### 3.创建线程类（方法一）

1. 定义 Thread 类的子类，**并重写该类的 run() 方法**，**该 run() 方法的方法体就代表了线程需要完成的任务**，因此把 run() 方法称为线程执行体。
2. 创建 Thread 子类的实例，即创建线程对象。
3. 调用线程对象的 start() 方法来启动该线程。

```java
// 定义线程类
public class MyThread extends Thread{
	public void run(){
	
	}
}
// 创建线程对象
MyThread t = new MyThread();
// 启动线程。
t.start();
```

**eg.**

```java
public class MyThread extends Thread {
 
    /**
     * run () 方法就是子线程要执行的代码
     */
    @Override
    public void run() {
        System.out.println("这是子线程打印的东西");
    }
 
    /**
     * 主线程 main
     */
    public static void main(String [] args) {
        System.out.println("JVM 启动 main线程，main线程执行了main方法");
        // 创建子线程对象
        MyThread thread = new MyThread();
        // 启动线程----启动线程后，会自动执行自定义类中的 run 方法
        thread.start();
        System.out.println("mian 主线程执行结束");
    }
}
```

运行结果：

```java
JVM 启动 main线程，main线程执行了main方法
mian 主线程执行结束
这是子线程打印的东西
```

### 4.创建线程（方法二）

**使用 Thread(Runnable target) 构造方法，创建对象**

编写一个类，**实现** **`java.lang.Runnable`** 接口，**实现`run方法`**。

1.定义 Runnable 接口的实现类，并重写该接口的 run() 方法，该 run() 方法的方法体同样是该线程的线程执行体。
		2.创建 Runnable 实现类的实例，并以此实例作为 Thread 的 target 来创建 Thread 对象，该 Thread 对象才是真正的线程对象。
		3.调用线程对象的 start() 方法来启动线程。

```java
// 定义一个可运行的类
public class MyRunnable implements Runnable {
	public void run(){
	
	}
}
// 创建线程对象
Thread t = new Thread(new MyRunnable());
// 启动线程
t.start();

```

**eg.**

```java
public class ThreadTest03 {
    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable()); 
        // 启动线程
        t.start();
        
        for(int i = 0; i < 100; i++){
            System.out.println("主线程--->" + i);
        }
    }
}

// 这并不是一个线程类，是一个可运行的类。它还不是一个线程。
class MyRunnable implements Runnable {
    @Override
    public void run() {
        for(int i = 0; i < 100; i++){
            System.out.println("分支线程--->" + i);
        }
    }
}

```

#### **采用匿名内部类创建：**

```java
public class ThreadTest04 {
    public static void main(String[] args) {
        // 创建线程对象，采用匿名内部类方式。
        Thread t = new Thread(new Runnable(){
            @Override
            public void run() {
                for(int i = 0; i < 100; i++){
                    System.out.println("t线程---> " + i);
                }
            }
        });

        // 启动线程
        t.start();

        for(int i = 0; i < 100; i++){
            System.out.println("main线程---> " + i);
        }
    }
}

```

> **注意：**第二种方式实现接口比较常用，因为一个类实现了接口，它还可以去继承其它的类，更灵活。

## Java基础之——内部类

### 1.介绍

内部类就是定义在一个类里的类，里面的类可以理解成（寄生），外部类可以理解成（宿主）

### 2.使用场景

+ 当一个事物的内部，还有一个部分需要一个完整的结构进行描述，而这个内部的完整的结构又只是为外部事物提供服务，那么整个内部的完整结构可以选择使用内部类来设计。
+ 内部类通常可以方便访问外部类的成员，包括私有的成员。[深入理解Java中为什么内部类可以访问外部类的成员](https://blog.csdn.net/zhangjg_blog/article/details/20000769)
+ 内部类提供了更好的封装性，内部类本身就可以用private protected 等修饰，封装性可以做更多控制。

### 3.静态内部类

> 有static修饰，属于外部类本身
>
> 它的特点和使用与普通类是完全一样的，类有的成分它都有，只是位置在别人里面而已

#### 3.1 定义格式

```java
public class Outer{
    // 静态成员内部类
    public static class Inner{}
}
```

#### 3.2 创建对象的格式

```java
外部类名.内部类名 对象名 = new 外部类名.内部类构造器;
Outer.Inner in = new Outer.Inner();
```

#### 3.3 静态内部类的访问拓展

- 静态内部类中是否可以直接访问外部类的静态成员？

  可以，外部类的静态成员只有一份可以被共享访问

- 静态内部类中是否可以直接访问外部类的实例成员？

  不可以的，外部类的实例成员必须用外部类对象访问

### 4.成员内部类

> 无static修饰，属于外部类的对象
>
> JDK16之前，成员内部类中不能定义静态成员，JDK16开始也可以定义静态成员了

#### 4.1 定义格式

```java
public class Outer{
    // 成员内部类
    public class Inner{}
}
```

#### 4.2 创建对象的格式

```java
外部类名.内部类名 对象名 = new 外部类构造器.new 内部构造器();
Outer.Inner in = new Outer().new Inner();
```

#### 4.3 成员内部类的访问扩展

- 成员内部类中是否可以直接访问外部类的静态成员

  可以，外部类的静态成员只有一份可以被共享访问

- 成员内部类的实例方法中是否可以直接访问外部类的实例成员

  可以的，因为必须现有外部类对象，才能有成员内部类对象，所以可以直接访问外部类对象的实例成员

### 5.匿名内部类

> 本质上是一个没有名字的局部内部类，定义在方法中、代码块中等

#### 5.1 作用

方便创建子类对象，最终目的为了简化代码编写

#### 5.2 格式

```java
new 外部类().方法(new 接口|抽象类|父类(){
  接口方法的实现();
  或
  重写抽象类或父类的方法() ;
}) ;
```

```java
接口名称 对象名 = new 接口名称() { 
     重写抽象方法; 
}
Animal a = new Animal(){
    public void run(){}
};
a.run();
```

#### 5.3 特点

- 匿名内部类是一个没有名字的内部类
- 匿名内部类写出来就会产生一个匿名内部类的对象
- 匿名内部类的对象类型相当于是当前new的那个的类型的子类类型

