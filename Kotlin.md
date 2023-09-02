```kotlin
class Student(val sno: String, val grade: Int, name: String, age: Int) :
	 Person(name, age) {
 constructor(name: String, age: Int) : this("", 0, name, age) {
     
 	}
 constructor() : this("", 0) {
     
 	}
}
```

+ 分析：constructor(name: String, age: Int) : this("", 0, name, age) {		}

> 这个是 Student 类的一个辅助构造函数。我们来分析一下:
>
> 1. constructor 表示这是一个构造函数。
>
> 2. 它接受 name 和 age 两个参数。
>
> 3. 使用 this 调用 Student 的主构造函数。
>
> 4. 传入的参数分别是:
>
> - ""  表示 empty string
> - 0  表示年级 0 
> - name 和 age ,传给主构造函数的参数。
>
> 5. 作用是:使用 name 和 age 初始化 Student 对象,同时 year 和 sno 使用默认值。
>
> 对应的用法是:
>
> ```kotlin
> val student = Student("John", 18)
> ```
>
> 等价于:
>
> ```kotlin
> val student = Student("", 0, "John", 18)
> ```
>
> 因此,通过这个辅助构造函数,我们可以只传入 name 和 age 这两个参数,将 sno 和 year 使用默认值。
>
> 主要注意几点:
>
> - 辅助构造函数通过 this 调用主构造函数,从而初始化对象。
> - 可以只传部分参数给主构造函数,其他使用默认值。
> - 这样可以提供多个构造方式,满足不同初始化需求。
>
> 希望我的分析能为你解惑!如果还有任何疑惑,欢迎随时提问:)

```kotlin
MainActivity.class
MainActivity::class.java
```





### val

在 Kotlin 中，`val` 是用来声明只读属性（read-only property）的关键字。`val` 声明的属性是不可变的，一旦初始化后就不能再被修改。

### 空指针

#### 可空类型系统

```kotlin
fun doStudy(study: Study) {
 study.readBooks()
 study.doHomework()
}
```

由于Kotlin默认所有的参数和变量都不可为空，所以这里传入的Study参数也一定不会为空，如果传入null会报错

#### 如何传入`null`参数

**只需要在类名的后面加上一个问号。**比如，Int表示不可为空的整型，而`Int?`就表示可为空的整型；String表示不可为空的字符串，而

`String?`就表示可为空的字符串。

```kotlin
fun doStudy(study: Study?) {
 study.readBooks()
 study.doHomework()
}
```

这样我们就可以传入`null`了，但通过study调用两个方法会报错，因为study可能为空，那要怎么解决呢？这就需要判空辅助工具的帮助了。

### 判空辅助工具

#### ①操作符 ?.

最常用的工具是`?.`操作符。这个操作符的作用非常好理解，就是当对象不为空时正常调用相应的方法，当对象为空时则什么都不做。比如以下的判空处理代码：

```kotlin
if (a != null) {
 a.doSomething()
}
```

以上代码就可以转化成：

```kotlin
a?.doSomething()
```

之前的代码就可以这样写：

```kotlin
fun doStudy(study: Study?) {
 study?.readBooks()
 study?.doHomework()
}
```

#### ②操作符 ?:

接下来学习另外一个非常常用的`?:`操作符。这个操作符的左右两边都接收一个表达式，如果左边表达式的结果不为空就返回左边表达式的结果，否则就返回右边表达式的结果。观察

如下代码：

```kotlin
val c = if (a ! = null) {
 	a
} else {
 	b
}
```

以上代码就可以简化成：

```kotlin
val c = a ?: b
```

接下来我们通过一个具体的例子来结合使用?.和?:这两个操作符，从而让你加深对它们的理

解。

比如现在我们要编写一个函数用来获得一段文本的长度，使用传统的写法就可以这样写：

```kotlin
fun getTextLength(text: String?): Int {

 if (text != null) {

 return text.length

 }

 return 0

}
```

由于文本是可能为空的，因此我们需要先进行一次判空操作，如果文本不为空就返回它的长度，如果文本为空就返回0。

这段代码看上去也并不复杂，但是我们却可以借助操作符让它变得更加简单，如下所示：

```kotlin
fun getTextLength(text: String?) = text?.length ?: 0
```

这里我们将?.和?:操作符结合到了一起使用，首先由于text是可能为空的，因此我们在调用它的length字段时需要使用?.操作符，而当text为空时，text?.length会返回一个null值，这个时候我们再借助?:操作符让它返回0。

#### ③工具 !!

不过Kotlin的空指针检查机制也并非总是那么智能，有的时候我们可能从逻辑上已经将空指针异常处理了，但是Kotlin的编译器并不知道，这个时候它还是会编译失败。

在这种情况下，如果我们想要强行通过编译，可以使用非空断言工具，写法是在对象的后面加

上`!!`，如下所示：

```kotlin
fun printUpperCase() {

 val upperCase = content!!.toUpperCase()

 println(upperCase)

}
```

这是一种有风险的写法，意在告诉Kotlin，我非常确信这里的对象不会为空，所以不用你来帮我做空指针检查了，如果出现问题，你可以直接抛出空指针异常，后果由我自己承担。

虽然这样编写代码确实可以通过编译，但是当你想要使用非空断言工具的时候，最好提醒一下自己，是不是还有更好的实现方式。你最自信这个对象不会为空的时候，其实可能就是一个潜在空指针异常发生的时候。

#### ④工具 let

let既不是操作符，也不是什么关键字，而是一个函数。这个函数提供了函数式API的编程接口，并将原始调用对象作为参数传递到

`Lambda`表达式中。示例代码如下：

```kotlin
obj.let { obj2 ->

 // 编写具体的业务逻辑

}
```

可以看到，这里调用了obj对象的let函数，然后Lambda表达式中的代码就会立即执行，并且这个obj对象本身还会作为参数传递到Lambda表达式中。不过，<u>为了防止变量重名，这里我将参数名改成了obj2，但实际上它们是同一个对象</u>，这就是let函数的作用。

而`let`函数的特性配合上`?.`操作符在空指针排除上有巨大作用。

比如之前的代码：

```kotlin
fun doStudy(study: Study?) {
 study?.readBooks()
 study?.doHomework()
}
```

就可以改成：

```kotlin
fun doStudy(study: Study?) {
  study?.let { stu ->
     stu.readBooks()
     stu.doHomework()
  }
}
```

分析以上代码：`?.`操作符表示对象为空时什么都不做，对象不为空时就调用`let`函数，而`let`函数会将study对象本身作为参数传递到Lambda表达式中，此时的study对象肯定不为空。

这段代码还可以继续简化，Lambda有一个特性：**当Lambda表达式的参数列表中只有一个参数时，可以不用声明参数名，直接使用`it`关键字来代替即可**，那么代码就可以进一步简化成：

```kotlin
fun doStudy(study: Study?) {

 study?.let {

 it.readBooks()

 it.doHomework()

 }

}
```

