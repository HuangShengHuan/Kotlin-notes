# 函数、方法

## 作用

函数、方法的定义除了能使功能模块结构化外，还能让一些比较占用资源的代码临时化，即使用完即刻释放，不长时间占用内存空间；

## 基本概念

1. 方法即成员方法，定义在类中的跟类相关的，表示类的功能，特性等；

2. 函数衍生自C语言（C语言本身没有类的概念），没有所谓类的归属，是一个独立的功能单位；

3. 函数不属于任何类，其不能保存状态，也不能受到状态影响，其结果只与输入参数有关，通过参数结果运算结果是相同的，而方法属于类，其运算结果可能受类的状态影响;

4. Java是一门完全面向对象的语言，其不允许将方法定义在类的外面，也就是不允许有函数，但是某些情况下我们需要定义一些与类无关的函数，比如运算的函数，而在Java中只能通过在类中定义静态方法，比如Math类，完全没有运用到类的特性;

## 签名

   函数、方法的签名是由方法名和参数列表(参数类型、个数、顺序)决定的，而返回值是不能决定签名的，因为有时候即使方法有返回值，我们也可以不接收，那么编译器根本不知道我们调用的是哪种返回值的方法；

比如：

```
fun a():String{}

fun a():Int{}
```

当我们调用时不需要返回值：

```
  a()
```

此时编译器不知道我们调用的是哪个方法。

## 字面函数 

**字面量：** 在编程语言中，字面量是一种表示值的记法。例如，"Hello, World!" 在许多语言中都表示一个字符串字面量（string literal ）。
一般除去表达式，给变量赋值时，等号右边都可以认为是字面量。
字面量分为字符串字面量(string literal )、数组字面量(array literal)和对象字面量(object literal)，另外还有函数字面量(function literal)。


    var test="hello world!";
    "hello world!"就是字符串字面量，test是变量名。

    函数字面量(Function Literals)：
    var fn = fun(x){ alert(x); }

 可以认为匿名函数就是函数字面量的表现形式。在Kotlin中，匿名函数除了一般的表现形式外，还可以以lambda表达式的形式进行表示，这些形式表示的函数体就是字面函数。
 
    var fn = fun(){}
    var fn = {}

 上面两种定义了同一个字面函数；
 
