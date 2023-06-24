**①首先修改theme文件中的代码(深色模式中的也要改)：**

```java
<style name="Theme.NavigationView" parent="Theme.MaterialComponents.DayNight.NoActionBar">
```

**②在color文件中添加颜色：**

> 后面四个颜色是上面的那四个颜色的`A%`改为20得到的
>
> ![image-20230621140957340](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306211409460.png)

```java
    <color name="home">#1194AA</color>
    <color name="like">#C9379D</color>
    <color name="notification">#AE7E0F</color>
    <color name="profile">#5B37B7</color>

    <color name="home_20">#331194AA</color>
    <color name="like_20">#33C9379D</color>
    <color name="notification_20">#33AE7E0F</color>
    <color name="profile_20">#335B37B7</color>
```

**③添加图标文件**

图标文件分为两部分，一部分是未选中的图标，一个是选中后的图标，文件结构如下

![image-20230621142403420](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306211424464.png)

选中状态的图标只需要修改`path`中的`fileColor`为之前添加的颜色即可

![image-20230621142642740](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306211426783.png)

**④在drawable文件中添加三个按钮被选中时布局的背景**



```css
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle"
    >
    <solid android:color="@color/home_20"/>
    <corners android:radius="100dp"/>
</shape>
```

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306211502762.png" alt="image-20230621150250710" style="zoom:80%;" />

设置背景后的情况![image-20230621150357721](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306211503753.png)

**⑤修改布局文件**

```css
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    >
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:weightSum="4"
        android:paddingStart="20dp"
        android:paddingEnd="20dp"
        android:paddingTop="15dp"
        android:paddingBottom="15dp"
        android:elevation="10dp"
        android:gravity="center"
        android:layout_alignParentBottom="true"
        android:background="@color/white"
        >

        <LinearLayout
            android:background="@drawable/round_back_home_100"
            android:layout_width="wrap_content"
            android:layout_height="50dp"
            android:orientation="horizontal"
            android:paddingStart="5dp"
            android:paddingEnd="5dp"
            android:gravity="center"
            android:layout_weight="1"
            >
        <ImageView
            android:layout_width="20dp"
            android:layout_height="wrap_content"
            android:adjustViewBounds="true"
            android:src="@drawable/home_selected"
            />
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="home"
                android:layout_marginStart="10dp"
                android:textStyle="bold"
                android:textColor="@color/home"
                android:textSize="16sp"
                />
        </LinearLayout>
    </LinearLayout>

</RelativeLayout>
```

<span style="background-color:#a2e043;"><b>效果图：</b></span>

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306211513280.png" alt="image-20230621151339235" style="zoom: 50%;" />

之后将按钮的代码赋值粘贴后，修改布局的backgroud，设置TextView的text、visibility和textColor，ImageView的src后如下：