**`let`函数是可以处理全局变量的判空问题的，而if判断语句则无法做到这一点。**比如我们将doStudy()函数中的参数变成一个全局变量，使用let函数仍然可以正常工作，但使用if判断语句则会提示错误:

![image-20230801233333890](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202308012333123.png)

之所以这里会报错，是因为全局变量的值随时都有可能被其他线程所修改，即使做了判空处理，仍然无法保证if语句中的study变量没有空指针风险。从这一点上也能体现出let函数的优势。



### 字符串内嵌表达式

Kotlin中字符串内嵌表达式的语法规则：

```kotlin
"hello, ${obj.name}. nice to meet you!"
```

可以看到，**Kotlin允许我们在字符串里嵌入`${}`这种语法结构的表达式，并在运行时使用表达式执行的结果替代这一部分内容。**另外，当表达式中仅有一个变量的时候，还可以将两边的大括号省略，如下所示：

```kotlin
"hello, $name. nice to meet you!"
```

接下来给出范例：

```kotlin
val brand = "Samsung"
val price = 1299.99
println("Cellphone(brand=" + brand + ", price=" + price + ")")
```

以上代码就可以写成以下形式：

```kotlin
val brand = "Samsung"
val price = 1299.99
println("Cellphone(brand=$brand, price=$price)")
```

### 函数的参数默认值

我们可以在定义函数的时候给任意参数设定一个默认值，这样当调用此函数时就不会强制要求调用方为此参数传值，在没有传值的情况下会自动使用参数的默认值。

给参数设定默认值的方式也很简单，观察如下代码：

```kotlin
fun printParams(num: Int, str: String = "hello") {
 println("num is $num , str is $str")
}
```

这里我们给`printParams()`函数的第二个参数设定了一个默认值，这样当调用`printParams()`函数时，可以选择给第二个参数传值，也可以选择不传，在不传的情况下就会自动使用默认值。

将代码改成给第一个参数设定默认值，如下所示：

```kotlin
 printParams(num: Int = 100, str: String) {
 println("num is $num , str is $str")
}
```

这时如果想让num参数使用默认值该怎么办呢？<u>模仿刚才的写法肯定是行不通的，因为编译器会认为我们想把字符串赋值给第一个num参数，从而报类型不匹配的错误.</u>

不过不用担心，**Kotlin提供了另外一种神奇的机制，就是可以通过键值对的方式来传参,比如调用`printParams()`函数，我们还可以这样写：**

```kotlin
printParams(str = "world", num = 123)
```

此时哪个参数在前哪个参数在后都无所谓，Kotlin可以准确地将参数匹配上。而使用这种键值对的传参方式之后，我们就可以省略num参数了，代码如下：

```kotlin
fun printParams(num: Int = 100, str: String) {

 println("num is $num , str is $str")

}

fun main() {

 printParams(str = "world")

}
```

之前我们在学习次构造函数时了解到次构造函数不长使用，那种写法在Kotlin中其实是不必要的，因为我们完全可以通过只编写一个主构造函数，然后给参

数设定默认值的方式来实现，代码如下所示：

```kotlin
class Student(val sno: String = "", val grade: Int = 0, name: String = "", age: Int = 0) :

 Person(name, age) {

}
```

在给主构造函数的每个参数都设定了默认值之后，我们就可以使用任何传参组合的方式来对Student类进行实例化。

### `Intent`启动`Activity`

```kotlin
 companion object {
 fun actionStart(context: Context, data1: String, data2: String) {
 	val intent = Intent(context, SecondActivity::class.java)
	intent.putExtra("param1", data1)
 	intent.putExtra("param2", data2)
 	context.startActivity(intent)
 	}
 }
```

调用的话很简单：

```kotlin
XXXActivity.actionStart(this,data1,data2)
```

```kotlin
inline fun <reified T> startActivity(context: Context) {
     val intent = Intent(context, T::class.java)
     context.startActivity(intent)
}
```

如果我们想要启动TestActivity，只需要这样写就可以了：

```kotlin
startActivity<TestActivity>(context)
```

不过，现在的startActivity()函数其实还是有问题的，因为通常在启用Activity的时候还可能会使用Intent附带一些参数，比如下面的写法：

```kotlin
val intent = Intent(context, TestActivity::class.java)
intent.putExtra("param1", "data")
intent.putExtra("param2", 123)
context.startActivity(intent)
```

这个问题也不难解决，只需要借助之前在第6章学习的高阶函数就可以轻松搞定。回到reified.kt文件当中，这里添加一个新的startActivity()函数重载，如下所示：

```kotlin
inline fun <reified T> startActivity(context: Context, block: Intent.() -> Unit) {
 val intent = Intent(context, T::class.java)

 intent.block()

 context.startActivity(intent)

}
```

可以看到，这次的startActivity()函数中增加了一个函数类型参数，并且它的函数类型是定义在Intent类当中的。在创建完Intent的实例之后，随即调用该函数类型参数，并把Intent的实例传入，这样调用startActivity()函数的时候就可以在Lambda表达式中为Intent传递参数了，如下所示：

```kotlin
startActivity<TestActivity>(context) {

 putExtra("param1", "data")

 putExtra("param2", 123)

}
```

### 标准函数和静态方法

#### ①函数 `with`

with函数接收两个参数：第一个参数可以是一个任意类型的对象，第二个参数是一个Lambda表达式。with函数会在Lambda表达式中提供第一个参数对象

的上下文，并使用Lambda表达式中的最后一行代码作为返回值返回。示例代码如下：

```kotlin
val result = with(obj) {//注意 这里是Lambda表达式的简化（当Lambda表达式是最后一个参数是可以拿到括号外面）

 // 这里是obj的上下文，意思就是在这里能直接调用obj的函数就行，默认是obj调用的

 "value" // with函数的返回值

}
```

**用处：**它可以在连续调用同一个对象的多个方法时让代码变得更加精简。

例如：

```kotlin
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape")
val builder = StringBuilder()
builder.append("Start eating fruits.\n")
for (fruit in list) {
	builder.append(fruit).append("\n")
}
builder.append("Ate all fruits.")
val result = builder.toString()
println(result)
```

就可以简化成：

```kotlin
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape")
val result = with(StringBuilder()) {
 append("Start eating fruits.\n")
 for (fruit in list) {
 	append(fruit).append("\n")
 }
 append("Ate all fruits.")
 toString()
}
println(result)
```

#### ②函数 `run`

> run函数的用法和使用场景其实和with函数是非常类似的，只是稍微做了一些语法改动而已。

+ `run`函数要在某个对象的基础上调用
+ 只接收一个Lambda参数，并且会在Lambda表达式中提供调用对象的上下文
+ 其他方面和with函数是一样的

格式：

```kotlin
val result = obj.run {
 // 这里是obj的上下文
 "value" // run函数的返回值
}
```

#### ③函数`apply`

+ 和run函数一样，都要在某个对象上调用
+ 只接收一个Lambda参数，也会在Lambda表达式中提供调用对象的上下文
+ 但是apply函数无法指定返回值，而是会自动返回调用对象本身

