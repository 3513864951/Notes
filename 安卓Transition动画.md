# 安卓Transition动画



### 1.**Explode** 

> `Explode` 的效果是下一个页面的元素从四面八方进入，最终形成完整的页面。 

<font color="orange"><b>代码：</b></font> 

+ 初始Activity中的代码：

  ```java
          Intent intent=new Intent(this,HomeActivity.class);
          startActivity(intent, ActivityOptions.makeSceneTransitionAnimation(this).toBundle());
  ```

+ 被跳转的Activity中要添加的代码：

  ```java
  getWindow().setEnterTransition(new Explode());
  ```

![image-20230611133300976](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306111333017.png)



<span style="background-color:#a2e043;"><b>运行结果：</b></span>

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306111329812.gif" alt="device-2023-06-11-13_-original-original" style="zoom:25%;" />



### 2.Slide

> `Slide` 就是下一个页面元素从底部依次向上运动，最终形成完整的页面。



<font color="orange"><b>代码：</b></font>

**<font color=Gray>只需要修改被跳转的Activity中的代码就好：</font>**

```JAVA
getWindow().setEnterTransition(new Slide());
```

<span style="background-color:#a2e043;"><b>运行结果：</b></span>

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306111341050.gif" alt="device-2023-06-11-13_-original-original (1)" style="zoom:25%;" />

### 3.Fade

> `Fade` 就是下一个页面元素渐变出现，最终形成完整的页面。



<font color="orange"><b>代码：</b></font>

**<font color=Gray>跟上面的一样，只需要修改被跳转的Acitivity中的代码即可</font>**

```java
getWindow().setEnterTransition(new Fade());
getWindow().setExitTransition(new Fade());
```

<span style="background-color:#a2e043;"><b>运行结果：</b></span>

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306111345086.gif" alt="device-2023-06-11-13_-original-original (2)" style="zoom:25%;" />



有一点小缺陷，下面那个黑色导航栏在切换场景的时候会白闪一下

### 4.Share

> `Share` 是最复杂的一种转场方式，在跳转的两个 Activity 之间，如果有相同的 View 元素，那么，两个元素就可以设置成共享状态，在跳转时，这个 View 就会从第一个 Activity 的显示状态过渡到第二个 Activity 的显示状态，给用户的感觉仿佛是两个 Activity 共享一个 View 。

#### <span style="background-color:#00FFFF;"><b>使用方法：</b></span>

首先创建两个活动`FirstActivity`、`SecondActivity`，分别作为初始活动界面和被跳转界面

**布局文件:**

给布局文件中相同的View元素设置相同的`transitionName`，这样就实现了共享状态

`activity_first.xml`中代码如下：

```css
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/container"
    android:layout_width="match_parent"
    android:layout_height="match_parent">



    <ImageView
        android:id="@+id/imageView1"
        android:layout_width="49dp"
        android:layout_height="49dp"
        android:src="@drawable/img_15"
        android:transitionName="image_transition1" />

    <TextView
        android:id="@+id/text"
        android:layout_width="351dp"
        android:layout_height="48dp"
        android:layout_marginLeft="50dp"
        android:text="我推的孩子 更新啦！"
        android:textAlignment="center"
        android:textSize="25sp"
        android:textStyle="bold"
        android:transitionName="text" />
</RelativeLayout>
```

`activity_second.xml`中代码如下：

```css
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/container"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#BDF3FA"
    xmlns:tools="http://schemas.android.com/tools"
    tools:context=".model6.DetailActivity"
    >


    <ImageView
        android:id="@+id/imageView1"
        android:layout_width="match_parent"
        android:layout_height="510dp"
        android:layout_marginTop="100dp"
        android:layout_centerInParent="true"
        android:src="@drawable/img_15"
        android:transitionName="image_transition1" />

    <TextView
        android:id="@+id/text"
        android:layout_width="285dp"
        android:layout_height="44dp"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="600dp"
        android:text="我推的孩子 更新啦！"
        android:textAlignment="center"
        android:textSize="25sp"
        android:textStyle="bold"
        android:transitionName="text" />
</RelativeLayout>
```

**“活动代码”**

`FirstActivity`中的代码如下：

```java
public class FirstActivity extends AppCompatActivity {
    private ImageView imageView1;
    private TextView text;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        getWindow().requestFeature(Window.FEATURE_CONTENT_TRANSITIONS);
        setContentView(R.layout.activity_first);
        imageView1=findViewById(R.id.imageView1);
        text=findViewById(R.id.text);
        imageView1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
				Intent intent = new Intent(FirstActivity.this, DetailActivity.class);
                androidx.core.util.Pair<View,String> pair_imageView1=new androidx.core.util.Pair<>(imageView1,"image_transition1");
                androidx.core.util.Pair<View,String> pair_textView1=new androidx.core.util.Pair<>(text,"text");
                ActivityOptionsCompat activityOptionsCompat=ActivityOptionsCompat.
                    makeSceneTransitionAnimation(FirstActivity.this,pair_imageView1,pair_textView1);
                startActivity(intent,activityOptionsCompat.toBundle());
            }
        });
    }
}
```

单对元素的写法：

```java
//单个共享元素
                ActivityOptionsCompat options1 = ActivityOptionsCompat.
                    makeSceneTransitionAnimation(FirstActivity.this, imageView1, "image_transition1");
                startActivity(intent, options1.toBundle());
```

根据引入不同的`Pair`包，还有另一种写法：

```java
                Intent intent = new Intent(FirstActivity.this, DetailActivity.class);
//多个共享元素的
                Pair<View,String> pair_imageView=new Pair<>(imageView1,"image_transition1");
                Pair<View,String> pair_textView=new Pair<>(text,"text");
                startActivity(intent, ActivityOptions.makeSceneTransitionAnimation(FirstActivity.this,pair_imageView,pair_textView).toBundle());
//单个共享元素
                ActivityOptions options=ActivityOptions.makeSceneTransitionAnimation(FirstActivity.this,imageView1,"image_transition1");
                startActivity(intent, options.toBundle());

```

`SecondActivity`中的代码如下：

```java
public class SecondActivity extends AppCompatActivity {

    private ImageView imageView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);
        imageView = findViewById(R.id.imageView1);
        imageView.setOnClickListener(v -> {
            onBackPressed();
            //实现点击图片后类似返回键的功能
        });

    }
}
```

<span style="background-color:#a2e043;"><b>运行结果：</b></span>

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306111917061.gif" alt="device-2023-06-11-19_-original-original" style="zoom:25%;" />