# 函数

## FunctionX

 Kotlin中存在类似Rx的函数式接口，Function0..Function22，这些接口在lambda中称为函数式接口（SAM），可以用来表示所有函数类型，最简单的接口为：
 
 ```
 public interface Function0<out R> : Function<R> {
    /** Invokes the function. */
    public operator fun invoke(): R
}
 ```
 
 对应的函数类型为：() -> Unit
 

## 函数类型

 在Kotlin中，除了基本的数据类型和类外，还有另一种独有的类型，称为函数类型（Function type）；
 
 **函数类型对应Kotlin中23个FunctionX接口，是这些接口的另一种表现形式，类似于操作符与操作符函数之间的对应关系，比如() -> Unit用来表示Function0<Unit>;**

 **函数类型的本质是FunctionX接口的实现类实例！**
 
 函数类型可以被：具名函数，匿名函数，函数式接口，lambda表达式等形式赋值；
 
  最简单的函数类型为：()->Unit 表示一个既没有参数，也没有返回值的函数；
 
 ```
//具名函数赋值，需要使用函数引用
val bbFun: (Int) -> Int = ::h


//匿名函数赋值
val bFun : (Int)->Int= fun(a: Int) = a



//lambda赋值
val bLambda: (Int) -> Int = { a -> a }



//函数式接口赋值
val bInterface : (Int)->Int =object :Function1<Int,Int>{
    override fun invoke(p1: Int): Int {
        TODO("not implemented") //To change body of created functions use File | Settings | File Templates.
    }
}

val bInterface2 :(Int)->Int= object :(Int)->Int{
    override fun invoke(p1: Int): Int {
        TODO("not implemented") //To change body of created functions use File | Settings | File Templates.
    }
}
 
 ```
 
 ### 函数类型作为返回值
 
 既然函数类型的本质是FunctionX接口，那么我们也可以用接口实例作为返回值使用，形成一种嵌套的效果：
 
 ```
 fun testClosure(x: Int): (Int) -> (Int) -> Int {
    return fun(y: Int): (Int) -> Int {
        return fun(z: Int): Int {
            return x + y + z
        }
    }
}

//通过简化为：
fun testClosure(x: Int) = fun(y: Int) = fun(z: Int) = x + y + z

```