```kotlin
val result = obj.apply {
 // 这里是obj的上下文
}
// result == obj
```

使用apply函数来修改一下上面的这段代码，如下所示：

```kotlin
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape")
val result = StringBuilder().apply {
 append("Start eating fruits.\n")
	 for (fruit in list) {
		 append(fruit).append("\n")
 }
	 append("Ate all fruits.")
}
println(result.toString())
```

> 由于apply函数无法指定返回值，只能返回调用对象本身，因此这里的result实际上是一个StringBuilder对象，所以我们在最后打印的时候还要再调用它的
>
> toString()方法才行。

> 回想一下刚刚在最佳实践环节编写的启动Activity的代码：
>
> ```kotlin
> val intent = Intent(context, SecondActivity::class.java)
> 
> intent.putExtra("param1", "data1")
> 
> intent.putExtra("param2", "data2")
> 
> context.startActivity(intent)
> ```
>
> 这里每传递一个参数就要调用一次intent.putExtra()方法，如果要传递10个参数，那就得调用10次。对于这种情况，我们就可以使用标准函数来对代码进行精简，如下所示：
>
> ```kotlin
> val intent = Intent(context, SecondActivity::class.java).apply {
> 
>  putExtra("param1", "data1")
> 
>  putExtra("param2", "data2")
> 
> }
> 
> context.startActivity(intent)
> ```
>
> 可以看到，由于Lambda表达式中的上下文就是Intent对象，所以我们不再需要调用intent.putExtra()方法，而是直接调用putExtra()方法就可以了。传递的参数越多，这种写法的优势也就越明显。

### 定义静态方法

#### 方式一：object单例类

通过kotlin的单例类实现：

```kotlin
object Util {
	 fun doAction() {
		 println("do action")
	 }
}
```

+ 缺点：单例类所有方法都是静态方法

#### 方式二：companion object

通过`companion object`实现：

```kotlin
class Util {
 	fun doAction1() {
 		println("do action1")
 	}
 	companion object {
 		fun doAction2() {
 			println("do action2")
 		}
 	}
}
```

这样就只有`doAction2()`方法是静态方法了

上面两种方法已经足够满足日常开发需求量，如果你确确实实需要定义真正的静态方法，**以下两种方法可以实现真正的静态方法：**

#### 方式三：@JvmStatic

<u>前面使用的单例类和companion object都只是在语法的形式上模仿了静态方法的调用方式，实际上它们都不是真正的静态方法。因此**如果你在Java代码中以静态方法的形式去调用的话，你会发现这些方法并不存在。**</u>而如果我们给单例类或companion object中的方法加上`@JvmStatic`注解，那么**Kotlin编译器就会将这些方法编译成真正的静态方法**，如下所示：

```kotlin
class Util {

 fun doAction1() {
 	println("do action1")
 }

 companion object {

 	@JvmStatic
 	fun doAction2() {
 		println("do action2")
 	}
 }
}
```

<span style="background-color:Darkorange ;"><b>注意：</b></span>`@JvmStatic`注解只能加在单例类或companion object中的方法上，如果你尝试加在一个普通方法上，会直接提示语法错误。

#### 方式四：顶层方法

> 顶层方法指的是那些没有定义在任何类中的方法

想要定义一个顶层方法，首先需要创建一个Kotlin文件。对着任意包名右击 → New → KotlinFile/Class，在弹出的对话框中输入文件名即可。注意创建类型要选择File:<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202308041234014.png" alt="image-20230804123417862" style="zoom:50%;" />

点击“OK”完成创建，这样刚刚的包名路径下就会出现一个Helper.kt文件。现在我们在这个文件中定义的任何方法都会是顶层方法，比如这里我就定义一个doSomething()方法吧，如下所示：

```kotlin
fun doSomething() {

 println("do something")

}
```

**kotlin代码中调用：**

```kotlin
doSomething()
```

**java代码中调用：**

```java
HelperKt.doSomething()
```

如果是在Java代码中调用，你会发现是找不到doSomething()这个方法的，因为Java中没有顶层方法这个概念，所有的方法必须定义在类中。于是Kotlin编译器会自动创建一个叫作HelperKt的Java类，doSomething()方法就是以静态方法的形式定义在HelperKt类里面的，因此在Java中使用HelperKt.doSomething()的写法来调用就可以了。

### kotlin中的类型强制转化

关键词：`as`

```kotlin
val activity = context as Activity
```

### 定义常量

关键字：`const`

```kotlin
const val TYPE = 1
```

<span style="background-color:Darkorange ;"><b>注意：</b></span>只能在单例类或companion object或顶层方法才可以使用`const`

### P199	Adapter创建不同布局

```kotlin
class MsgAdapter(val msgList: List<Msg>) : RecyclerView.Adapter<RecyclerView.ViewHolder>() {
    
 inner class LeftViewHolder(view: View) : RecyclerView.ViewHolder(view) {
 	val leftMsg: TextView = view.findViewById(R.id.leftMsg)
 }
    
 inner class RightViewHolder(view: View) : RecyclerView.ViewHolder(view) {
 	val rightMsg: TextView = view.findViewById(R.id.rightMsg)
 }
    
 override fun getItemViewType(position: Int): Int {
 	val msg = msgList[position]
 	return msg.type
 }
    
 override fun onCreateViewHolder(parent: ViewGroup, viewType: Int) = if (viewType ==
 	Msg.TYPE_RECEIVED) {
	 val view = LayoutInflater.from(parent.context).inflate(R.layout.msg_left_item,
 	parent, false)
 	LeftViewHolder(view)
 } else {
 	val view = LayoutInflater.from(parent.context).inflate(R.layout.msg_right_item,
 	parent, false)
	 RightViewHolder(view)
 }
    
 override fun onBindViewHolder(holder: RecyclerView.ViewHolder, position: Int) {
 	val msg = msgList[position]
 	when (holder) {
 		is LeftViewHolder -> holder.leftMsg.text = msg.content
 		is RightViewHolder -> holder.rightMsg.text = msg.content
 		else -> throw IllegalArgumentException()
 	}
 }
    
 override fun getItemCount() = msgList.size
    
}
```

有一些不好判断变量类型的：

```kotlin
val button:Button=findById(R.id.button)
val fragment=
```

### 延迟初始化 `lateinit`

> 当使用kotlin编写代码时，可能会出现定义全局变量对象，明明可以保证对象调用方法时一定不为空，但要满足kotlin语法要求，仍需要通过`?.`来进行判空操作，十分麻烦。解决方法就是对全局变量**`延迟初始化`**

例如下面的代码：

```kotlin
class MainActivity : AppCompatActivity(), View.OnClickListener {
    
    //延迟初始化
 	private lateinit var adapter: MsgAdapter 
    
 	override fun onCreate(savedInstanceState: Bundle?) {
 		...
 		adapter = MsgAdapter(msgList)
 		...
 	}
 	override fun onClick(v: View?) {
 		...
 		adapter.notifyItemInserted(msgList.size - 1)
 		...
 	}
}
```

