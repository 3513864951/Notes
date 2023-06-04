## MVVM

### 前言

> MVVM框架是一种在Android应用中使用的软件架构模式。MVVM代表着Model-View-ViewModel，其中Model代表数据层或数据源，View代表用户界面，而ViewModel则充当数据和用户界面界面之间的桥梁。ViewModel通过管理Model中的数据来保持View的状态同步。这种框架的好处之一是它可以帮助开发人员更轻松地测试应用程序的各个组件。🤓📱🧑‍💻

**gradle配置**

+ `build.gradle（:app）`

  添加如下代码：

```java
buildFeatures {
    dataBinding true
}
```

+ `build.gradle(MVVM)`

```java
// Top-level build file where you can add configuration options common to all sub-projects/modules.
buildscript {repositories {maven{ url 'https://maven.aliyun.com/repository/google'}
    maven{ url 'https://maven.aliyun.com/repository/gradle-plugin'}
    maven{ url 'https://maven.aliyun.com/repository/public'}
    maven{ url 'https://maven.aliyun.com/repository/jcenter'}
    google()
    jcenter()}
    dependencies {
        classpath 'com.android.tools.build:gradle:7.1.2'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files}
    }}
plugins {
    id 'com.android.application' version '7.3.1' apply false
    id 'com.android.library' version '7.3.1' apply false
}
```

+ `settings.gradle`

  ```java
  pluginManagement {
      repositories {
          gradlePluginPortal()
          google()
          mavenCentral()
      }
  }
  dependencyResolutionManagement {
      repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
      repositories {
          google()
          mavenCentral()
          maven { url 'https://jitpack.io' }
  
      }
  }
  rootProject.name = "MVVM"
  include ':app'
  ```

### 一、ViewModel使用

#### 前言

> ViewModel是一个Android架构组件，用于管理UI相关的数据。ViewModel的作用是将UI的数据和逻辑与Activity或Fragment分离开来，从而使得Activity或Fragment变得更加轻量级，更加容易进行测试和维护。ViewModel可以存储和管理与UI相关的数据，并且可以在Activity或Fragment被销毁后，将这些数据保持不变。ViewModel的生命周期与Activity或Fragment不同，它的生命周期是与Activity或Fragment分离开来的，因此即使Activity或Fragment被销毁并重新创建，ViewModel中的数据仍然会保持不变。使用ViewModel可以帮助开发者更好地组织和管理UI相关的数据，并且可以提高应用程序的可维护性和可测试性。

#### ①启用DataBinding

找到app模块下的build.gradle，在`android{}`闭包下添加如下代码：

```java
buildFeatures {
    dataBinding true
}
```
#### ②绑定Activity

新建一个`MainViewModel`类，用来跟MainActivity绑定：

```java
public class MainViewModel extends ViewModel {
    
    public String account;
    public String pwd;

}
```

接下来绑定将MainViewModel和MainActivity

```JAVA
public class MainActivity extends AppCompatActivity {
    MainViewModel mainViewModel;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mainViewModel=new ViewModelProvider(this).get(MainViewModel.class);
        }
}
```

![image-20230603205546666](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306032055853.png)

#### ③界面编写

接下来已登录界面为例进行编写，修改`MainViewMode`中的代码如下：l

```java
	public String account;
    public String pwd;
```

接下来修改`activity_main.xml`文件中的代码：

```CSS
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="32dp"
    tools:context=".MainActivity">

    <com.google.android.material.textfield.TextInputLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <com.google.android.material.textfield.TextInputEditText
            android:id="@+id/et_account"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="@color/white"
            android:hint="账号" />
    </com.google.android.material.textfield.TextInputLayout>

    <com.google.android.material.textfield.TextInputLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="12dp">

        <com.google.android.material.textfield.TextInputEditText
            android:id="@+id/et_pwd"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="@color/white"
            android:hint="密码"
            android:inputType="textPassword" />
    </com.google.android.material.textfield.TextInputLayout>

    <com.google.android.material.button.MaterialButton
        android:id="@+id/btn_login"
        android:layout_width="match_parent"
        android:layout_height="48dp"
        android:layout_margin="24dp"
        android:insetTop="0dp"
        android:insetBottom="0dp"
        android:text="登  录"
        app:cornerRadius="12dp" />

</LinearLayout>
```

#### ④实现登录功能

在MainActivity中添加如下代码：

