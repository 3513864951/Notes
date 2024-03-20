# Android Fragment 使用教程

本教程将向您介绍如何在 Android 应用中使用 Fragment。我们将创建一个简单的示例应用来演示如何创建和使用 Fragment。

## 1. 概念简介

Fragment 是 Android 应用中的一种组件，它允许在一个 Activity 中嵌套多个子视图。这使得我们可以在不同屏幕尺寸和方向上创建更加灵活的用户界面。Fragment 可以被视为一个具有生命周期、布局和行为的“子 Activity”。

## 2. 示例应用

我们将创建一个简单的应用，该应用包含一个 Activity 和两个 Fragment。这两个 Fragment 将在 Activity 中并排显示。第一个 Fragment 将显示一个文本框，用于输入文本，第二个 Fragment 将显示一个按钮，当点击该按钮时，会在其下方显示来自第一个 Fragment 的文本。

## 3. 开始创建

### 3.1 创建项目

首先，创建一个新的 Android 项目，选择 "Empty Activity" 作为初始模板。

### 3.2 添加 Fragment

创建两个 Fragment 类，分别命名为 `InputFragment` 和 `DisplayFragment`。这两个类都需要继承自 `androidx.fragment.app.Fragment`。请确保导入正确的包。

### 3.3 布局文件

为这两个 Fragment 创建布局文件：

**input_fragment.xml**

xml

Copy

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <EditText
        android:id="@+id/edit_text"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="输入文本" />

</LinearLayout>
```

**display_fragment.xml**

xml

Copy

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <Button
        android:id="@+id/show_text_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="显示文本" />

    <TextView
        android:id="@+id/text_view"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp" />

</LinearLayout>
```

### 3.4 实现 Fragment 类

在 `InputFragment` 类中，重写 `onCreateView()` 方法并加载布局：

java

Copy

```java
public class InputFragment extends Fragment {

    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        return inflater.inflate(R.layout.input_fragment, container, false);
    }
}
```

在 `DisplayFragment` 类中，同样重写 `onCreateView()` 方法并加载布局。此外，还需要实现一个接口，用于在点击按钮时与 `InputFragment` 通信。

java

Copy

```java
public class DisplayFragment extends Fragment {

    private OnShowTextListener mListener;

    public interface OnShowTextListener {
        void onShowText();
    }

    @Override
    public void onAttach(@NonNull Context context) {
        super.onAttach(context);
        if (context instanceof OnShowTextListener) {
            mListener = (OnShowTextListener) context;
        } else {
            throw new RuntimeException(context.toString() + " 必须实现 OnShowTextListener");
        }
    }

    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.display_fragment, container, false);
        Button button = view.findViewById(R.id.show_text_button);
        button.setOnClickListener(v -> mListener.onShowText());
        return view;
    }

    public void displayText(String text) {
        TextView textView = getView().findViewById(R.id.text_view);
        textView.setText(text);
    }
}
```

### 3.5 修改 Activity

在 `MainActivity` 中，实现 `DisplayFragment.OnShowTextListener` 接口，并重写 `onShowText()` 方法。然后在布局文件中添加这两个 Fragment。

**activity_main.xml**

xml

Copy

```java
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">

    <fragment
        android:id="@+id/input_fragment"
        android:name="com.example.fragmentdemo.InputFragment"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="1"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toStartOf="@+id/display_fragment"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <fragment
        android:id="@+id/display_fragment"
        android:name="com.example.fragmentdemo.DisplayFragment"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="1"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toEndOf="@+id/input_fragment"
        app:layout_constraintTop_toTopOf="parent" />

</LinearLayout>
```

**MainActivity.java**

java

Copy

```java
public class MainActivity extends AppCompatActivity implements DisplayFragment.OnShowTextListener {

    private InputFragment mInputFragment;
    private DisplayFragment mDisplayFragment;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mInputFragment = (InputFragment) getSupportFragmentManager().findFragmentById(R.id.input_fragment);
        mDisplayFragment = (DisplayFragment) getSupportFragmentManager().findFragmentById(R.id.display_fragment);
    }

    @Override
    public void onShowText() {
        String text = mInputFragment.getText();
        mDisplayFragment.displayText(text);
    }
}
```