<u>另外，我们还可以通过代码来判断一个全局变量是否已经完成了初始化，这样在某些时候能够有效地避免重复对某一个变量进行初始化操作，示例代码如下：</u>

```kotlin
class MainActivity : AppCompatActivity(), View.OnClickListener {
    
 	private lateinit var adapter: MsgAdapter
    
 	override fun onCreate(savedInstanceState: Bundle?) {
		...
 		if (!::adapter.isInitialized) {
 			adapter = MsgAdapter(msgList)
 		}
 	...
 	}
    
 }
```

> `::adapter.isInitialized`可用于判断adapter变量是否已经初始化。虽然语法看上去有点奇怪，但这是固定的写法。

### 使用密封类优化代码 `sealed class`

> **<font color=Gray>由于密封类通常可以结合RecyclerView适配器中的ViewHolder一起使用，因此我们就正好借这个机会在本节学习一下它的用法。当然，密封类的使用场景远不止于此，它可以在很多时候帮助你写出更加规范和安全的代码，所以非常值得一学。</font>**
>
> 使用场景：为了满足编译器的要求而编写无用条件分支。



例如：下面有一个Result接口，Success和Failure分别是不同的结果

```kotlin
interface Result
class Success(val msg: String) : Result
class Failure(val error: Exception) : Result
```

接下来再定义一个getResultMsg()方法，用于获取最终执行结果的信息，代码如下所示：

```kotlin
fun getResultMsg(result: Result) = when (result) {

 is Success -> result.msg

 is Failure -> result.error.message

 else -> throw IllegalArgumentException()

}
```

> 以上代码通过判断Result传入的对象的类型来输出不同的数据，但必须要实现`else`语法，否则Kotlin编译器会认为这里缺少条件分支，代码将无法编译通过。但实际上Result的执行结果只可能是Success或者Failure，这个else条件是永远走不到的，所以我们在这里直接抛出了一个异常，只是为了满足Kotlin编译器的语法检查而已。
>
> 在编写else条件时，需要考虑可能出现的新情况<u>。如果新增了一个Unknown类并实现了Result接口，表示未知的执行结果，但是忘记在getResultMsg()方法中添加相应的条件分支，编译器不会提醒我们，而是会在运行时进入else条件中，导致程序抛出异常并崩溃。</u>

密封类的关键字是`sealed class`，它的用法同样非常简单，我们可以轻松地将Result接口改造成密封类的写法：

```kotlin
sealed class Result

class Success(val msg: String) : Result()

class Failure(val error: Exception) : Result()
```

只需要将interface关键字改成了sealed class。另外，由于密封类是一个可继承的类，因此在继承它的时候需要在后面加上一对括号。

改成密封类后你会发现现在getResultMsg()方法中的else条件已经不再需要了，如下所示：

```kotlin
fun getResultMsg(result: Result) = when (result) {

 is Success -> result.msg

 is Failure -> "Error is ${result.error.message}"

}
```

<u>为什么这里去掉了else条件仍然能编译通过呢？这是因为当在when语句中传入一个密封类变量作为条件时，Kotlin编译器会自动检查该密封类有哪些子类，并强制要求你将每一个子类所对应的条件全部处理。这样就可以保证，即使没有编写else条件，也不可能会出现漏写条件分支的情况。</u>



> <span style="background-color:Darkorange ;"><b>注意：</b></span>密封类及其所有子类只能定义在同一个文件的顶层位置，不能嵌套在其他类中，这是被密封类底层的实现机制所限制的。

### 扩展函数

  

> **扩展函数**表示即使在不修改某个类的源码的情况下，仍然可以打开这个类，向该类添加新的函数，相当于在类外部写了一个函数，但绑定到了这个类上面，这个类相当于里面加了这个函数。
>
> **顶层方法**是指在任何类的内部之外定义的方法。这些方法不属于任何类，可以在文件的顶部直接定义。



一段字符串中可能包含字母、数字和特殊符号等字符，现在我们希望统计字符串中字母的数量，你要怎么实现这个功能呢？如果按照一般

的编程思维，可能大多数人会很自然地写出如下函数：

```kotlin
object StringUtil {

     fun lettersCount(str: String): Int {

         var count = 0

         for (char in str) {

            if (char.isLetter()) {
                count++
            }

         }

         return count

     }

}
```

这里先定义了一个StringUtil单例类，然后在这个单例类中定义了一个lettersCount()函数，该函数接收一个字符串参数。在lettersCount()方法中，我们使用for-in循环去遍历字符串中的每一个字符。如果该字符是一个字母的话，那么就将计数器加1，最终返回计数器的值。

现在，当我们需要统计某个字符串中的字母数量时，只需要编写如下代码即可：

```kotlin
val str = "ABC123xyz!@#"

val count = StringUtil.lettersCount(str)
```

这种写法绝对可以正常工作，并且这也是Java编程中最标准的实现思维。但是有了扩展函数之后就不一样了，我们可以使用一种更加面向对象的思维来实现这个功能，比如说将lettersCount()函数添加到String类当中。

下面我们先来学习一下定义扩展函数的语法结构，**其实非常简单，定义扩展函数只需要在函数名的前面加上一个`ClassName.`的语法结构，就表示将该函数添加到指定类`ClassName`当中了。**



由于我们希望向String类中添加一个扩展函数，因此需要先创建一个String.kt文件。<u>文件名虽然并没有固定的要求，但是我建议向哪个类中添加扩展函数，就定义一个同名的Kotlin文件，这样便于你以后查找。</u>

现在在String.kt文件中编写如下代码：

```kotlin
fun String.lettersCount(): Int {

     var count = 0

     for (char in this) {
         if (char.isLetter()) {
         	count++

         }

     }

     return count

}
```

注意这里的代码变化，<u>现在我们将lettersCount()方法定义成了String类的扩展函数，那么函数中就自动拥有了String实例的上下文。</u>因此`lettersCount()`函数就不再需要接收一个字符串参数了，而是直接遍历this即可，因为现在this就代表着字符串本身。定义好了扩展函数之后，统计某个字符串中的字母数量只需要这样写即可：

```kotlin
val count = "ABC123xyz!@#".lettersCount()
```

看上去就好像是String类中自带了lettersCount()方法一样。

### 运算符重载 `operator`

> 这里kotlin很像c++的运算符重载

运算符重载使用的是`operator关键字`，只要在指定函数的前面加上operator关键字，就可以实现运算符重载的功能了。

**指定函数**是运算符重载里面比较复杂的一个问题，因为不同的运算符对应的重载函数也是不同的。比如说加号运算符对应的是`plus()`函数，减号运算符对应的是`minus()`函数。

是以加号运算符为例，如果想要实现让两个对象相加的功能，那么它的语法结构如

下：

```kotlin
class Obj {

 operator fun plus(obj: Obj): Obj {

 // 处理相加的逻辑

 }

}
```

在上述语法结构中，关键字operator和函数名plus都是固定不变的，而接收的参数和函数返回值可以根据你的逻辑自行设定。那么上述代码就表示一个Obj对象可以与另一个Obj对象相加，最终返回一个新的Obj对象。对应的调用方式如下：

