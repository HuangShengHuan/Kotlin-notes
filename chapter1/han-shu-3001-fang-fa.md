# 函数、方法

## 基本概念

方法即成员方法，定义在类中的跟类相关的，表示类的功能，特性等；

函数衍生自C语言（C语言本身没有类的概念），没有所谓类的归属，是一个独立的功能单位；


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