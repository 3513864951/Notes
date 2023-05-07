[toc]



## 一、*类与对象*

#### **1.概述**

![image-20221021220345355](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072124323.png)

**类似于C语言中的结构体**

#### **2.示例：**

```java
public class Text{
    public static void main(String[] args){
        Cat cat1=new Cat();//创建第一只猫Cat1，C爱他为对象名new Cat()为对象空间即对象
        Cat1,name="小白";
        Cat1.age=3;
        Cat.color="白色";
    }
}
class Cat{
    String name;
    int age;
    String color;
}
```

#### **3.对象内存布局**

![image-20221021223322973](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072124063.png)

#### **4.细节**

(1)属性的定义类型可以为任意类型，包括基本类型和引用类型。

(2)属性如果不赋值，规则跟数组一样。

#### **5.对象的创建**

![image-20221021225234788](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072124348.png)

#### **6.对象分配机制**

![image-20221021225722828](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072124040.png)

## 二、成员方法

#### 1.基本介绍

![image-20221021232800178](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072124863.png)

**近似C语言中自己定义的函数**

#### 2.方法创建与调用



```java
class Person {
    String name;
	int age ;
	//方法(成员方法) 
	//添加speak成员方法,输出“我是一个好人”
	//1. public 表示方法是公开
	//2.void:表示方法没有返回值
	//3. speak() : speak是方法名， () 形参列表
	//4. {}方法体，可以写要执行的代码
	//5. System . out.println("我是一个好人");表示我们的方法就是输出一句话
	public void speak() {
		System.out.println("我是一个好人");
        
    //添加ca102成员方法,该方法 可以接收一个数n，计算从1+..+n 的结果
	public void ca102(int n) {
		int res = 0;
		for(inti=1;i<=n;i++){
			res += i;
			}
		System.out.println("ca102方法计算结果=" + res);
		}
	//添加getSum成员方法,可以计算两个数的和
	public int getSum(int num1, int num2) {
		int	res=num1+num2;
		return res;
}

    }

```

**调用方法**：*对象名.方法名();*//跨类调用时

例如：*person1.speak();*

#### 3.成员方法的定义

![image-20221022001616885](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072124398.png)

#### 4.注意事项及细节

返回数据类型
1.一个方法最多有一个返回值<u>(*如果要返回多个值，可以返回数组*)</u>
2.返回类型可以为任意类型，包含基本类型或引用类型(数组，对象)
3.如果方法要求有返回数据类型，则方法体中最后的执行语句必须为return值;而
且要求返回值类型必须和return的值类型一 致或兼容
4.如果方法是void,则方法体中可以没有return语句，或者只写return ;
方法名
遵循驼峰命名法，最好见名知义，表达出该功能的意思即可，比如得到两个数的和
*getSum*,开发中按照规范

```java
//如何返回多个结果返回数组
public static void main(String[] args) {
	AA a = new AA();
	int[] res = a. getSumAndsub(1, 4);
    System.out.println("和=" + res[0]);
	System.out.println("差=" + res[1]);
}
class AA {
public int[ ] getSumAndSub(int n1, int n2) {
int[] resArr = new int[2]; // 创建一个数组
resArr[0] = n1+n2;
resArr[1] = n1 - n2;
return resArr ;
}

```

```java
//同一个类中的方法调用:直接调用即可，不需要创建对象
class A {
public void print(int n) {
System.out.println("print()方法被调用n="+ n);
}
public void sayOk(){ //say0k调用print(直 接调用即可)
print(10);
}
//跨类中B类调用A类需要创建对象
class B {
	public void hi() {
		A.a=new A();//创建对象
        a.sayOK();
}

```

## 三、成员方法传参机制WAITING``````

*<u>与C语言中函数的传参一致</u>*

对于基本数据类型，传递的是值(值拷贝) ,形参的任何改变不影响实参!

对于引用类型传递的是地址(传递也是值，但是值是地址) ,可以通过形参影响实参!

## 四、方法重载（OverLoad）

#### 1.基本介绍

![image-20221022095852293](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072124658.png)

#### 2.示例：

```java
class MyCalculator {
//两个整数的和
	public int calculate(int n1, int n2) {
		return n1 + n2;
	}
	//一个整数，-个double的和
	public double calculate(int n1, double n2) {
	return n1 + n2;
	}
	//一个double ，一个Int和
	public double calculate(double n1, int n2) {
	return n1 + n2;
	}
	//三个int的和
	public int calculate(int n1, int n2,int n3) {
	return n1 + n2 + n2;
}

```