```kotlin
val obj1 = Obj()

val obj2 = Obj()

val obj3 = obj1 + obj2
```

这种`obj1 + obj2`的语法看上去好像很神奇，但其实这就是Kotlin给我们提供的一种语法糖，它会在编译的时候被转换成`obj1.plus(obj2)`的调用方式。

**让两个对象相加：**

让两个Money对象相加。首先定义Money类的结构，这里我准备让Money的主构造函数接收一个value参数，用于表示钱的金额。创建`Money.kt`文件，代码如下所示：

```kotlin
class Money(val value: Int)
```

定义好了Money类的结构，接下来我们就使用运算符重载来实现让两个Money对象相加的功

能：

```kotlin
class Money(val value: Int) {

 operator fun plus(money: Money): Money {

 val sum = value + money.value

 return Money(sum)

 }

}
```

可以看到，这里使用了operator关键字来修饰plus()函数，这是必不可少的。在plus()函数中，我们将当前Money对象的value和参数传入的Money对象的value相加，然后将得到的和传给一个新的Money对象并将该对象返回。这样两个Money对象就可以相加了，就是这么简单。

现在我们可以使用如下代码来对刚刚编写的功能进行测试：

```kotlin
val money1 = Money(5)
val money2 = Money(10)
val money3 = money1 + money2
println(money3.value)
```

最终打印的结果一定是15。

**实现对象与数字相加**

Money对象只允许和另一个Money对象相加，有没有觉得这样不够方便呢？或许你会觉得，如果Money对象能够直接和数字相加的话，就更好了。这个功能当然也是可以实现的，因为**Kotlin允许我们对同一个运算符进行多重重载**，代码如下所示：

```kotlin
class Money(val value: Int) {

     operator fun plus(money: Money): Money {

     val sum = value + money.value

     return Money(sum)

 	 }

     operator fun plus(newValue: Int): Money {

         val sum = value + newValue

         return Money(sum)

     }

}
```

**调用：**

```kotlin
val money1 = Money(5)
val money2 = Money(10)
val money3 = money1 + money2
val money4 = money3 + 20
println(money4.value)
```

这里让money3对象再加上20的金额，最终打印的结果就变成了35。

#### 实际调用函数对照表

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202308060034524.png" alt="image-20230806003431188" style="zoom:67%;" />

### 高阶函数详解

> 高阶函数的定义：如果一个函数接收另一个函数作为**参数**，或者**返回值的类型**是另一个函数，那么该函数就称为高阶函数。

函数类型的语法规则是有点特殊的，基本规则如下：

```kotlin
(String, Int) -> Unit
```

> **`->左边`**的部分就是用来声明该函数接收什么参数的，多个参数之间使用逗号隔开，如果不接收任何参数，写一对空括号就可以了。而**`->右边`**的部分用于声明该函数的返回值是什么类型，如果没有返回值就使用Unit，它大致相当于Java中的void。



在将上述函数类型添加到某个函数的参数声明或者返回值声明上，那么这个函数就是一个高阶函数了，如下所示：

```kotlin
fun example(func: (String, Int) -> Unit) {

 func("hello", 123)

}
```

这里的example()函数接收了一个函数类型的参数，因此example()函数就是一个高阶函数。而调用一个函数类型的参数，它的语法类似于调用一个普通的函数，只需要在参数名的后面加上一对括号，并在括号中传入必要的参数即可。

**由于高阶函数的用途：**这里如果要让我简单概括一下的话，那就是高阶函数允许让函数类型的参数来决定函数的执行逻辑。即使是同一个高阶函数，只要传入不同的函数类型参数，那么它的执行逻辑和最终的返回结果就可能是完全不同的。

**例如：**定义一个叫作`num1AndNum2()`的高阶函数，并让它接收两个整型和一个函数类型的参数。我们会在`num1AndNum2()`函数中对传入的两个整型参数进行某种运算，并返回最终的运算结果，但是具体进行什么运算是由传入的函数类型参数决定的。

```kotlin
fun num1AndNum2(num1: Int, num2: Int, operation: (Int, Int) -> Int): Int {
 val result = operation(num1, num2)
 return result
}
```

此Kotlin还支持其他多种方式来调用高阶函数，比如Lambda表达式、匿名函数、成员引用等。其中，**Lambda表达式是最常见也是最普遍的高阶函数调用方式**，也是我们接下来要重点学习的内容。

先给出一段代码：

```kotlin
fun plus(num1: Int, num2: Int): Int {
 	return num1 + num2
}
fun minus(num1: Int, num2: Int): Int {
 	return num1 - num2
}
fun main() {
     val num1 = 100
     val num2 = 80
     val result1 = num1AndNum2(num1, num2, ::plus)
     val result2 = num1AndNum2(num1, num2, ::minus)
     println("result1 is $result1")
     println("result2 is $result2")
}

```

> 注意这里调用num1AndNum2()函数的方式，第三个参数使用了::plus和::minus这种写法。这是一种函数引用方式的写法，表示将plus()和minus()函数作为参数传递给num1AndNum2()函数。

上面的代码通过Lambda表达式可以转化为：

```kotlin
fun main() {
     val num1 = 100
     val num2 = 80
     val result1 = num1AndNum2(num1, num2) { n1, n2 ->
     	n1 + n2
     }
     val result2 = num1AndNum2(num1, num2) { n1, n2 ->
     	n1 - n2
     }
     println("result1 is $result1")
     println("result2 is $result2")
}
```

Lambda表达式同样可以完整地表达一个函数的参数声明和返回值声明（**Lambda表达式中的最后一行代码会自动作为返回值**），但是写法却更加精简。

------------------

回顾之前在第3章学习的`apply`函数，它可以用于给Lambda表达式提供一个指定的上下文，当需要连续调用同一个对象的多个方法时，apply函数可以让代码变得更加精简，比如StringBuilder就是一个典型的例子。接下来我们就使用高阶函数模仿实现一个类似的功能。

```kotlin
fun StringBuilder.build(block: StringBuilder.() -> Unit): StringBuilder {
     block()
     return this
}
```

> 具体分解:
>
> - `StringBuilder.()`:
>   表示lambda可以直接访问定义它的StringBuilder对象的成员,如append()等方法。
> - `()`:
>   lambda形参列表为空,不需要传入参数。
> - `-> Unit`:
>   lambda函数体执行结束后没有返回值,返回类型为Unit。

这里我们给StringBuilder类定义了一个build扩展函数，这个扩展函数接收一个**函数类型参数**，并且**返回值类型**也是StringBuilder。

注意，这个函数类型参数的声明方式和我们前面学习的语法有所不同：它在函数类型的前面加上了一个`StringBuilder. `的语法结构。这是什么意思呢？**其实这才是定义高阶函数完整的语法规则，在函数类型的前面加上`ClassName.` 就表示这个函数类型是定义在哪个类当中的。**