在 `InputFragment` 中添加一个新方法用于获取输入的文本：

java

Copy

```java
public String getText() {
    View view = getView();
    if (view != null) {
        EditText editText = view.findViewById(R.id.edit_text);
        return editText.getText().toString();
    }
    return "";
}
```

## 4. 测试运行

现在您可以运行该应用了。在模拟器或连接的设备上启动应用，您应该能看到两个 Fragment 并排显示。在左侧的 `InputFragment` 中输入文本，然后点击右侧 `DisplayFragment` 的 "显示文本" 按钮，您应该能看到输入的文本在按钮下方的 TextView 中显示。

## 总结

通过本教程，您学习了如何在 Android 应用中创建和使用 Fragment。您可以使用这些知识为您的应用创建更加灵活和响应式的用户界面。有关更多详细信息和高级用法，请参阅 [官方文档](https://developer.android.com/guide/components/fragments)。

```java
这个代码是一个 Android 应用程序的主活动（MainActivity），它包含了一个底部导航栏（BottomNavigationView）和一个导航控制器（NavController）。

在 onCreate 方法中，首先调用父类的 onCreate 方法，然后使用 ActivityMainBinding 类将布局文件 activity_main.xml 中的视图绑定到 MainActivity 的实例上。接着使用 findViewById 方法获取底部导航栏的实例，并使用 AppBarConfiguration 类创建一个顶部工具栏的配置。然后使用 Navigation 类的 findNavController 方法获取导航控制器的实例，并将其与底部导航栏和顶部工具栏进行关联（使用 NavigationUI 类的 setupActionBarWithNavController 和 setupWithNavController 方法）。最后，将 MainActivity 的内容视图设置为根视图（getRoot）。

这个代码的作用是创建一个具有导航功能的 Android 应用程序，用户可以通过底部导航栏和顶部工具栏来浏览不同的页面和功能。通过导航控制器，应用程序可以管理和控制页面之间的导航，以及处理页面之间的数据传递和状态保存。
```



   **binding还挺好使**



# Fragment

#### <span style="background-color:#a2e043;"><b>使用范例：</b></span>

**1.创建三个Fragment类**

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202304272233268.png" alt="image-20230427223309944" style="zoom: 80%;" />



```java
package com.example.fragment;

import android.os.Bundle;

import androidx.fragment.app.Fragment;

import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;


public class ContactFragment extends Fragment {


    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.fragment_contact, container, false);
    }
}
```

**2.在MainActivity中生成这三个的对象**

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202304280942882.png" alt="image-20230428094245740" style="zoom:67%;" />

**3.设置三个按钮点击后跳转Fragment**

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202304280945413.png" alt="image-20230428094516297" style="zoom:67%;" />

##### **在此基础上完成模拟QQ底部按钮的跳转**

**1.修改底部按钮**

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#EBFADA"
    tools:context=".MainActivity">



    <!--    fragment 显示的地方-->

    <androidx.constraintlayout.widget.ConstraintLayout
        android:id="@+id/display_fragment"
        android:layout_width="300dp"
        android:layout_height="300dp"
        android:background="#00BCD4"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.495"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.429" />

    <androidx.constraintlayout.widget.ConstraintLayout
        android:id="@+id/constraintLayout8"
        android:layout_width="0dp"
        android:layout_height="100dp"
        app:layout_constraintEnd_toStartOf="@+id/constraintLayout7"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintHorizontal_bias="0.5"
        app:layout_constraintStart_toStartOf="parent">

        <ImageView
            android:id="@+id/imageView"
            android:layout_width="60dp"
            android:layout_height="60dp"
            android:layout_marginTop="8dp"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            app:srcCompat="@drawable/message" />

        <TextView
            android:id="@+id/textView2"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="8dp"
            android:text="消息"
            android:textColor="@drawable/textcolor"
            app:layout_constraintEnd_toEndOf="@+id/imageView"
            app:layout_constraintStart_toStartOf="@+id/imageView"
            app:layout_constraintTop_toBottomOf="@+id/imageView" />
    </androidx.constraintlayout.widget.ConstraintLayout>

    <androidx.constraintlayout.widget.ConstraintLayout
        android:id="@+id/constraintLayout7"
        android:layout_width="0dp"
        android:layout_height="100dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toStartOf="@+id/constraintLayout9"
        app:layout_constraintHorizontal_bias="0.5"
        app:layout_constraintStart_toEndOf="@+id/constraintLayout8">

        <ImageView
            android:id="@+id/imageView2"
            android:layout_width="60dp"
            android:layout_height="60dp"
            android:layout_marginTop="8dp"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            app:srcCompat="@drawable/contact" />

        <TextView
            android:id="@+id/textView3"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="8dp"
            android:text="联系人"
            android:textColor="@drawable/textcolor"
            app:layout_constraintEnd_toEndOf="@+id/imageView2"
            app:layout_constraintStart_toStartOf="@+id/imageView2"
            app:layout_constraintTop_toBottomOf="@+id/imageView2" />
    </androidx.constraintlayout.widget.ConstraintLayout>

    <androidx.constraintlayout.widget.ConstraintLayout
        android:id="@+id/constraintLayout9"
        android:layout_width="0dp"
        android:layout_height="100dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.5"
        app:layout_constraintStart_toEndOf="@+id/constraintLayout7">

        <ImageView
            android:id="@+id/imageView3"
            android:layout_width="60dp"
            android:layout_height="60dp"
            android:layout_marginTop="8dp"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            app:srcCompat="@drawable/dongtai" />

        <TextView
            android:id="@+id/textView4"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="8dp"
            android:text="动态"
            android:textColor="@drawable/textcolor"
            app:layout_constraintEnd_toEndOf="@+id/imageView3"
            app:layout_constraintStart_toStartOf="@+id/imageView3"
            app:layout_constraintTop_toBottomOf="@+id/imageView3" />
    </androidx.constraintlayout.widget.ConstraintLayout>

</androidx.constraintlayout.widget.ConstraintLayout>
```

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202304281320455.png" alt="image-20230428132052341" style="zoom:67%;" />

**要实现点击后底部按钮变色还要配套icon图标和字体颜色**

**1.首先是ImageView的**

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202304281323720.png" alt="image-20230428132325624" style="zoom:67%;" />

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/contact_true" android:state_selected="true" />
    <item android:drawable="@drawable/contact_false" android:state_selected="false" />
</selector>

```

**2.接着是字体的颜色**

同样的创建步骤

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202304281324048.png" alt="image-20230428132419985" style="zoom:50%;" />

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@color/selected_true" android:state_selected="true" />
    <item android:drawable="@color/selected_false" android:state_selected="false" />
</selector>
```

**在colors里面配置颜色**

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202304281325600.png" alt="image-20230428132537519" style="zoom:67%;" />

**3.接下来给控件添加这些配置**

![image-20230428132651859](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202304281326928.png)

#### **结果：**

![](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202304291851319.gif)

#### **项目结构：**

![image-20230429191841094](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202304291918200.png)

#### **代码：**

```java
package com.example.fragment;

import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;
import androidx.constraintlayout.widget.ConstraintLayout;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class MainActivity extends AppCompatActivity {
    MessageFragment messageFragment=new MessageFragment();
    DongtaiFragment dongtaiFragment=new DongtaiFragment();
    ContactFragment contactFragment=new ContactFragment();
    ConstraintLayout constraintLayout1;
    ConstraintLayout constraintLayout2;
    ConstraintLayout constraintLayout3;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        constraintLayout1=findViewById(R.id.constraintLayout8);
        constraintLayout2=findViewById(R.id.constraintLayout7);
        constraintLayout3=findViewById(R.id.constraintLayout9);
        //初始化界面，使第一次显示的是messageFragment
        getSupportFragmentManager().beginTransaction().replace(R.id.display_fragment,messageFragment).commit();
        constraintLayout1.setSelected(true);
        constraintLayout2.setSelected(false);
        constraintLayout3.setSelected(false);
        
        constraintLayout1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if(!constraintLayout1.isSelected()){
                    					getSupportFragmentManager().beginTransaction().replace(R.id.display_fragment,messageFragment).commit();
                    constraintLayout1.setSelected(true);
                    constraintLayout2.setSelected(false);
                    constraintLayout3.setSelected(false);
                }

            }
        });
        constraintLayout2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if(!constraintLayout2.isSelected()){
                    getSupportFragmentManager().beginTransaction().replace(R.id.display_fragment,contactFragment).commit();
                    constraintLayout1.setSelected(false);
                    constraintLayout2.setSelected(true);
                    constraintLayout3.setSelected(false);
                }


            }
        });
        constraintLayout3.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if(!constraintLayout3.isSelected()){
                    getSupportFragmentManager().beginTransaction().replace(R.id.display_fragment,dongtaiFragment).commit();
                    constraintLayout1.setSelected(false);
                    constraintLayout2.setSelected(false);
                    constraintLayout3.setSelected(true);
                }



            }
        });

    }
    
}
```

```java
这一句也能换成这个
getSupportFragmentManager().beginTransaction().replace(R.id.display_fragment,dongtaiFragment).commit(); 