#### 3.注意事项与细节

1)方法名:必须相同
<u>2)参数列表:必须不同(参数类型或个数或顺序，至少有一样不同，参数名无要求)</u>
3)返回类型:无要求

## 五、可变参数

#### 1.基本概念

java允许将同个类中多个同名同功能但参数个数不同 的方法， 封装成一个方法。
就可以通过可变参数实现

#### 2.基本语法及示例

访问修饰符 返回类型 方法名(数据类型... 形参名) {   }

```java
public int sum(int n1, int n2) {//2个数的和
return n1+n2;
}
public int sum(int n1, int n2, int n3) {//3个数的和 
return n1 + n2 + n3;
}
public int sum(int n1, int n2, int n3, int n4) {//4个数的和
return n1 +n2+n3+n4;
}
//.....
//可变参数改进——>>
//1.int... 表示接受的是可变参数，类型是int ,即可以接收多个int(0-多)
//2.使用可变参数时，可以当做数组来使用即nums 可以当做数组
//3.遍历nums求和即可
public int sum(int... nums) {
	//System . out . println("接收的参数个数=”+ nums . length);
	int res = 0;
	for(int i = 0; i < nums . length; i++) {
		res+= nums[i];
	}
	return res ;
}

```

#### 3.注意事项与使用细节

1)可变参数的实参可以为0个或任意多个。
2)可变参数的实参可以为数组。int arr[]={1,2,3};	sum(arr);
3)可变参数的本质就是数组.
4)可变参数可以和普通类型的参数-起放在形参列表，但必须保证可变参数在最后

```java
public void test(String str,double... nums){
    
}
```

5)一个形参列表中只能出现一个可变参数

## 六、作用域Scope

#### 1.java中作用域的分类

全局变量:也就是属性，作用域为整个类体Cat类: cry eat等方法使用属性

局部变量:也就是除了属性之外的其他变量，作用域为定义它的代码块中!
全局变量(属性)可以不赋值，直接使用，因为有默认值，局部变量必须赋值后，
才能使用，因为没有默认值。

![image-20221022113025393](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072124985.png)

#### 2.注意事项与使用细节

1.属性和局部变量可以重名，访问时遵循就近原则。
2.在同一个作用域中，比如在同一个成员方法中，两个局部变量，不能重名。

![image-20221022143942884](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072124751.png)

3.属性生命周期较长，伴随着对象的创建而创建，伴随着对象的死亡而死亡。局部变
量，生命周期较短，伴随着它的代码块的执行而创建，伴随着代码块的结束而死亡。
即在一次方法调用过程中。

4.作用域范围不同
全局变量/属性:可以被本类使用，或其他类使用(通过对象调用)
局部变量:只能在本类中对应的方法中使用
5.修饰符不同
全局变量/属性可以加修饰符
局部变量不可以加修饰符

## 七、构造器(constructor)

#### 1.基本介绍


构造方法又叫构造器(constructor),是类的一种特殊的方法，它的主要作用是
完成对***新对象的初始化***(不是创建对象)。它有几个特点:
*1)方法名和类名相同**,也不能写void*
*2)没有返回值*
*3)在创建对象时，系统会自动的调用该类的构造器完成对对象的初始化。*

#### 2.基本语法

 [修饰符]方法名(形参列表){
方法体;                                                                                 																		    }
**说明:**
1)构造器的修饰符可以默认，也可以是public protected private
2)构造器没有返回值,*也不能写void*
3)方法名和类名字必须-样
4)参数列表和成员方法一样的规则
5)构造器的调用由系统完成

#### 3.示例

```java
public static void main(String[] args) {
//当我们new一个对象时，直接通过构造器指定名字和年龄
Person p1 = new Person("smith"，80); 
}
//在创建人类的对象时，就直接指定这个对象的年龄和姓名
//
class Person {
	String name;
	int age;
	//构造器
	//1. 构造器没有返回值，也不能写void
	//2.构造器的名称和类Person一样
	//3. (String pName, int pAge) 是构造器形参列表，规则和成员方法样
	public Person(String pName, int pAge) {
		name = pName ;
	age = pAge;
	}
}

```

#### 4.注意事项与使用细节