```java
        etAccount=findViewById(R.id.et_account);
        etPwd=findViewById(R.id.et_pwd);
        findViewById(R.id.btn_login).setOnClickListener(v -> {
            mainViewModel.account=etAccount.getText().toString().trim();
            mainViewModel.pwd=etPwd.getText().toString().trim();
            if(mainViewModel.account.isEmpty()) {
                Toast.makeText(this, "请输入账号", Toast.LENGTH_SHORT).show();
                return;
            }
                if(mainViewModel.pwd.isEmpty()) {
                    Toast.makeText(this, "请输入密码", Toast.LENGTH_SHORT).show();
                    return;
                }
                    Toast.makeText(this,"登录成功",Toast.LENGTH_SHORT).show();
        });
```

<span style="background-color:#a2e043;"><b>运行结果：</b></span>

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306032105874.gif" alt="untitled" style="zoom: 25%;" />

直接看好像看不出什么，但是手机在切换横竖屏的时候，Activity是会被重新创建的，那么切换后输入框中应该没有数值，但是通过ViewModel去保存输入框中的数据就不一样了，即便Activity重新创建也不会清空数据，MainViewModel仍然稳定，因此横竖屏切换时不会造成数据丢失。

**这主要得益于ViewModel的生命周期：**

`ViewModel`的生命周期与Activity不同，它的生命周期是与Activity的生命周期分离的。**当Activity被销毁时，ViewModel并不会被销毁，而是会一直存在于内存中，直到整个应用程序进程被销毁。**<u>具体来说，ViewModel的生命周期可以分为以下三个阶段：</u>

1. **创建阶段**：当Activity首次创建时，会创建对应的ViewModel实例，并且ViewModel实例会一直存在于内存中，直到整个应用程序进程被销毁。
2. **活动阶段**：当Activity处于活动状态时，可以通过ViewModel获取之前保存的数据，并且可以通过ViewModel保存新的数据。
3. **清理阶段**：当Activity被销毁时，ViewModel并不会被销毁，而是会继续存在于内存中。当整个应用程序进程被销毁时，ViewModel才会被销毁。在ViewModel被销毁之前，可以通过ViewModel保存的数据进行数据持久化等操作。

### 二、LiveData使用

#### 前言

> LiveData是用来解决Android中数据更新时，UI无法及时响应的问题。LiveData是一个可观察的数据持有类，它可以感知Activity或Fragment的生命周期，并且只在这些组件处于激活状态时才会通知观察者。LiveData可以确保UI界面中的数据和应用程序中的数据保持一致，并且可以避免Activity或Fragment在后台时，由于数据更新而导致的崩溃问题。LiveData还可以避免由于Activity或Fragment的生命周期变化而导致的内存泄漏问题，并且可以保证数据的一致性和可靠性。使用LiveData可以让开发者更加方便地编写响应式的UI界面，并且可以帮助开发者更好地管理Activity和Fragment的生命周期。
>
> <u>简单来说，LiveData可以感知数据的变化，在数据变化后自动执行一些操作，比如更新UI显示的数据。</u>

#### ①修改MainViewModel中的变量：

```java
	public MutableLiveData<String> account = new MutableLiveData<>();
    public MutableLiveData<String> pwd = new MutableLiveData<>();
```

注意这里用的是`MutableLiveData`,与LiveData不同的是MutableLiveData可以通过`setValue()`和`postValue()`方法来更新数据，而在LiveData中，数据是不可变的，只能通过观察者来获取数据。MutableLiveData通常用于ViewModel中，它可以在ViewModel中保存数据并通知UI层进行更新，而且更加灵活、方便。

接着在`activity_main.xml`中添加显示数据的TextView：

```css
	<TextView
        android:id="@+id/tv_account"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

    <TextView
        android:id="@+id/tv_pwd"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>
```

#### ②修改MainActivity中的代码并添加数据观察

![image-20230603220601364](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306032206568.png)

```java
	mainViewModel.account.setValue(etAccount.getText().toString().trim());
	mainViewModel.pwd.setValue(etPwd.getText().toString().trim());
```

+ 使用MutableLiveData的`setValue()`方法更新数据

  > **<font color=Gray>还有一种方式是postValue()，不过setValue()只能在主线程中调用，postValue()可以在任何线程中调用。</font>**

```java
mainViewModel.account.observe(this,account->tvAccount.setText("账号"+account));
mainViewModel.pwd.observe(this,pwd->tvPwd.setText("密码"+pwd));
```

