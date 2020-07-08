# Kotlin学习

Kotlin之所以可以在JVM上执行，是因为JVM只认字节码。

### 函数

```kotlin
fun sum(x: Int, y: Int): Int{return x + y}
fun sum(x: Int, y: Int) = return x + y
```



Tips:

1. 一般函数**返回值**都需要显示指定；
2. 函数参数都需要指定





###  var与val

```kotlin
val book = Book()
```

book是指向堆中该对象的引用，val代表引用本身不可变，但引用指向的对象可变。

若将引用理解为对象实体的地址变量，则地址变量本身不可变，但目的地的对象可变。



### 函数式编程

函数可以像变量一样传来传去，或者说函数本身就被当成了复杂组合的基本元素。可以将函数作为参数传入到一个函数中。

函数类型就类似于我们当时解释器做的sml语言：

(参数类型列表) -> 返回值类型

```kotlin
(Int, String) -> Int
() -> Int
//也可以给参数取名字
(name: String, code: Int) -> Unit
//在类型后加上?代表可选

//参数类型和返回值也可设定为另一个函数类型：
((Int)-> Int) -> Int //参数为函数类型
()  -> ((Int) -> Int) //返回值为函数类型
```

（Kotlin中抛弃了基本变量类型，全部使用对象类型，因此Int的i是大写的。



## 逻辑控制

if的特殊用法是if可以有返回值，例子如下：

```kotlin
fun getIfRes(name : String):String = if(name.length == 1){
    name
}else{
    "0"
}
```



when类似于switch的变体，但when后的值类型没有限制：

```kotlin
fun getScore(tag: String) :Int = when(tag){
    "a" -> 1
    "b" -> 2
    else ->{100}
}
```





## 循环

+ while(与java相同)
+ for-in

```kotlin
for(i in 0..10){}
for(j in 0 until 10 step 2)
for(k in 10 downTo 1)
```

也可用来遍历集合，跟python中的for-in一样，多了个括号。

## 面向对象

**kotlin中的类默认不可被继承，相当于加了final**

若希望其被继承则加上open:

```kotlin
open class Person{
}
```

继承：

```kotlin
class Student : Person(){}
```

接口：

```kotlin
interface IA{}
//实现接口与继承一样，不过不用加括号
class Student : IA{}
```



### 权限

默认是public而非包内可见。protected仅对当前类和子类可见。

internal：

只对同一个模块内的类可见。



### 数据类与单例类

分别用data和object关键字即可

```kotlin
data class Cell(val num : Int)
val c = Cell(100)

object SingleTon{
	//定义数据和操作
}
SingleTon.foo()
//此处并非静态方法调用，而是编译器会自动e构建一个类
```



//后面看另外两本书再补充，第一行代码Android讲kotlin实在一般。