1.一个类可以定义多个不同的构造器，即构造器重载
比如:给上图Person类定义一个构造器用来创建对象的时候,只指定人名，不需要指定年龄

```java
class Person{
    String name;
    int age;
    public Person(String pName, int pAge) {
		name = pName ;
	age = pAge;
	}
    public Person(String pname){
        name=pname;
    }
}
```

2.构造器名和类名要相同
3.构造器没有返回值
4.构造器是完成对象的初始化，并不是创建对象
5.在创建对象时,系统自动的调用该类的构造方法

6.如果程序员没有定义构造器，系统会自动给类生成一个默认无参构造器(也叫默认构造器)，比如Person 0{},使用javap指令反编译看看
7.一旦定义了自己的构造器,默认的构造器就覆盖了，就不能再使用默认的无参构造器（即不能new Person()中的括号不放东西），除非显式的定义一下,即: Person(）{}

#### 5.对象创建流程分析

![image-20221023112828918](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072124619.png)

1）先在方法区加载类信息	2）在堆开辟空间并默认初始化即age=0；name=null；	3）之后再赋值age=90	4)构造器初始化

## 八、this关键字

#### 1.介绍与示例

运用this可以使构造器可以在括号中使用类中的属性名来命名局部变量

哪个对象调用this，this就代表哪个对象



```java
class Dog{ //类
	String name;
	int age ;
	public Dog(String name, int age){//构造器
	//this.name就是当前对象的属性name,而不是局部变量
      this.name = name;
	//this.age就是当前对象的属性age，而不是局部变量
	this.age = age;
}
```

#### 2.this内存

![image-20221023132302230](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072124083.png)

#### 3.注意事项与使用细节

1. this关键字可以用来访问本类的属性(不受构造器中局部变量的影响，输出类语句块中的全局变量)、方法、构造器

2. this用于区分当前类的属性和局部变量

3. 访问成员方法的语法: <u>this.方法名(参数列表);</u>

4. 访问<u>构造器</u>语法: this(参数列表); 注意只能在构造器中使用,且必须放在第一条语句

5. this不能在类定义的外部使用，只能在类定义的方法中使用。

   例题：●定义Person类，里面有name、age属性，并提供compareTo比较方法，用于判断
   是否和另一个人相等，提供测试类TestPerson用于测试，名字和年龄完全一样，就
   返回true,否则返回false

```java
public class HI{
    public static void main(String[] args){
        person p1=new person("wwww",122);
        person p2=new person("wwww",1212);
System.out.println("p1与p2比较"+p1.compareto(p2));
//p1.compareto代表引用的是p1中的compareto方法，this就代表着对象p1
    }
}
class person{
    String name;
    int age;
    public person(String name,int age){
        this.name=name;
        this.age=age;
    }
    public boolean compareto(person p){
        return this.name.equals(p.name)&&this.age==p.age;
    }

```

```java
public class HI{
    public static void main(String[] args){
       
        book B=new book("归还借款",176);
      System.out.println(B.prince());
    }
}
class book{
    String name;
    int value;
    public book(String name,int value){
        this.name=name;
        this.value=value;
    }
    public int prince(){
        if(this.value>100&&this.value<=150)   //以下this可加可不加
            this.value=100;
        else if(this.value>150)
            this.value=150;
        return this.value;
    }
}

```

## 九、包

#### 1.包的三大作用

引入包的原因就是使用包下面的类

1.区分相同名字的类
2.当类很多时可以很好的管理类[看Java API文档]
3.控制访问范围

#### 2.包基本语法

package com.hspedu; 
说明:

1. package 关键字，表示打包.
2. com.hspedu: 表示包名



#### 3.包的本质分析

![image-20221026212036390](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072124742.png)



![image-20221026212108380](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072124373.png)

类似头文件？

#### 4.包的命名

V命名规则:
只能包含数字、字母、下划线、 小圆点.但不能用数字开头，不能是关键字或保留字
demo.class.exec1 //错误class是关键字
demo.12a //错误12a是数字开头，12a是文件夹demo的子文件名
demo.ab12.oa //对
命名规范
一般是小写字母+小圆点-般是
com.公司名项目名业务模块名
比如: com.hspedu.oa.model; com.hspedu.oa.controller;
举例:
com.sina.crm.user //用户模块
com.sina.crm.order //订单模块
com.sina.crm.utils //工具类

