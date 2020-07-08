# Activity in Android

## AndroidManifest

所有的Activity都要在AndroidManifest.xml文件中进行注册才有用。

```xml
<!--AndroidManifest.xml-->
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.ch3">
    <!--上方的包名已经指定了包名，因此在下面的name从.MainActivity开始就可以-->
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
          <!--在intent-filter中指定这个activity为入口-->
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>

</manifest>
```

## 引用控件或布局

每一个activity都与一个layout相关，在代码中用setContentView方法传入一个layout。

在代码中用R.layout.xxxxlayout这样的方式来饮用这个值，在xml中使用"@id/xxx"的方式来引用，其中的R代表res文件夹，所有的布局、values文件应该放在res文件夹下面。

在kotlin中，无需再用findViewById的方式来将控件形成一个对象，**直接输入控件的name，用Studio快捷补全就可以导入这个控件，名字本身就变成了那个对象的引用**。

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        //val button1 : Button = findViewById(R.id.press) //Button是View抽象类的派生对象，findView这里返回值是View，因此要指定类型
        press.setOnClickListener {
            Toast.makeText(this, "苟利国家生死以，岂因祸福避趋之！", Toast.LENGTH_SHORT).show()
        }

    }
}
```



## Activity之间的切换——Intent

Intent是Android中各组件通信的重要方式，此处仅说明在Activity中的应用。

Intent主要分为：

+ 显式

即设置好intent的意图，**目的性**很强，所以称作显示。

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        //val button1 : Button = findViewById(R.id.press) //Button是View抽象类的派生对象，findView这里返回值是View，因此要指定类型
        press.setOnClickListener {
            val intent = Intent(this, SecondActivity::class.java)
            startActivity(intent)
        }

    }
}
```

+ 隐式

不指定实际的目的Activity，而是由系统来找到最适合响应这个Intent的Activity来启动。

```kotlin
//Listener
            val intent = Intent("MyAction")
            intent.addCategory("MyCategory")
            Log.d("SecondActivity", intent.categories.toString())
            startActivity(intent)
```



```xml
//AndroidManifest.xml
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
```

隐式intent不仅可以调用本app内的Activity，也可以调用其他程序的。比如让系统的浏览器来打开一个网页：

```kotlin
press.setOnClickListener {
    val intent = Intent(Intent.ACTION_VIEW)
    intent.data = Uri.parse("https://www.google.com")
    startActivity(intent)
}
```



### Activity之间传递数据

利用intent的putExtra方法即可实现目的。
在需要接收数据的Activity里面直接intent.getStringExtra(keyname)的方式就可以获得对应的value

此处intent不必声明是因为系统自动调用了getIntent方法。此处只能用intent，相当于一个关键字，不能用其他自定义名称来实现这个效果。



### Activity生存周期

Activity借助Back Stack来管理Activity。

几种状态：

+ 运行
+ 暂停：不在栈顶但是可见，比如只占一部分屏幕。
+ 停止：不在栈顶且完全不可见，此时系统会保存状态，但不一定可靠，也有可能被回收掉。
+ 销毁

几种生存期：

首先是onCreate()，AC第一次被创建时被调用，与之相对的是onDestroy()。一般在前者内初始化资源，后者内清理资源。这段时间称作**完整生存期**

Create之后就是onStart()，此时可见性从无到有，与之对应的是onStop()，这段时间称作“可见生存期”

onStart()之后为onResume()，这段时间AC与用户可以交互，对应onPause()。称作前台生存期。