+ 当mainViewModel中的account数据发生变化时，会通知观察者（即这个代码所在的Activity或Fragment），然后将新的account值更新到tvAccount这个TextView上。

+ 注意这段代码不在按钮的点击事件里面，也就是说，点击按钮后TextView显示的数据变化不是因为点击事件发生的，而是因为点击事件使得mainViewModel中的数据发生了改变，MutableLiveData观察到了这一变化，因此改变了TextView中的数据

+ 这里的代码使用了lambda表达式进行了简化，实际代码如下：

  ![image-20230603221625582](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306032216644.png)

  <span style="background-color:#a2e043;"><b>运行结果：</b></span>

  <img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306032219245.gif" alt="untitled (1)" style="zoom:25%;" />

### 三、DataBing使用

`DataBing`已经在Android Studio中内置，在android{}闭包里面添加：

```java
    buildFeatures {
        dataBinding true
    }
```

DataBinding，顾名思义就是数据绑定，可以看到现在的三个组件都与数据有关系，ViewModel数据持有，LiveData数据观察、DataBinding数据绑定。

#### ①单向绑定

就相当于单向沟通，A给B说去拿快递，洗衣服，做饭，但B却不能命令A；双向绑定相当于A能影响B，B同样也能影响A。

下面创建一个用户User用来储存数据:

```java
public class User extends BaseObservable {

    private String account;
    private String pwd;

    public String getAccount() {
        return account;
    }

    public void setAccount(String account) {
        this.account = account;
        notifyChange();//通知改变 所有参数改变
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    public User(String account, String pwd) {
        this.account = account;
        this.pwd = pwd;
    }
}
```

这里我继承了BaseObservable，注意它在androidx.databinding包下。然后我们的数据是需要显示在页面上的，而之前是通过Activity获取xml中的控件，然后显示数据在控件上，而现在有了DataBinding，可以直接和xml的中数据进行绑定，这看起来和JS比较像。下面我们对activity_main.xml进行改变。改变后代码如下：

```css
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>
        <variable
            name="user"
            type="com.example.mvvmtest.User" />
    </data>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        android:orientation="vertical"
        android:padding="32dp"
        tools:context=".MainActivity">

        <com.google.android.material.textfield.TextInputLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content">

            <com.google.android.material.textfield.TextInputEditText
                android:id="@+id/et_account"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:background="@color/white"
                android:hint="账号" />
        </com.google.android.material.textfield.TextInputLayout>

        <com.google.android.material.textfield.TextInputLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="12dp">

            <com.google.android.material.textfield.TextInputEditText
                android:id="@+id/et_pwd"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:background="@color/white"
                android:hint="密码"

                />
        </com.google.android.material.textfield.TextInputLayout>
        <!--    android:inputType="textPassword"-->
        <com.google.android.material.button.MaterialButton
            android:id="@+id/btn_login"
            android:layout_width="match_parent"
            android:layout_height="48dp"
            android:layout_margin="24dp"
            android:insetTop="0dp"
            android:insetBottom="0dp"
            android:backgroundTint="@color/green"
            android:textColor="@color/black"
            android:text="登  录"
            app:cornerRadius="12dp" />

        <TextView
            android:id="@+id/tv_account"
            android:text="@{user.account}"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content" />

        <TextView
            android:id="@+id/tv_pwd"
            android:text="@{user.pwd}"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content" />

    </LinearLayout>
</layout>

```

这里我在最外层加了一个`layout`标签，然后将原来的布局放在layout里面，再增加一个数据源，也就是user对象，然后再底部的两个tv_account和tv_pwd两个TextView中的text属性中绑定了user对象中的属性值。当然这样还没有完成，最后一步是在MainActivity中去进行绑定的。

修改`MainActivity`中的代码：

```java
public class MainActivity extends AppCompatActivity {
    private User user;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //数据绑定视图
        ActivityMainBinding dataBing= DataBindingUtil.setContentView(this,R.layout.activity_main);
        user=new User("admin","123456");
        //设置数据，直接显示在视图上
        dataBing.setUser(user);
		//通过手动更改数据源的方法，将更改的数据通知到xml
        dataBing.btnLogin.setOnClickListener(v -> {
            user.setAccount(dataBing.etAccount.getText().toString().trim());
            user.setPwd(dataBing.etPwd.getText().toString().trim());
        });
    }
}
```