```css
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/home"
    tools:context=".MainActivity"
    >
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:weightSum="4"
        android:paddingStart="20dp"
        android:paddingEnd="20dp"
        android:paddingTop="15dp"
        android:paddingBottom="15dp"
        android:elevation="10dp"
        android:gravity="center"
        android:layout_alignParentBottom="true"
        android:background="@color/white"
        >

        <LinearLayout
            android:id="@+id/homeLayout"
            android:background="@drawable/round_back_home_100"
            android:layout_width="wrap_content"
            android:layout_height="50dp"
            android:orientation="horizontal"
            android:paddingStart="5dp"
            android:paddingEnd="5dp"
            android:gravity="center"
            android:layout_weight="1"
            >

            <ImageView
                android:id="@+id/home_im"
                android:layout_width="20dp"
                android:layout_height="wrap_content"
                android:adjustViewBounds="true"
                android:src="@drawable/home_selected" />

            <TextView
                android:id="@+id/home_tx"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginStart="10dp"
                android:text="home"
                android:textColor="@color/home"
                android:textSize="16sp"
                android:textStyle="bold" />
        </LinearLayout>
        <LinearLayout
            android:id="@+id/likeLayout"
            android:background="@android:color/transparent"
            android:layout_width="wrap_content"
            android:layout_height="50dp"
            android:orientation="horizontal"
            android:paddingStart="5dp"
            android:paddingEnd="5dp"
            android:gravity="center"
            android:layout_weight="1"
            >

            <ImageView
                android:id="@+id/like_im"
                android:layout_width="20dp"
                android:layout_height="wrap_content"
                android:adjustViewBounds="true"
                android:src="@drawable/like" />
            <TextView
                android:id="@+id/like_tx"
                android:visibility="gone"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="like"
                android:layout_marginStart="10dp"
                android:textStyle="bold"
                android:textColor="@color/home"
                android:textSize="16sp"
                />
        </LinearLayout>
        <LinearLayout
            android:id="@+id/notificationLayout"
            android:background="@android:color/transparent"
            android:layout_width="wrap_content"
            android:layout_height="50dp"
            android:orientation="horizontal"
            android:paddingStart="5dp"
            android:paddingEnd="5dp"
            android:gravity="center"
            android:layout_weight="1"
            >

            <ImageView
                android:id="@+id/nitification_im"
                android:layout_width="20dp"
                android:layout_height="wrap_content"
                android:adjustViewBounds="true"
                android:src="@drawable/notification" />
            <TextView
                android:id="@+id/notification_tx"
                android:visibility="gone"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="notification"
                android:layout_marginStart="10dp"
                android:textStyle="bold"
                android:textColor="@color/home"
                android:textSize="16sp"
                />
        </LinearLayout>
        <!--        布局的宽度根据权值平局分配-->
        <LinearLayout
            android:id="@+id/profileLayout"
            android:background="@android:color/transparent"
            android:layout_width="wrap_content"
            android:layout_height="50dp"
            android:orientation="horizontal"
            android:paddingStart="5dp"
            android:paddingEnd="5dp"
            android:gravity="center"
            android:layout_weight="1"
            >

            <ImageView
                android:id="@+id/profile_im"
                android:layout_width="20dp"
                android:layout_height="wrap_content"
                android:adjustViewBounds="true"
                android:src="@drawable/profile" />
            <TextView
                android:id="@+id/profile_tx"
                android:visibility="gone"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="profile"
                android:layout_marginStart="10dp"
                android:textStyle="bold"
                android:textColor="@color/profile"
                android:textSize="16sp"
                />
        </LinearLayout>
    </LinearLayout>

</RelativeLayout>
```

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306211532570.png" alt="image-20230621153213510" style="zoom:80%;" />

**⑥修改`MainActivity`中的代码**

