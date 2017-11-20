## 内联函数

作用：内联的作用是把函数在编译时，嵌入在每一个调用处，这样能够减少不必要的函数调用，特别是在高阶函数中，能够优化函数结构，减少函数调用时的时间开销，提高运行效率；

使用前提：一般是逻辑较为简单（没有循环、递归等）、代码1~5行、函数调用和返回开销相对较大，并且其适合成为独立的功能模块，此时可以使用内联；

#### Kotlin中的内联

在Kotlin中，运用内联最多的地方就是高阶函数。
> 使用高阶函数会带来一些运行时的效率损失：每一个函数都是一个对象，并且会捕获一个闭包。 即那些在函数体内会访问到的变量。 内存分配（对于函数对象和类）和虚拟调用会引入运行时间开销。

此处的闭包形成原因是内部函数持有了外部函数的局部变量或参数，比如：
```
lock(l) { foo(l) }
```
foo函数使用了外部函数lock的参数：l，则foo函数持有了lock函数的引用，lock的生命周期不再局限于运行期间，而是会受到foo影响，再看一个例子：
```
fun makeFun():()->Unit{
    var count = 0
    return fun(){
        ++count
        println(count)
    }
}

fun main(args: Array<String>) {
    val x = makeFun()
    //每次调用，并不会走原函数流程，所以count不会被置0
    //每次调用x，count都会加1，原函数的生命周期被延长
    //类似于变量，存在了状态
    x()
    x()
    x()
    x()
}
```
闭包让函数生命周期被延长，函数的内存不被释放；

通过使用内联，可减少不必要的闭包：
```
lock(l) { foo() }
```
这种情况下，如果不使用内联，foo还是会隐式持有lock的引用，通过增加inline，则可以去掉闭包：
```
l.lock()
try {
    foo()
}
finally {
    l.unlock()
}
```
#### 非局部返回
一个函数中，如果存在一个lambda表达式，在该lambda中不支持直接进行return退出该函数，比如：

```
fun outterFun() {
    innerFun {
        //return  //错误，不支持直接return
        //只支持通过标签，返回innerFun
        return@innerFun
    }

    //如果是匿名或者具名函数，则支持
    var f = fun(){
        return
    }
}

fun innerFun(a: () -> Unit) {}
```

除非，innerFun是inline函数：
```
fun outterFun() {
    innerFun {
        return  //支持直接返回outterFun
    }
}

inline fun innerFun(a: () -> Unit) {}
```
这种直接在lambda返回外部函数的情况称为非局部返回。

#### noinline 
> 如果你只想被（作为参数）传给一个内联函数的 lamda 表达式中只有一些被内联，你可以用 noinline 修饰符标记一些函数参数：

```
inline fun foo(inlined: () -> Unit, noinline notInlined: () -> Unit) {
    // ……
}
```
> 可以内联的 lambda 表达式只能在内联函数内部调用或者作为可内联的参数传递， 但是 noinline 的可以以任何我们喜欢的方式操作：存储在字段中、传送它等等。

说白了，就是让内联函数的lambda参数不进行内联，保留一般函数的特征。

#### crossinline 
crossinline 的作用是让被标记的lambda表达式不允许非局部返回。
首先，默认内联函数的lambda表达式参数是允许非局部返回的，即上面：
```
fun outterFun() {
    innerFun {
        return  //支持直接返回outterFun
    }
}

inline fun innerFun(a: () -> Unit) {}
```
通过将innerFun的lambda参数标记为crossinline后，return操作将不被允许；
这样做的原因是，官方文档解释：
> 一些内联函数可能调用传给它们的不是直接来自函数体、而是来自另一个执行上下文的 lambda 表达式参数，例如来自局部对象或嵌套函数。在这种情况下，该 lambda 表达式中也不允许非局部控制流。为了标识这种情况，该 lambda 表达式参数需要用 crossinline 修饰符标记。

说白了，就是我们如果直接将lambda参数作为另一个函数的返回值，这种情况是不被允许的：
```
interface TestInterface{
    fun test(a:Int):Int
}

inline fun testInterface(crossinline t: (Int) -> Int): TestInterface = object : TestInterface {
    override fun test(a: Int): Int = t.invoke(a)
}

//调用testInterface
testInterface{
    1
}

//虽然testInterface方法是inline的，但是此处禁止直接return，因为其lambda参数使用了crossinline标记：
testInterface{
    return //错误
}
```

此处我们定义了一个TestInterface接口，并定义了testInterface方法，返回值是TestInterface的实现类实例；
可以看到，test方法中，直接返回的lambda表达式t执行后的返回值：
```
 override fun test(a: Int): Int = t.invoke(a)
```
如果不通过crossinline禁止lambda表达式t直接执行return操作，那么t直接return后，返回值是Unit，这并不符合fun test(a: Int): Int 需要Int返回值的要求，即可能是：
```
override fun test(a: Int): Int = Unit
```

**引用stackoverflow上关于crossinline与noinline的问题：**
thread函数：
```
public fun thread(start: Boolean = true, isDaemon: Boolean = false, contextClassLoader: ClassLoader? = null, name: String? = null, priority: Int = -1, block: () -> Unit): Thread {
    val thread = object : Thread() {
        public override fun run() {
            block()
        }
    }
    if (isDaemon)
        thread.isDaemon = true
    if (priority > 0)
        thread.priority = priority
    if (name != null)
        thread.name = name
    if (contextClassLoader != null)
        thread.contextClassLoader = contextClassLoader
    if (start)
        thread.start()
    return thread
}
```

1、报warning：
```
inline fun test(noinline f: () -> Unit) {
    thread(block = f)
}
```
这种方式内联已经没有用处，等价于：
```
fun test(f: () -> Unit) {
    thread(block = f)
}
```
2、编译错误
```
inline fun test(crossinline f: () -> Unit) {
    thread(block = f)
}
```
因为block是一个普通函数，而f是一个crossinline类型的函数（内联，不支持裸return），除非block也是个crossinline类型的函数。

3、报warning：
```
inline fun test(noinline f: () -> Unit) {
    thread { f() }
}
```
此处如果不声明为noinline或者crossinline，将会报错，因为在thread函数中，block返回值是作为run方法的返回值的：
```
    val thread = object : Thread() {
        public override fun run() {
            block()
        }
    }
```
而声明为noinline或者crossinline都限制了f直接return，noline表示不使用inline，所以报warning与第一种是同一种情况；
4、正确，使用crossinline限制了lambda表达式f直接return
```
inline fun test(crossinline f: () -> Unit) {
    thread { f() }
}
```

参考：
[https://stackoverflow.com/questions/38827186/what-is-the-difference-between-crossinline-and-noinline-in-kotlin/38827414](https://stackoverflow.com/questions/38827186/what-is-the-difference-between-crossinline-and-noinline-in-kotlin/38827414)
[http://www.kotlincn.net/docs/reference/inline-functions.html#具体化的类型参数](http://www.kotlincn.net/docs/reference/inline-functions.html#具体化的类型参数)