#### 5.注意事项和使用细节

1. package 的作用是声明当前类所在的包，需要放在class的最上面，一个类中
   最多只有一句package
2. import指令 位置放在package的下面，在类定义前面，可以有多句且没有顺序
   要求。

## IDEA

## 1.快捷键

1.删除当前行，自己配置 **ctrl+ d**
2.复制当前行，自己配置 **ctrl + alt +向下光标**
3.补全代码 **alt + /**
4.添加注释和取消注释 **ctrl + /** [第次是添加注释，第二次是取消注释]

5.导入该行需要的类先配置auto import ,然后使用 **alt+ enter** 即可

6.快速格式化代码 **ctrl + alt + L**
7.快速运行程序 **alt + R**

8.生成构造方法等 **alt + insert** [提高开发效率]
9.查看一个类的层级关系 **ctrl + H** [学习继承后，非常有用]
10.将光标放在个方法上，输入 **ctrl + B**,可以选择定位到哪个类的方法[学继承后，非常有用]
11.自动的分配变量名,通过在后面 **.var**

## 访问修饰符

#### 访问范围和注意事项

![image-20221027113307902](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072123494.png)

## 十、封装

#### 1.基本介绍

封装(encapsulation)就是把抽象出的数据**[属性]**和对数据的操作**[方法]**封装在一起，数
据被保护在内部,程序的其它部分只有通过被授权的操作**[方法]**,才能对数据进行操作。

不能直接赋值。（p1.name="jack";）

![image-20221027194809689](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072123291.png)

#### 2.好处

1)隐藏实现细节:方法(连接数据库) <--调用(传入参数..)
2)可以对数据进行验证，保证安全合理

#### 3.步骤（三步）

1)将属性进行私有化private[不能直接修改属性]
2)提供一个公共的（public）set方法，用于对属性判断并赋值
public void setXxx(类型 参数名){//Xxx表示某个属性
	//加入数据验证的业务逻辑
	属性=参数名;
}
3)提供一个公共的get方法，用于获取属性的值
public 数据类型 getXxx(){ //权限判断	Xxx代表属性
	return XX; 
}

#### 4.通过构造器赋值仍能保证私有化

```java

class Person{
    public String name;
    private int age;
    private double salary;
    public Person(){
        
    }
	public Person(String name, int age, double saLary) {//构造器

		this.setName (name) ;
		this.setAge(age);
		this.setSaLary(saLary);
		}
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }
}

```

## 十一、继承

#### 1.

 ![image-20221027205601491](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072123766.png)

体现:一旦子类A继承父类B以后，子类A中就获取了父类B中声明的所有的属性和方法。

特别的，父类中声明为private的属性或方法，子类继承父类以后，仍然认为获取了父类中私有的结构。只有因为封装性的影响，使得子类不能直接调用父类的结构而已。

#### 2.继承的格式

![image-20221028102937314](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072123287.png)

#### 3.规则

![image-20221028103051328](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072123270.png)

![image-20221028103415339](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305072123505.png)

如：间接父类有睡觉，直接父类继承间接父类，子类也可以使用睡觉

## 重写

#### 1.介绍

1.重写:子类继承父类以后，可以对父类中同名同参数的方法，进行覆盖操作
2.应用:重写以后，当创建子类对象以后，通过子类对象调用子父类中的同名同参数的方法时，实际执行的是子类重写父类的方法。（否则干嘛重写，直接调用父类不就好了？）

#### 2.重写的规定:

方法的声明: **权限修饰符 返回值类型 方法名(形参列表){**
**/ /方法体**

**}**

约定俗称:子类中的叫重写的方法，父类中的叫被重写的方法

①子类重写的方法的方法名和形参列表与父类被重写的方法的**方法名和形参列表**相同
②子类重写的方法的**权限修饰符不小于父类**被重写的方法的权限修饰符

> 特殊情况:子类不能重写父类中声明为private权限的方法

③返回值类型:

>父类被重写的方法的返回值类型是void,则子类重写的方法的返回值类型只能是void
>父类被重写的方法的返回值类型（针对引用数据类型）是A类型，则子类重写的方法的返回值类型可以是A类或A类的子类。

父类被重写的方法的返回值类型是基本数据类型(比如: doub1e), 则子类重写的方法的返回值类型必须是相同的基本数据类型（double）

 