```java
public class MainActivity extends AppCompatActivity {
    /**
     * 被选中的底部按钮的序号，默认值为1
     */
    private int selectedTab =1;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        final LinearLayout homeLayout=findViewById(R.id.homeLayout);
        final LinearLayout likeLayout=findViewById(R.id.likeLayout);
        final LinearLayout notificationLayout=findViewById(R.id.notificationLayout);
        final LinearLayout profileLayout=findViewById(R.id.profileLayout);

        final ImageView homeImage =findViewById(R.id.home_im);
        final ImageView likeImage =findViewById(R.id.like_im);
        final ImageView notificationImage =findViewById(R.id.notification_im);
        final ImageView profileImage =findViewById(R.id.profile_im);

        final TextView homeText=findViewById(R.id.home_tx);
        final TextView likeText=findViewById(R.id.like_tx);
        final TextView notificationText=findViewById(R.id.notification_tx);
        final TextView profileText=findViewById(R.id.profile_tx);

        homeLayout.setOnClickListener(v -> {

            //检查是否被选中
            if (selectedTab!=1){

                //设置其他按钮为未选中状态
                likeText.setVisibility(View.GONE);
                notificationText.setVisibility(View.GONE);
                profileText.setVisibility(View.GONE);

                likeImage.setImageResource(R.drawable.like);
                notificationImage.setImageResource(R.drawable.notification);
                profileImage.setImageResource(R.drawable.profile);


                likeLayout.setBackgroundColor(getResources().getColor(R.color.white));
                notificationLayout.setBackgroundColor(getResources().getColor(R.color.white));
                profileLayout.setBackgroundColor(getResources().getColor(R.color.white));

                //设置home按钮的选中状态
                homeText.setVisibility(View.VISIBLE);
                homeImage.setImageResource(R.drawable.home_selected);
                homeLayout.setBackgroundResource(R.drawable.round_back_home_100);

                //添加动画
                ScaleAnimation scaleAnimation=new ScaleAnimation(0.8f,1.0f,1f,1f,Animation.RELATIVE_TO_SELF,0.0f,Animation.RELATIVE_TO_SELF,0.0f);
                scaleAnimation.setDuration(200);
                scaleAnimation.setFillAfter(true);
                homeLayout.startAnimation(scaleAnimation);

                selectedTab=1;
            }
        });

        likeLayout.setOnClickListener(v -> {

            //检查是否被选中
            if (selectedTab!=2){

                //设置其他按钮为未选中状态
                homeText.setVisibility(View.GONE);
                notificationText.setVisibility(View.GONE);
                profileText.setVisibility(View.GONE);

                homeImage.setImageResource(R.drawable.home);
                notificationImage.setImageResource(R.drawable.notification);
                profileImage.setImageResource(R.drawable.profile);

                homeLayout.setBackgroundColor(getResources().getColor(R.color.white));
                notificationLayout.setBackgroundColor(getResources().getColor(R.color.white));
                profileLayout.setBackgroundColor(getResources().getColor(R.color.white));

                //设置like按钮的选中状态
                likeText.setVisibility(View.VISIBLE);
                likeImage.setImageResource(R.drawable.like_selected);
                likeLayout.setBackgroundResource(R.drawable.round_back_like_100);

                //添加动画
                ScaleAnimation scaleAnimation=new ScaleAnimation(0.8f,1.0f,1f,1f,Animation.RELATIVE_TO_SELF,1.0f,Animation.RELATIVE_TO_SELF,0.0f);
                scaleAnimation.setDuration(200);
                scaleAnimation.setFillAfter(true);
                likeLayout.startAnimation(scaleAnimation);

                selectedTab=2;
            }
        });

        notificationLayout.setOnClickListener(v -> {

            //检查是否被选中
            if (selectedTab!=3){

                //设置其他按钮为未选中状态
                homeText.setVisibility(View.GONE);
                likeText.setVisibility(View.GONE);
                profileText.setVisibility(View.GONE);

                homeImage.setImageResource(R.drawable.home);
                likeImage.setImageResource(R.drawable.like);
                profileImage.setImageResource(R.drawable.profile);

                homeLayout.setBackgroundColor(getResources().getColor(R.color.white));
                likeLayout.setBackgroundColor(getResources().getColor(R.color.white));
                profileLayout.setBackgroundColor(getResources().getColor(R.color.white));

                //设置notification按钮的选中状态
                notificationText.setVisibility(View.VISIBLE);
                notificationImage.setImageResource(R.drawable.notification_selected);
                notificationLayout.setBackgroundResource(R.drawable.round_back_notification_100);

                //添加动画
                ScaleAnimation scaleAnimation=new ScaleAnimation(0.8f,1.0f,1f,1f,Animation.RELATIVE_TO_SELF,1.0f,Animation.RELATIVE_TO_SELF,0.0f);
                scaleAnimation.setDuration(200);
                scaleAnimation.setFillAfter(true);
                notificationLayout.startAnimation(scaleAnimation);

                selectedTab=3;
            }

        });

        profileLayout.setOnClickListener(v -> {

            //检查是否被选中
            if (selectedTab!=4){

                //设置其他按钮为未选中状态
                homeText.setVisibility(View.GONE);
                notificationText.setVisibility(View.GONE);
                likeText.setVisibility(View.GONE);

                homeImage.setImageResource(R.drawable.home);
                notificationImage.setImageResource(R.drawable.notification);
                likeImage.setImageResource(R.drawable.like);

                homeLayout.setBackgroundColor(getResources().getColor(R.color.white));
                notificationLayout.setBackgroundColor(getResources().getColor(R.color.white));
                profileLayout.setBackgroundColor(getResources().getColor(R.color.white));

                //设置profile按钮的选中状态
                profileText.setVisibility(View.VISIBLE);
                profileImage.setImageResource(R.drawable.profile_selected);
                profileLayout.setBackgroundResource(R.drawable.round_back_profile_100);

                //添加动画
                ScaleAnimation scaleAnimation=new ScaleAnimation(0.8f,1.0f,1f,1f,Animation.RELATIVE_TO_SELF,1.0f,Animation.RELATIVE_TO_SELF,0.0f);
                scaleAnimation.setDuration(200);
                scaleAnimation.setFillAfter(true);
                profileLayout.startAnimation(scaleAnimation);

                selectedTab=4;
            }

        });
    }



}
```