那么这里将函数类型定义到StringBuilder类当中有什么好处呢？好处就是当我们调用build函数时传入的Lambda表达式将会自动拥有StringBuilder的上下文，同时这也是apply函数的实现方式。

现在我们就可以使用自己创建的build函数来简化StringBuilder构建字符串的方式了。这里仍然用吃水果这个功能来举例：

```kotlin
fun main() {

     val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape")

     val result = StringBuilder().build {

         append("Start eating fruits.\n")

         for (fruit in list) {

            append(fruit).append("\n")

         }

         append("Ate all fruits.")

     }

    println(result.toString())

}
```

可以看到，build函数的用法和apply函数基本上是一模一样的，只不过我们编写的build函数目前只能作用在StringBuilder类上面，而apply函数是可以作用在所有类上面的。如果想实现apply函数的这个功能，需要借助于Kotlin的泛型才行。

### 内联函数的作用(TODO)





### 八、泛型和委托

#### 泛型的基本用法

> 在一般的编程模式下，我们需要给任何一个变量指定一个具体的类型，而泛型允许我们在不指定具体类型的情况下进行编程，这样编写出来的代码将会拥有更好的扩展性。

**泛型主要有两种定义方式：一种是定义泛型类，另一种是定义泛型方法，使用的语法结构都是<T>。**当然括号内的T并不是固定要求的，事实上你使用任何英文字母或单词都可以，但是通常情况下，T是一种约定俗成的泛型写法。

如果我们要**定义一个泛型类**，就可以这么写：

```kotlin
class MyClass<T> {

 fun method(param: T): T {

 return param

 }
}
```

此时的MyClass就是一个泛型类，MyClass中的方法允许使用T类型的参数和返回值。**如果我们不想定义一个泛型类，只是想定义一个泛型方法**，应该要怎么写呢？也很简单，只需要将定义泛型的语法结构写在方法上面就可以了，如下所示：

```kotlin
class MyClass {

 fun <T> method(param: T): T {

 return param

 }

}
```

此时的调用方式也需要进行相应的调整：

```kotlin
val myClass = MyClass()

val result = myClass.method<Int>(123)
```

由于Kotlin还拥有非常出色的类型推导机制，例如我们传入了一个Int类型的参数，它能够自动推导出泛型的类型就是Int型，因此这里也**可以直接省略泛型的指定：**

```kotlin
val myClass = MyClass()
val result = myClass.method(123)
```

**Kotlin还允许我们对泛型的类型进行限制。**目前你可以将method()方法的泛型指定成任意类

型，但是如果这并不是你想要的话，还可以通过指定上界的方式来对泛型的类型进行约束，比

如这里将method()方法的泛型上界设置为Number类型，如下所示：

```kotlin
class MyClass {

 fun <T : Number> method(param: T): T {
     
 return param
     
 }
}
```

这种写法就表明，我们只能将method()方法的泛型指定成数字类型，比如Int、Float、Double等。但是如果你指定成字符串类型，就肯定会报错，因为它不是一个数字。另外，**在默认情况下，所有的泛型都是可以指定成可空类型的，这是因为在不手动指定上界的时候，泛型的上界默认是Any?。而如果想要让泛型的类型不可为空，只需要将泛型的上界手动指定成Any就可以了。**

#### 类委托和委托属性



### 十、泛型的高级属性

#### 对泛型进行实化

> 意思就是在编译时期让编译器识别出来泛型具体的种类(Int,String)，而不是不知道泛型的类型，比如List<T>在运行的时候只知道这是个List而不知道T的类型。
>
> Kotlin提供了内联函数，就是调用一个内联函数时，先把参数传到原函数中，然后再将原函数的逻辑传到调用内联函数的地方

##### 工作原理：



![image-20230816225320863](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202308162253099.png)











### 十一、使用协程编写高效的并发程序

#### 协程的基本用法

##### 引入依赖：

```kotlin
 implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:1.1.1"
 implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:1.1.1"
```

#### 协程是什么

> - **协程通过将复杂性放入库来简化异步编程。程序的逻辑可以在协程中顺序地表达，而底层库会为我们解决其异步性。该库可以将用户代码的相关部分包装为回调、订阅相关事件、在不同线程（甚至不同机器！）上调度执行，而代码则保持如同顺序执行一样简单。**
> - **协程是一种并发设计模式，您可以在Android平台上使用它来简化异步执行的代码**

简单的概括就是我们可以，以同步的方式去编写异步执行的代码。协程是依赖于线程，但是协程挂起时不需要阻塞线程，几乎是无代价的。所以协程像是一种用户态的线程，非常轻量级，一个线程中可以创建N个协程。协程的创建是过`CoroutineScope`创建,协程的启动方式有三种：

1. `runBlocking：T` 启动一个新的协程并阻塞调用它的线程，直到里面的代码执行完毕,返回值是泛型`T`，就是你协程体中最后一行是什么类型，最终返回的是什么类型`T`就是什么类型。
2. `launch：Job` 启动一个协程但不会阻塞调用线程,必须要在协程作用域(`CoroutineScope`)中才能调用,返回值是一个Job。
3. `async:Deferred<T>` 启动一个协程但不会阻塞调用线程,必须要在协程作用域(`CoroutineScope`)中才能调用。以`Deferred`对象的形式返回协程任务。返回值泛型`T`同`runBlocking`类似都是协程体最后一行的类型。











#### Flow使用







### 十三、高级程序开发组件

#### 13.4 LiveData

##### 13.4.1 基本用法

> LiveData可以包含任何类型的数据，并在数据发生变化的时候通知给观察者。也就是说，如果我们将计数器的计数使用LiveData来包装，然后在Activity中去观察它，就可以主动将数据变化通知给Activity了。

①修改`MainViewModel`中的代码，添加`MutableLiveData`：

```kotlin
class MainViewModel(countReserved: Int) : ViewModel() {
    
     val counter = MutableLiveData<Int>()
    
     init {
         counter.value = countReserved
     }
    
     fun plusOne() {
         //注意调用LiveData的getValue()方法所获得的数据是可能为空的，因此这里使用了一个?:操作符
         val count = counter.value ?: 0
         counter.value = count + 1
     }
    
     fun clear() {
         counter.value = 0
     }
}
```

> 这里我们将counter变量修改成了一个`MutableLiveData`对象，并指定它的泛型为Int，表示它包含的是整型数据。
>
> **MutableLiveData是一种可变的LiveData，它的用法很简单，主要有3种读写数据的方法，分别是getValue()、setValue()和postValue()方法。**
>
> getValue()方法用于获取LiveData中包含的数据；setValue()方法用于给LiveData设置数据，但是只能在主线程中调用；postValue()方法用于在非主线程中给LiveData设置数据。而上述代码其实就是调用getValue()和setValue()方法对应的语法糖写法。

②在`MainActivity`中添加`MutableLiveData`数据的监听：