[javascript里面的字面量](https://zhidao.baidu.com/question/584062903.html)   

[Js函数字面量和Function()构造函数的区别||匿名函数](http://blog.csdn.net/qq_25178609/article/details/51669870)
[JavaScript对象字面量讲解](http://www.jeepyurongfu.com/1216/6386.html)

## 局部函数

Kotlin 支持局部函数，比如在一个函数包含另一函数。


    fun dfs(graph: Graph) {
      fun dfs(current: Vertex, visited: Set<Vertex>) {
        if (!visited.add(current)) return
        for (v in current.neighbors)
          dfs(v, visited)
      }
    
      dfs(graph.vertices[0], HashSet())
    }
    
## vararg变长参数

  在Kotlin中，定义方法和函数时，参数是不能使用var和val修饰的，因为参数不能定义为属性，
除非是构造方法，但是我们可以使用vararg，表示变长参数；

在Kotlin中，变长参数可以定义在参数列表中的任意位置，不像Java，只能定义在参数列表的最后，

由于变长参数是可以定义在任意位置，所以在调用函数进行赋值时，必须使用具名参数进行赋值，
否则编译器无法识别参数；

比如：
```
fun testVar(vararg a: Int, str: String) {
}

```

在赋给str时我们必须显示指定：
```
testVar(1, 2, 3, str = "haha")
```

变长参数支持传入数组，但必须通过展开运算符*标记；

比如定义
```
 val arr = IntArryOf(1,2,3)
 testVar(*arr,str="haha")
```

## 具名参数

 具名参数即在调用函数时指定传入的参数名称；
 
 通过具名参数，我们可以不按函数参数的顺序传入来调用函数：

 比如：
 ```
 fun testName(x: Int, y: String, z: Boolean) {}
 ```
 
 调用：
 ```
 testName(z = false, y = "", x = 1)
 ```
 
 在函数存在默认参数，并且该参数不是在参数列表的最后时，可以使用具名参数指定传递的参数：
 
 ```
 fun ggg(a: Int = 2, b: Int) {}
 
 //这种情况下，如果想要保留原来的默认参数，则必须使用具名参数指定传递给参数b
 ggg(b = 1)
 ```


## 函数、方法引用

 在定义匿名函数时，我们以声明变量的形式声明匿名函数，这种形式我们能够直接拿到函数的引用：
 
 ```
  var funN = fun(a ：Int){}
 ```
 
 其中，funN表示该匿名函数的引用，调用的方式可以是具名函数的形式，也可以直接调用操作符函数：invoke
 
 ```
  funN(1)
  funN.invoke(1)
 ```
 
 对于具名函数，由于已经声明了函数名，所以我们可以直接用函数名来获取引用：
 
 ```
 //定义一个具名函数
fun test(a:Int){}

//通过函数名来拿到引用
//方式1
::test.invoke(1)
(::test)(1)

//方式2
var f = ::test
f(1)
f.invoke(1)

//
var cpa = String::codePointAt

 ```

### 方法引用

 类的方法也可以使用引用的形式来调用，不同于函数引用，方法引用需要类实例作为参数；
 
  比如：
  
```
//定义String的length方法的引用
var len = String::length

//使用时必须有String的实例
len("hello")
len.invoke("world")

//String的codePointAt原先只有一个参数
var cpa = String::codePointAt

//以引用的方式调用，必须使用类实例作为第一个参数
cpa.invoke("hello", 1)
cpa("hello", 1)
```   

### 扩展方法、属性的引用  

 和方法引用一样，扩展方法和扩展属性也可以使用方法引用的形式进行调用：
 
 ```
 //定义String的扩展方法和扩展属性
 val String.kp: Int get() = 1
 fun String.kz(a: Int) = a

 //使用引用的形式调用 
(String::kz).invoke("a", 1)
(String::kp).invoke("a")

val kzv = String::kz
kzv.invoke("a", 1)

val kpv =String::kp
kpv.invoke("a")
 ```

### 方法引用在高阶函数中使用 
类的方法引用，让我们可以使用另一种形式来调用类的方法，并且这种方式可以作为函数、方法的参数被传递，在高阶函数中，通过使用类的方法引用，构造出符合对应函数签名的参数：

```
//定义高阶函数
fun hfun(a: (String, Int) -> Int) {}

//通过方法引用的形式构造出符合函数签名的参数
hfun(String::codePointAt)

```

 String的codePointAt方法原先的签名为(Int) -> Int，使用方法引用的形式后，变为(String , Int) -> Int，其中的第一个参数为String类实例自身。


## 默认参数与方法重载

在Kotlin中，支持给函数、方法设置默认的参数，这样只需一个参数最多的方法就可实现方法的重载；

```
fun methodOverload(a: Int = 0, b: String = "null", c: Boolean = false) {}
```

### 方法重载的前提

使用方法重载的前提：必须是支持默认参数，才符合重载的要求。也就是说较少参数的方法与较多参数的方法其实逻辑相同，只是较少参数的方法使用了默认值（参看Java中的多个构造方法时，少参数的构造方法会使用默认值，并调用多参数的构造方法），否则不应该使用方法重载，Java中的集合remove方法就犯了该错误。

在Java的集合中存在一个方法重载的问题，集合中存在两个重载方法，remove(int)和remove(object)，如果我们集合中的元素是Integer时，我们将无法正常调用remove(object)，因为这个始终会被识别为remove(int).

Kotlin中则解决了这个问题，当根据index来移除时使用：removeAt(index)
如果是直接移除元素的使用：remove(object)

## 默认参数与Java兼容

Kotlin中支持方法的默认参数，但是Java中不支持，当在Java中调用Kotlin中的方法时，只会有一个有所有参数的方法。
  
为了兼容Java，可以在默认参数方法上添加注解：@JvmOverloads，这样会自动生成多个重载方法。

```
@JvmOverloads
fun methodOverload(a: Int = 0, b: String = "null", c: Boolean = false) {}
```


## 内联函数

作用：内联的作用是把函数的所有代码在编译时，直接插入到调用的地方，这样能够减少不必要的函数调用，特别是在高阶函数中，能够优化函数结构，减少函数调用时的时间开销，提高运行效率；

使用前提：一般是逻辑较为简单（没有循环、递归等）、代码1~5行、函数调用和返回开销相对较大，并且其适合成为独立的功能模块，此时可以使用内联；


## 闭包

#### 概念

闭包就是能够读取其他函数内部变量的函数。

在Javascript语言中，只有函数内部的子函数才能读取函数的局部变量，因此可以把闭包简单理解成"定义在一个函数内部的函数"。

在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。

#### 特点

闭包可以用在许多地方。它的最大用处有两个，一个是可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中。(即原函数中的局部变量的作用域始终不会被清除，值一直保有。)

闭包使函数可以有状态，在函数中可以定义新的函数和新的类；

闭包让函数可以像变量一样被声明，在声明为变量之后，拥有了变量的生命特征，其中的变量的生命周期不再局限于运行期间，而是而是拥有了与普通变量一样的生命周期；

函数这种类似于变量生命周期的特征可以被认为类似于没有停止条件的递归，并且该递归只有在被调用时才执行一次，因为函数永远执行不完，所以其内存作用域不会被释放；

#### Kotlin中的闭包


 lambda在Java和Kotlin中的区别；