**⑦创建各个界面的Fragment**

**结构如下：**

![image-20230621164717363](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306211647395.png)

**布局文件：**

```css
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/home"
    tools:context=".HomeFragment">
<TextView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:text="主页"
    android:textSize="20sp"
    android:textStyle="bold"
    android:textColor="@color/black"
    />


</FrameLayout>
```

并在`activity_main.xml`文件中添加Fragment的容器

![image-20230621165109914](https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306211651964.png)

修改`MainActivity`中的代码，添加Fragment的切换：

```java
public class MainActivity extends AppCompatActivity {
    /**
     * 被选中的底部按钮的序号，默认值为1
     */
    private int selectedTab =1;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        final LinearLayout homeLayout=findViewById(R.id.homeLayout);
        final LinearLayout likeLayout=findViewById(R.id.likeLayout);
        final LinearLayout notificationLayout=findViewById(R.id.notificationLayout);
        final LinearLayout profileLayout=findViewById(R.id.profileLayout);

        final ImageView homeImage =findViewById(R.id.home_im);
        final ImageView likeImage =findViewById(R.id.like_im);
        final ImageView notificationImage =findViewById(R.id.notification_im);
        final ImageView profileImage =findViewById(R.id.profile_im);

        final TextView homeText=findViewById(R.id.home_tx);
        final TextView likeText=findViewById(R.id.like_tx);
        final TextView notificationText=findViewById(R.id.notification_tx);
        final TextView profileText=findViewById(R.id.profile_tx);

        //设置默认界面为HomeFragment
        getSupportFragmentManager().beginTransaction()
                        .setReorderingAllowed(true)
                                .replace(R.id.fragment_container,HomeFragment.class,null)
                                        .commit();


        homeLayout.setOnClickListener(v -> {

            //检查是否被选中
            if (selectedTab!=1){

                //设置界面为HomeFragment
                getSupportFragmentManager().beginTransaction()
                        .setReorderingAllowed(true)
                        .replace(R.id.fragment_container,HomeFragment.class,null)
                        .commit();

                //设置其他按钮为未选中状态
                likeText.setVisibility(View.GONE);
                notificationText.setVisibility(View.GONE);
                profileText.setVisibility(View.GONE);

                likeImage.setImageResource(R.drawable.like);
                notificationImage.setImageResource(R.drawable.notification);
                profileImage.setImageResource(R.drawable.profile);


                likeLayout.setBackgroundColor(getResources().getColor(R.color.white));
                notificationLayout.setBackgroundColor(getResources().getColor(R.color.white));
                profileLayout.setBackgroundColor(getResources().getColor(R.color.white));

                //设置home按钮的选中状态
                homeText.setVisibility(View.VISIBLE);
                homeImage.setImageResource(R.drawable.home_selected);
                homeLayout.setBackgroundResource(R.drawable.round_back_home_100);

                //添加动画
                ScaleAnimation scaleAnimation=new ScaleAnimation(0.8f,1.0f,1f,1f,Animation.RELATIVE_TO_SELF,0.0f,Animation.RELATIVE_TO_SELF,0.0f);
                scaleAnimation.setDuration(200);
                scaleAnimation.setFillAfter(true);
                homeLayout.startAnimation(scaleAnimation);

                selectedTab=1;
            }
        });

        likeLayout.setOnClickListener(v -> {

            //检查是否被选中
            if (selectedTab!=2){

                //设置界面为LikeFragment
                getSupportFragmentManager().beginTransaction()
                        .setReorderingAllowed(true)
                        .replace(R.id.fragment_container,LikeFragment.class,null)
                        .commit();

                //设置其他按钮为未选中状态
                homeText.setVisibility(View.GONE);
                notificationText.setVisibility(View.GONE);
                profileText.setVisibility(View.GONE);

                homeImage.setImageResource(R.drawable.home);
                notificationImage.setImageResource(R.drawable.notification);
                profileImage.setImageResource(R.drawable.profile);

                homeLayout.setBackgroundColor(getResources().getColor(R.color.white));
                notificationLayout.setBackgroundColor(getResources().getColor(R.color.white));
                profileLayout.setBackgroundColor(getResources().getColor(R.color.white));

                //设置like按钮的选中状态
                likeText.setVisibility(View.VISIBLE);
                likeImage.setImageResource(R.drawable.like_selected);
                likeLayout.setBackgroundResource(R.drawable.round_back_like_100);

                //添加动画
                ScaleAnimation scaleAnimation=new ScaleAnimation(0.8f,1.0f,1f,1f,Animation.RELATIVE_TO_SELF,1.0f,Animation.RELATIVE_TO_SELF,0.0f);
                scaleAnimation.setDuration(200);
                scaleAnimation.setFillAfter(true);
                likeLayout.startAnimation(scaleAnimation);

                selectedTab=2;
            }
        });

        notificationLayout.setOnClickListener(v -> {

            //检查是否被选中
            if (selectedTab!=3){

                //设置界面为NotificationFragment
                getSupportFragmentManager().beginTransaction()
                        .setReorderingAllowed(true)
                        .replace(R.id.fragment_container,NotificationFragment.class,null)
                        .commit();

                //设置其他按钮为未选中状态
                homeText.setVisibility(View.GONE);
                likeText.setVisibility(View.GONE);
                profileText.setVisibility(View.GONE);

                homeImage.setImageResource(R.drawable.home);
                likeImage.setImageResource(R.drawable.like);
                profileImage.setImageResource(R.drawable.profile);

                homeLayout.setBackgroundColor(getResources().getColor(R.color.white));
                likeLayout.setBackgroundColor(getResources().getColor(R.color.white));
                profileLayout.setBackgroundColor(getResources().getColor(R.color.white));

                //设置notification按钮的选中状态
                notificationText.setVisibility(View.VISIBLE);
                notificationImage.setImageResource(R.drawable.notification_selected);
                notificationLayout.setBackgroundResource(R.drawable.round_back_notification_100);

                //添加动画
                ScaleAnimation scaleAnimation=new ScaleAnimation(0.8f,1.0f,1f,1f,Animation.RELATIVE_TO_SELF,1.0f,Animation.RELATIVE_TO_SELF,0.0f);
                scaleAnimation.setDuration(200);
                scaleAnimation.setFillAfter(true);
                notificationLayout.startAnimation(scaleAnimation);

                selectedTab=3;
            }

        });

        profileLayout.setOnClickListener(v -> {

            //检查是否被选中
            if (selectedTab!=4){

                //设置界面为ProfileFragment
                getSupportFragmentManager().beginTransaction()
                        .setReorderingAllowed(true)
                        .replace(R.id.fragment_container,ProfileFragment.class,null)
                        .commit();

                //设置其他按钮为未选中状态
                homeText.setVisibility(View.GONE);
                notificationText.setVisibility(View.GONE);
                likeText.setVisibility(View.GONE);

                homeImage.setImageResource(R.drawable.home);
                notificationImage.setImageResource(R.drawable.notification);
                likeImage.setImageResource(R.drawable.like);

                homeLayout.setBackgroundColor(getResources().getColor(R.color.white));
                notificationLayout.setBackgroundColor(getResources().getColor(R.color.white));
                profileLayout.setBackgroundColor(getResources().getColor(R.color.white));

                //设置profile按钮的选中状态
                profileText.setVisibility(View.VISIBLE);
                profileImage.setImageResource(R.drawable.profile_selected);
                profileLayout.setBackgroundResource(R.drawable.round_back_profile_100);

                //添加动画
                ScaleAnimation scaleAnimation=new ScaleAnimation(0.8f,1.0f,1f,1f,Animation.RELATIVE_TO_SELF,1.0f,Animation.RELATIVE_TO_SELF,0.0f);
                scaleAnimation.setDuration(200);
                scaleAnimation.setFillAfter(true);
                profileLayout.startAnimation(scaleAnimation);

                selectedTab=4;
            }

        });
    }



}
```

<span style="background-color:#a2e043;"><b>运行结果：</b></span>

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306211727183.gif" alt="untitled" style="zoom:25%;" />

  

**⑧联动ViewPager，实现滑动切换界面**

这个暂时不用，应该是主页界面有Tab和ViewPager的结合
