# 高阶函数

## 替代回调监听

  在Java中，我们要对外暴露一个回调接口时，步骤很麻烦，需要先定义一个接口，然后
声明该接口的实例，添加set方法，在使用时还必须进行非空判断，再进行调用；

  在Kotlin中只需通过声明一个可为空的函数类型变量，在需要的地方，调用该变量的invoke
方法即可：

```
  var scrollListener: ((x: Int, y: Int) -> Unit)? = null

```

使用：

```
  scrollListener?.invoke(l, t)
```

两行代码即可搞定；使用时，赋给该变量匿名函数即可；

```
  scrollListener = {x,y -> println("xxx")}
  
```

> 函数类型实质就是FunctionX接口，比如上面(x: Int, y: Int) -> Unit对应Function2<Int,Int,Unit>接口，也就是说我们实质还是对外暴露了一个回调接口。这种方式在Java中也可以借鉴，比如在项目中引入了RxJava，那么可以发现其中包含了大量的Function接口（1.x），同样可以在Java中借鉴Kotlin的这种使用方式。


## infix中缀表达式

使用中缀表达式修饰的方法使用起来类似于运算符，一般是用来定义DSL（Domain Special Language）
中缀表达式允许我们在调用类的方法时不再需要使用".()"的形式，类似定义运算符；

当函数符合以下：
- 是成员函数或扩展函数；
- **只有一个参数**；
- 用 infix 关键字标注。

则可以使用中缀表达式的形式进行表示；

```
// 给 Int 定义扩展
infix fun Int.shl(x: Int): Int {
……
}

// 用中缀表示法调用扩展函数
1 shl 2

// 等同于这样
1.shl(2)
```

#### 中缀表达式实现复合函数

函数的复合通过增加Function接口的扩展方法，并通过增加infix表达式来实现操作函数的目的，操作后形成新的复合函数:
```
//符合Function1规范
val add={ i: Int-> i + 2}
//符合Function1规范
val multiply={x:Int-> x * 3}

//定义了两个Function1的复合函数
infix fun <P1, P2, R> Function1<P1, P2>.compose(function: Function1<P2, R>): Function1<P1, R> {
    return fun(p: P1): R {
        return function.invoke(this.invoke(p))
    }
}

val compose = add compose multiply
print(compose.invoke(2))

```

此处通过复合了两个Function1来实现复合函数，以后**凡是符合Function1规范的函数类型，都可以使用该方式来进行调用；**
实际操作中可以通过扩展不同类型的Function来实现复合;


## 偏函数

1、函数通过指定默认值，其实已经可以实现部分偏函数的效果，但是，这种方式存在局限性，如果参数是位于中间，我们必须通过具名函数的方式来传值，较为麻烦；

2、通过参数科里化：当要构造的偏函数的参数恰好处于科里化参数列表的最后一个参数，则可以通过科里化来实现偏函数；
```
    fun add3Curr(x:Int)=fun(y:Int)= fun(z: Int) = x + y + z
    val z = add3Curr(1)(2)
    //此处通过科里化，构造出一个新的函数，
    //该函数只需指定z的值即可
    z(3)
```

3、通过扩展多个FunctionX实现任意位置的偏函数

```
fun testName(x: Int, y: String, z: Boolean) {
    println("i am partial ${y} $z  $x")
}
```

 通过扩展Function3的3个参数，可以实现任意一个参数的偏函数:
 
```
//定义三个参数函数的偏函数
//参数1的偏函数
fun <P1,P2,P3,R> Function3<P1,P2,P3,R>.partial1(p3:P3)=fun(p2:P2)= fun(p1: P1) = this(p1, p2, p3)
//参数2的偏函数
fun <P1,P2,P3,R> Function3<P1,P2,P3,R>.partial2(p3:P3)=fun(p1:P1)= fun(p2: P2) = this(p1, p2, p3)
//参数3的偏函数
fun <P1,P2,P3,R> Function3<P1,P2,P3,R>.partial3(p1:P1)=fun(p2:P2)= fun(p3: P3) = this(p1, p2, p3)

//partial1的第2,3个参数已经被设置为默认，第3个参数可以是任意值
val partial1 = ::testName.partial1(true)("1")

val partial2 = ::testName.partial2(false).invoke(2)

val partial3 = ::testName.partial3(3).invoke("3")

partial1.invoke(1)
partial2.invoke("2")
partial3.invoke(true)
```