```kotlin
class MainActivity : AppCompatActivity() {
     ...
     override fun onCreate(savedInstanceState: Bundle?) {
         ...
         plusOneBtn.setOnClickListener {
             viewModel.plusOne()
         }
         
         clearBtn.setOnClickListener {
             viewModel.clear()
         }
         
         viewModel.counter.observe(this, Observer { count ->
             infoText.text = count.toString()
         })
     }
     override fun onPause() {
         super.onPause()
         sp.edit {
         	putInt("count_reserved", viewModel.counter.value ?: 0)
         }

     }
}
```

> 调用`viewModel.counter的observe()`方法来观察数据的变化。经过对MainViewModel的改造，现在counter变量已经变成了一个LiveData对象，任
>
> 何LiveData对象都可以调用它的observe()方法来观察数据的变化。
>
> `observe()`方法接收两个参数：
>
> + **第一个参数**是一个LifecycleOwner对象，而Activity本身就是一个LifecycleOwner对象，因此直接传this就好；
>
> + **第二个参数**是一个Observer接口，当counter中包含的数据发生变化时，就会回调到这里，因此我们在这里将最新的计数更新到界面上即可。
>
> 
>
> <span style="background-color:Darkorange ;"><b>注意：</b></span>如果你需要在子线程中给LiveData设置数据，一定要调用postValue()方法，而不能再使用setValue()方法，否则会发生崩溃。

> 对observe()方法的语法扩展。我们只需要在app/build.gradle文件中添加如下依赖：
>
> ```kotlin
> dependencies {
>  ...
> 
>  implementation "androidx.lifecycle:lifecycle-livedata-ktx:2.2.0"
> 
> }
> ```
>
> 上面的observe函数的写法就可以变成·：
>
> ```kotlin
> viewModel.counter.observe(this) { count ->
>  	infoText.text = count.toString()
> }
> ```
>
> 虽说现在的写法可以正常工作，但其实这仍然不是最规范的LiveData用法，主要的问题就在于我们将counter这个可变的LiveData暴露给了外部。这样
>
> 即使是在ViewModel的外面也是可以给counter设置数据的，从而破坏了ViewModel数据的封装性，同时也可能带来一定的风险。
>
> 改造`MainViewModel`来实现这样的功能：
>
> ```kotlin
> class MainViewModel(countReserved: Int) : ViewModel() {
> 
>      val counter: LiveData<Int>
>      get() = _counter
> 	 //调用viewModel.getCounter()方法返回的是_count
>     
>      private val _counter = MutableLiveData<Int>()
> 
>      init {
>      	_counter.value = countReserved
>      }
>     
>      fun plusOne() {
>          val num = _counter.value ?: 0
>          _counter.value = num + 1
>      }
>     
>      fun clear() {
>          _counter.value = 0
>      }
> }
> ```
>
> 先将原来的counter变量改名为\_counter变量，并给它加上private修饰符，这样\_counter变量对于外部就是不可见的了。然后我们又新定义了一个counter变量，将它的类型声明为不可变的LiveData，并在它的get()属性方法中返回\_counter变量。这样，当外部调用counter变量时，实际上获得的就是_counter的实例，但是无法给counter设置数据，从而保证了ViewModel的数据封装性
>
> 

##### 13.4.2 **map**和**switchMap**

> 当项目变得复杂之后，可能会出现一些更加特殊的需求。LiveData为了能够应对各种不同的需求场景，提供了两种转换方法：map()和switchMap()方法。

###### `map()`方法

假如说有一个`User`类：

```kotlin
data class User(var firstName: String, var lastName: String, var age: Int)
```

接着在MainViewModel创建一个相应的LiveData来包含User类型的数据：

```kotlin
class MainViewModel(countReserved: Int) : ViewModel() {
     val userLiveData = MutableLiveData<User>()
     ...
}
```

但如果在Activity中我们只要求显式用户的姓名，其他的并不需要，再将整个User类型的LiveData暴露到外部就不合适了。

因此接下来就要运用到`map()`方法了：

```kotlin
class MainViewModel(countReserved: Int) : ViewModel() {
    
     private val userLiveData = MutableLiveData<User>()
    
      val userName: LiveData<String> = Transformations.map(userLiveData) { user ->
         "${user.firstName} ${user.lastName}"//姓和名
     }
    
     ...
}   
```

> 这里我们调用了Transformations的map()方法来对LiveData的数据类型进行转换。
>
> map()方法接收两个参数：
>
> + 第一个参数是原始的LiveData对象；
> + 第二个参数是一个转换函数，我们在转换函数里编写具体的转换逻辑即可。这里的逻辑也很简单，就是将User对象转换成一个只包含用户姓名的字符串。

###### `switchMap()`

> 虽然它的使用场景非常固定，但是可能比map()方法要更加常用。
>
> 前面我们所学的所有内容都有一个**前提**：LiveData对象的实例都是在ViewModel中创建的。然而在实际的项目中，不可能一直是这种理想情况，很有可能**ViewModel中的某个LiveData对象是调用另外的方法获取的。**

①新建一个Repository单例类

```kotlin
object Repository {
    
     fun getUser(userId: String): LiveData<User> {
         
         val liveData = MutableLiveData<User>()
         
         liveData.value = User(userId, userId, 0)
         
         return liveData
     }
    
}
```

> 我们在Repository类中添加了一个getUser()方法，这个方法接收一个userId参数。按照正常的编程逻辑，我们应该根据传入的userId参数去服务器请求或者到数据库中查找相应的User对象，但是这里只是模拟示例，因此每次将传入的userId当作用户姓名来创建一个新的User对象即可。
>
> getUser()方法返回的是一个包含User数据的LiveData对象，而且**每次调用getUser()方法都会返回一个新的LiveData实例。**

②我们在MainViewModel中也定义一个getUser()方法，并且让它调用Repository的getUser()方法来获取LiveData对象：

```kotlin
class MainViewModel(countReserved: Int) : ViewModel() {
     ...
     fun getUser(userId: String): LiveData<User> {
     	return Repository.getUser(userId)
      }
}
```

接下来的问题就是，在Activity中如何观察LiveData的数据变化呢？这个时候，switchMap()方法就可以派上用场了。

它的使用场景非常固定：<span style="background-color:#a2e043;"><b>如果ViewModel中的某个LiveData对象是调用另外的方法获取的，那么我们就可以借助switchMap()方法，将这个LiveData对象转换成另外一个可观察的LiveData对象。</b></span>

③

```kotlin
class MainViewModel(countReserved: Int) : ViewModel() {
     ...
     private val userIdLiveData = MutableLiveData<String>()
    
     val user: LiveData<User> = Transformations.switchMap(userIdLiveData) { userId ->
     	Repository.getUser(userId)
     }
    
     fun getUser(userId: String) {
         userIdLiveData.value = userId
     }
}
```

> switchMap()方法同样接收两个参数：
>
> + **第一个参数传入我们新增的userIdLiveData，switchMap()方法会对它进行观察**；
> + 第二个参数是一个转换函数，注意，我们必须在这个转换函数中返回一个LiveData对象，因为switchMap()方法的工作原理就是要将转换函数中返回的LiveData对象转换成另一个可观察的LiveData对象。