DataBindingUtil.setContentView返回的是一个ViewDataBinding对象，这个对象的生成取决于你的Activity，例如MainActivity，就会生成ActivityMainBinding。然后再通过生成的ActivityMainBinding去设置要显示在xml中控件的值。

<span style="background-color:#a2e043;"><b>运行结果：</b></span>

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306041350462.gif" alt="untitled (2)" style="zoom:25%;" />

#### ②双向绑定

修改一下MainViewModel，代码如下：

```java
public class MainViewModel extends ViewModel {

    public MutableLiveData<User> user;

    public MutableLiveData<User> getUser(){
        if(user == null){
            user = new MutableLiveData<>();
        }
        return user;
    }
}
```

接下来修改用户User

```java
public class User extends BaseObservable {

    public String account;
    public String pwd;

    @Bindable
    public String getAccount() {
        return account;
    }

    public void setAccount(String account) {
        this.account = account;
        notifyPropertyChanged(BR.account);
        //只通知改变的参数
    }

    @Bindable
    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
        notifyPropertyChanged(BR.pwd);
        //只通知改变的参数
    }

    public User(String account, String pwd) {
        this.account = account;
        this.pwd = pwd;
    }
}
```

修改activity_main.xml中的代码，将数据源改为MainViewModel中的User成员

```css
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    >

    <!--绑定数据-->
    <data>
        <variable
            name="viewModel"
            type="com.example.mvvm.MainViewModel" />
    </data>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        android:orientation="vertical"
        android:padding="32dp"
        tools:context=".MainActivity">

        <TextView
            android:id="@+id/tv_account"
            android:text="@{viewModel.user.account}"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"/>

        <TextView
            android:layout_marginBottom="24dp"
            android:id="@+id/tv_pwd"
            android:text="@{viewModel.user.pwd}"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"/>

        <com.google.android.material.textfield.TextInputLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content">

            <com.google.android.material.textfield.TextInputEditText
                android:id="@+id/et_account"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:background="@color/white"
                android:text="@={viewModel.user.account}"
                android:hint="账号" />
        </com.google.android.material.textfield.TextInputLayout>

        <com.google.android.material.textfield.TextInputLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="12dp">

            <com.google.android.material.textfield.TextInputEditText
                android:id="@+id/et_pwd"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:background="@color/white"
                android:text="@={viewModel.user.pwd}"
                android:hint="密码"
                android:inputType="textPassword" />
        </com.google.android.material.textfield.TextInputLayout>

        <com.google.android.material.button.MaterialButton
            android:id="@+id/btn_login"
            android:layout_width="match_parent"
            android:layout_height="48dp"
            android:layout_margin="24dp"
            android:insetTop="0dp"
            android:insetBottom="0dp"
            android:backgroundTint="@color/green"
            android:text="登  录"
            app:cornerRadius="12dp" />

    </LinearLayout>
</layout>
```

<span style="background-color:Darkorange ;"><b>注意：</b></span>

+ TextView使用的是`android:text="@{viewModel.user.account}"`，意思是将数据源的值赋值给TextView
+ TextInputEditText使用的是`android:text="@={viewModel.user.account}"`,意思是将输入的数据赋值给数据源

接下来修改MainActivity种的代码：

```java
public class MainActivity extends AppCompatActivity {
    private ActivityMainBinding dataBinding;
    private MainViewModel mainViewModel;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //数据绑定视图
        dataBinding = DataBindingUtil.setContentView(this, R.layout.activity_main);

        mainViewModel = new MainViewModel();
        //Model → View
        User user = new User("admin", "123456");
        mainViewModel.getUser().setValue(user);
        //获取观察对象
        MutableLiveData<User> user1 = mainViewModel.getUser();
        user1.observe(this, user2 -> dataBinding.setViewModel(mainViewModel));

        dataBinding.btnLogin.setOnClickListener(v -> {
            if (mainViewModel.user.getValue().getAccount().isEmpty()) {
                Toast.makeText(MainActivity.this, "请输入账号", Toast.LENGTH_SHORT).show();
                return;
            }
            if (mainViewModel.user.getValue().getPwd().isEmpty()) {
                Toast.makeText(MainActivity.this, "请输入密码", Toast.LENGTH_SHORT).show();
                return;
            }
            Toast.makeText(MainActivity.this, "登录成功", Toast.LENGTH_SHORT).show();
        });
    }

}
```

<span style="background-color:#a2e043;"><b>运行结果：</b></span>

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306041414668.gif" alt="untitled (3)" style="zoom:25%;" />