switchFragment(dongtaiFragment);

private void switchFragment(Fragment fragment) {
     FragmentManager fm = getSupportFragmentManager();
     FragmentTransaction transaction = fm.beginTransaction();
     transaction.replace(R.id.display_fragment, fragment);
     transaction.commitAllowingStateLoss();
 }
```

#### **与BottomNavigationView结合使用**

**1.将`activity_main.xml`中的底部按钮换成`BottomNavigationView`**

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.constraintlayout.widget.ConstraintLayout
        android:id="@+id/display_fragment"
        android:layout_width="300dp"
        android:layout_height="300dp"
        android:background="#03A9F4"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.495"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.429" />

    <com.google.android.material.bottomnavigation.BottomNavigationView
        android:id="@+id/navigation"
        android:layout_width="match_parent"
        android:layout_height="80dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:itemIconTint="@drawable/bottom_navigation_item_selector"
        app:itemTextColor="@drawable/bottom_navigation_item_selector"
        app:menu="@menu/main_bottom_navigation"
         />

</androidx.constraintlayout.widget.ConstraintLayout>
```

**2.在`MainActivity`添加`BottomNavigationView`的监听事件**

```java
public void initBottomNavigation() {
        mBottomNavigationView = findViewById(R.id.navigation);
        // 添加监听
        getSupportFragmentManager().beginTransaction().replace(R.id.display_fragment,messageFragment).commit();
        mBottomNavigationView.setOnNavigationItemSelectedListener(new BottomNavigationView.OnNavigationItemSelectedListener() {
            @Override
            public boolean onNavigationItemSelected(@NonNull MenuItem item) {
                switch (item.getItemId()) {
                    case R.id.menu_message:
                        switchFragment(messageFragment);
//                        getSupportFragmentManager().beginTransaction().replace(R.id.display_fragment,messageFragment).commit();
                        return  true;
                    /**
                     * 要是不返回true而是用break,那么最后返回的就是后面的那个false,这样就会出现点击底部导航栏按钮后，
                     * 虽然会监听到，但是屏幕中底部导航栏不会发生变化（变成深色的依然是点击前的那个按钮），
                     * 即点击事件执行完后，不会影响控件在屏幕中的变化
                     如果为 true，则将项目显示为选定项目;如果不应选择该项目，则为 false。
                     */
                    case R.id.menu_contacts:
//                        getSupportFragmentManager().beginTransaction().replace(R.id.display_fragment,contactFragment).commit();
                        switchFragment(contactFragment);
                        return  true;
                    case R.id.menu_discover:
                    case R.id.menu_me:
                        getSupportFragmentManager().beginTransaction().replace(R.id.display_fragment,dongtaiFragment).commit();
                        return  true;
                    default:
                        break;
                }
                return false;
            }
        });

    }
```















