**工作流程：**

首先，当外部调用MainViewModel的getUser()方法来获取用户数据时，并不会发起任何请求或者函数调用，只会将传入的userId值设置到userIdLiveData当中。而一旦userIdLiveData的数据发生变化，那么观察userIdLiveData的switchMap()方法就会执行，并且调用我们编写的转换函数。然后在转换函数中调用Repository.getUser()方法获取真正的用户数据。同时，switchMap()方法会将Repository.getUser()方法返回的LiveData对象转换成一个可观察的LiveData对象，对于Activity而言，只要去观察这个LiveData对象`user`就可以了。

**Activity中的代码：**

```kotlin
     ...
viewModel.getUser(userId)
     ...

viewModel.user.observe(this, Observer { user ->
     infoText.text = user.firstName
 })
```

当ViewModel中某个获取数据的方法有可能是没有参数的时应该怎么写呢？

其实跟上面的差不多：

```kotlin
class MyViewModel : ViewModel() {
    
     private val refreshLiveData = MutableLiveData<Any?>()
    
     val refreshResult = Transformations.switchMap(refreshLiveData) {//没有声明refreshResult的具体类型，而是依靠kotlind
         Repository.refresh() // 假设Repository中已经定义了refresh()方法
     }
    
     fun refresh() {
         refreshLiveData.value = refreshLiveData.value
     }
}
```

> 这里我们定义了一个不带参数的`refresh()`方法，又对应地定义了一个refreshLiveData，但是它不需要指定具体包含的数据类型，因此这里我们将LiveData的泛型指定成`Any?`即可。
>
> 接下来就是点睛之笔的地方了，在refresh()方法中，我们只是将refreshLiveData原有的数据取出来（默认是空），再重新设置到refreshLiveData当中，这样就能触发一次数据变化。**<font color=orange>是的，LiveData内部不会判断即将设置的数据和原有数据是否相同，只要调用了setValue()或postValue()方法，就一定会触发数据变化事件。</font>**然后我们在Activity中观察refreshResult这个LiveData对象即可，这样只要调用了refresh()方法，观察者的回调函数中就能够得到最新的数据。



### 初始化lazy和lateinit变量

#### lateinit

##### 前言

> `lateinit`顾名思义就是<u>**延迟初始化**</u>，当与类属性一起使用时，`lateinit` 修改器使该属性在其类的对象构造时不被初始化。
>
> <u>只有在程序的**后期初始化**时，才会为`lateinit` 变量分配内存，而不是在它们被声明时。</u>这在初始化的灵活性方面是非常方便的。

##### 主要特征

+ `lateinit` 属性在整个程序中可能会改变不止一次，而且应该是可变的。因此应该使用`var`而不是`val`和`const`来修饰·。

+ 其次，由于`lateinit`属性不支持可空类型，因此可以避免很多空值检查。

+ `lateinit` 可以很好地用于非原始数据类型，但它不适用于 long 或 int 等基本类型，因为`lateinit `只能用于非空类型，而基本类型（如 long 或 int）是不可为空的，因此不能使用 lateinit 修饰基本类型属性。

+ 在 Kotlin 中，当你使用` lateinit `声明一个非空类型的属性时，编译器会在生成的代码中将该属性标记为可为空，并将其初始化为 null。以便在访问该属性时可以进行空值检查。

Kotlin允许你检查一个`lateinit` 属性是否被初始化。

```kotlin
lateinit var myLateInitVar: String
...

if(::myLateInitVar.isInitialized) {
   // Do something
}

```

##### 何时使用

在常规的变量初始化中，你必须添加一个假值，而且很可能是一个空值。这将在每次访问时增加大量的空值检查，此时我们就可以使用`lateinit`，

##### 时刻记住

在访问`lateinit` 属性之前总是对其进行初始化，否则，可能会出现报错。

最后，当给定的属性的数据类型是原始的或者出现空值的可能性很大时，要避免使用`lateinit` 。它不是为这些情况设计的，并且不支持原始或可忽略的类型。

#### lazy

##### 前言

> 顾名思义就是**以一种懒惰的方式初始化一个属性。本质上，它创建了一个引用，但只有在第一次使用或调用该属性时才进行初始化。**
>
> 在构建类对象的时候，其所有的公共和私有属性都在其构造函数中被初始化。在一个类中初始化变量会有一些开销；变量越多，开销就越大。而如果只在调用某一个变量时再进行初始化就能节省内存开销。

##### 主要特征

由于用懒惰委托初始化的属性应该自始至终使用相同的值，它具有不可改变的性质，**一般用于只读属性。你必须用一个`val` 声明来标记它。**

**它是线程安全的，即只计算一次，并默认由所有线程共享。**一旦被初始化，它就会在整个程序中记住或缓存初始化值。

与`lateinit` 相比，懒惰委托支持一个 [自定义的setter和getter](https://link.juejin.cn?target=https%3A%2F%2Fkotlinlang.org%2Fapi%2Flatest%2Fjvm%2Fstdlib%2Fkotlin%2Flazy.html)，允许它在读写值时进行中间操作。

##### 时刻记住

懒惰初始化是一种委托，它只初始化一次东西，而且只在它被调用的时候。它是为了避免不必要的对象创建。

委托对象缓存了第一次访问时返回的值。这个缓存的值在程序中需要时被进一步使用。

你可以利用它的自定义getter和setter在读写值时进行中间操作。我也更喜欢将其用于不可变类型，因为我觉得它在整个程序中保持不变的情况下效果最好。

### 抽象类和`interface`的区别

#### **抽象类 (Abstract Class)**

1. 抽象类可以包含具体实现的方法和抽象方法。也就是说，抽象类可以同时包含部分完整实现和部分未实现的代码。
2. 抽象类可以有非抽象的子类。这意味着子类可以选择重写父类中的所有抽象方法，也可以选择只重写一部分。
3. 抽象类不能被实例化，只能被继承。

例如：

```kotlin
abstract class Animal {
    abstract fun makeSound()
    fun eat() {
        println("The animal is eating")
    }
}

```

#### **接口 (Interface)**

1. 接口在 Kotlin 中并不像在一些其他语言（如 Java）中那样常用。在 Java 中，接口通常用于定义一个类的行为，而不包括任何实现。在 Kotlin 中，我们通常更倾向于使用抽象类来定义部分实现的类。
2. 接口在 Kotlin 中更像是一种类型声明，它只包含抽象方法的声明，没有具体的实现。
3. 接口可以被多个类实现，实现接口的类需要实现接口中声明的所有方法。
4. 接口不能包含字段或具体的方法实现。

例如：

```kotlin
interface Flyable {
    fun fly()
}

```

**总结**

总的来说，`abstract` 和 `interface` 的主要区别在于：

- 抽象类可以包含具体的实现，而接口只包含方法的声明。
- 抽象类不能被直接实例化，而接口不能包含具体的实现。
- 抽象类更适合用来定义部分实现的类，而接口更适合用来定义行为的契约。
