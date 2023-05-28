### BRVAH

#### **前言**



#### 一、简单使用

##### <span style="background-color:#00FFFF;"><b>使用方法：</b></span>

**1.添加依赖**

```java
    implementation 'com.github.CymChad:BaseRecyclerViewAdapterHelper:3.0.10'
```

> sync后可能出现
>
> > Failed to resolve: com.github.CymChad:BaseRecyclerViewAdapterHelper:3.0.10
> >
> > 上网查询后发现主要是
> >
> > ```java
> > maven { url 'https://jitpack.io' }
> > ```
> >
> > 没有添加，上网查询后发现大部分都是添加在allprojects 中，但这跟我的好像不太匹配，因此只展示一下我如何解决的
>
> **解决方法：**
>
> <font color="gray"><b>1.检查build.gradle(Project:你自己的project名)中是否有这一句代码，有就删掉</b></font>
>
> ```java
> buildscript {repositories {maven{ url 'https://maven.aliyun.com/repository/google'}
>     maven{ url 'https://maven.aliyun.com/repository/gradle-plugin'}
>     maven{ url 'https://maven.aliyun.com/repository/public'}
>     maven{ url 'https://maven.aliyun.com/repository/jcenter'}
>     google()
>     jcenter()}
>     dependencies {
>         classpath 'com.android.tools.build:gradle:7.1.2'
>         // NOTE: Do not place your application dependencies here; they belong
>         // in the individual module build.gradle files}
>     }}
> plugins {
>     id 'com.android.application' version '7.3.1' apply false
>     id 'com.android.library' version '7.3.1' apply false
> }
> ```
>
> <font color="gray"><b>2.在settings.gradle(projret settings)中添加这句代码</b></font>
>
> ```java
> pluginManagement {
>     repositories {
>         gradlePluginPortal()
>         google()
>         mavenCentral()
> 
>     }
> }
> dependencyResolutionManagement {
>     repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
>     repositories {
>         google()
>         mavenCentral()
>         maven { url 'https://jitpack.io' }
>     }
> }
> rootProject.name = "TabLayout"
> include ':app'
> 
> ```
>
> 注意：上面那两个文件中只能有一句maven { url 'https://jitpack.io' }，否则会报错

**2.建立一个`Bean`类**

```java
public class Person {
    private String name;
    private int age;
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
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
}
```

**3.创建`list_item_person.xml`，并在`activity_main.xml`中添加recyclerView**

```css

<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">
    <TextView
        android:id="@+id/tv_name"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="16sp"
        android:textStyle="bold"
        android:layout_marginLeft="16dp"/>
    <TextView
        android:id="@+id/tv_age"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@+id/tv_name"
        android:layout_marginLeft="16dp"/>
</RelativeLayout>
```

```css
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:layout_marginStart="1dp"
        android:layout_marginTop="1dp"
        android:layout_marginEnd="1dp"
        android:layout_marginBottom="1dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

**4.创建适配器继承自`BaseQuickAdapter<T, VH>`，第一个泛型是Bean，第二个填`BaseViewHolder`就好**

```java
public class PersonAdapter extends BaseQuickAdapter<Person,BaseViewHolder> {
    public PersonAdapter(int layoutResId, List<Person> data) {
        super(layoutResId, data);
    }
    public PersonAdapter(int layoutResId) {
        super(layoutResId);
    }
    @Override
    protected void convert(BaseViewHolder helper, Person person) {
        helper.setText(R.id.tv_name, person.getName())
                .setText(R.id.tv_age, String.valueOf(person.getAge()));
        TextView textView1=helper.getView(R.id.tv_name);
        TextView textView2=helper.getView(R.id.tv_age);
//        textView1.setOnClickListener(new View.OnClickListener() {
//            @Override
//            public void onClick(View v) {
//                Snackbar.make(textView1,"4",Snackbar.LENGTH_SHORT).show();
//            }
//        });
    }

}
```

- 两个构造方法比较好理解，参数分别是布局资源ID和初始数据。
- `convert`方法类似原始适配器中的`onBindViewHolder()`方法,用于绑定数据、设置事件等。

**5.绑定适配器并添加监听事件**

```java
public class MainActivity extends AppCompatActivity {
    private RecyclerView mRecyclerView;
    private List<Person> mPersonList = new ArrayList<>();
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mRecyclerView = findViewById(R.id.recyclerView);
        mRecyclerView.setLayoutManager(new LinearLayoutManager(this));
        initData();
        PersonAdapter adapter = new PersonAdapter(R.layout.list_item_person, mPersonList);
 		//监听事件
        adapter.addChildClickViewIds(R.id.tv_name,R.id.tv_age);
        adapter.setOnItemChildClickListener(new OnItemChildClickListener() {
            @Override
            public void onItemChildClick(@NonNull BaseQuickAdapter adapter, @NonNull View view, int position) {
                if(view.getId()==R.id.tv_name){
                    Toast.makeText(MainActivity.this,"999",Toast.LENGTH_SHORT).show();
                }
            }
        });
        mRecyclerView.setAdapter(adapter);

    }
    private void initData() {
        mPersonList.add(new Person("John", 30));
        mPersonList.add(new Person("Jane", 25));
        mPersonList.add(new Person("Bob", 45));
        mPersonList.add(new Person("Alice", 28));
        mPersonList.add(new Person("Sam", 20));
        mPersonList.add(new Person("Kate", 35));
    }
}
```

##### <span style="background-color:#a2e043;"><b>结果：</b></span>

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202305122256201.gif" alt="QQ录屏20230512225556_-original-original" style="zoom: 80%;" />
