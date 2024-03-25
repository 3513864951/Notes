# Fragment切换

> 记得设置背景颜色

> 退出当前Fragment只需要;
>
> ```kotlin
> supportFragmentManager.popBackStack()
> parentFragmentManager.popBackStack()
> ```

```kotlin
class MainActivity : BaseActivity<ActivityMainBinding>() {
    private val mViewModel: MainViewModel by lazy {
        ViewModelProvider(
            this,
            ViewModelProvider.NewInstanceFactory()
        )[MainViewModel::class.java]
    }
    private lateinit var psychologyFragment: PsychologyFragment
    private lateinit var homeFragment: PersonFragment
    private lateinit var diaryFragment: DiaryFragment
    private lateinit var testFragment:TestResultFragment


    override fun ActivityMainBinding.initBindingView() {
        binding.viewModel = mViewModel
        mViewModel.userId = intent.getLongExtra(ConstantsUtil.EXTRA_USER_ID, 1)
        SmartApplication.pageToSwitch = mViewModel.pageToSwitch


        initFragment()
        setCurrentFragment(psychologyFragment)

        with(mViewModel) {
            safeLaunch {
                pageToSwitch.collect {
                    when (it) {
                        SwitchPage.DiaryEditPage -> {
                            editFragment()
                        }

                        SwitchPage.DiaryMenuPage -> {

                        }

                        SwitchPage.DiaryPage -> {
                            supportFragmentManager.popBackStack()
                        }

                        SwitchPage.PersonalityTestPage -> {
                            personalityTestFragment()
                        }

                        SwitchPage.PersonalityTestResultPage -> {
                            personalityResultFragment()
                        }
                    }
                }
            }
        }

        navView.apply {
            background = null
            setOnItemSelectedListener {
                when(it.itemId){
                    R.id.navigation_psychology ->{
                        setCurrentFragment(psychologyFragment)
                        true
                    }
                    R.id.navigation_test_result ->{
                        setCurrentFragment(testFragment)
                        true
                    }
                    R.id.navigation_diary ->{
                        setCurrentFragment(diaryFragment)
                        true
                    }
                    R.id.navigation_person ->{
                        setCurrentFragment(homeFragment)
                        true
                    }
                    else -> true
                }
            }
        }
    }


    /**
     * 设置当前的Fragment。
     * 该函数用于通过支持库的FragmentManager进行Fragment的替换操作，将指定的Fragment替换为当前容器中的Fragment。
     * @param [fragment] 要设置为当前Fragment的对象，该参数为即将显示在屏幕上的Fragment实例。
     */
    private fun setCurrentFragment(fragment: Fragment) =
        supportFragmentManager.beginTransaction().apply {
            // 使用replace方法替换容器中的Fragment，同时设置新的Fragment为当前Fragment
            replace(R.id.container_fragment, fragment)
            // 允许重新排序，以优化Fragment的事务执行效率
            setReorderingAllowed(true)
            // 提交事务，使其生效
            commit()
        }

    /**
     * 初始化Fragment
     *
     */
    private fun initFragment(){
         homeFragment = PersonFragment()
         psychologyFragment = PsychologyFragment()
         testFragment = TestResultFragment()
        diaryFragment = DiaryFragment()
    }

    /**
     * 切换到性格测试界面
     *
     */
    private fun personalityTestFragment() {
        supportFragmentManager.beginTransaction().apply {
            replace(R.id.top_container, PersonalityTestFragment())
            setReorderingAllowed(true)
            addToBackStack("personalityTestFragment")
        }.commit()
    }

    /**
     * 切换到性格测试结果界面
     *
     */
    private fun personalityResultFragment() {
        supportFragmentManager.beginTransaction().apply {
            replace(R.id.top_container, PersonalityResultFragment())
            setReorderingAllowed(true)
            addToBackStack("personalityResultFragment")
        }.commit()
    }

    /**
     * 切换到日记编辑界面
     *
     */
    private fun editFragment() {
        supportFragmentManager.beginTransaction().apply {
            replace(R.id.top_container, EditFragment())
            setReorderingAllowed(true)
            addToBackStack("editFragment")
        }.commit()
    }
}
```

布局界面：

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <data>

        <variable
            name="viewModel"
            type="com.example.smartpsychologyapp.ui.viewmodel.MainViewModel" />
    </data>

    <FrameLayout
        android:id="@+id/top_container"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <androidx.constraintlayout.widget.ConstraintLayout
            android:id="@+id/container"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:background="@color/white">

            <com.google.android.material.bottomnavigation.BottomNavigationView
                android:id="@+id/nav_view"
                style="@style/BottomNavigationViewStyle"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginStart="0dp"
                android:layout_marginEnd="0dp"
                android:background="?android:attr/windowBackground"
                app:layout_constraintBottom_toBottomOf="parent"
                app:layout_constraintLeft_toLeftOf="parent"
                app:layout_constraintRight_toRightOf="parent"
                app:menu="@menu/bottom_nav_menu" />

            <FrameLayout
                android:id="@+id/container_fragment"
                android:layout_width="match_parent"
                android:layout_height="0dp"
                app:layout_constraintBottom_toTopOf="@+id/nav_view"
                app:layout_constraintLeft_toLeftOf="parent"
                app:layout_constraintRight_toRightOf="parent"
                app:layout_constraintTop_toTopOf="parent"
                 />

        </androidx.constraintlayout.widget.ConstraintLayout>
    </FrameLayout>

</layout>
```

