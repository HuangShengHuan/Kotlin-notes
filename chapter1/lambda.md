## lambda

 参考：[Lambda 表达式](http://blog.csdn.net/huyuchaoheaven/article/details/51085638)
 
 ## 函数式接口：Single Abstract Method interfaces (SAM Interfaces)

lambda表达式对应的是函数式接口 
所以首先来了解下函数式接口：functional interface 
functional interface 又被叫做：Single Abstract Method interfaces (SAM Interfaces)

只有一个抽象方法的接口(接口里的方法默认就是抽象的abstract)

备注：该接口还可以包含default方法，static方法及Object类的public方法。

 
 ## Kotlin的lambda约定
 
 不同于Java8的lambda，在与Java交互时，Kotlin对lambda做了一些简化的约定：
 
 1、如果函数的最后一个参数是函数类型，则可以把该参数放到括号外边：
 
    window.decorView.setOnTouchListener { v, event ->
        false
    }
 
 2、如果最后一个参数是函数类型，并且该函数只有一个参数，则传递过来的字面函数可以省略参数，用it代替：
 
    window.decorView.setOnClickListener { 
        //view 用it代替！
    }
 
 3、如果方法支持多个函数式接口作为参数，可以通过适配器函数来指定函数式接口的类型，即添加名称指定使用的接口：
 
 ```
     var run = Runnable{ }
     
     //此处通过增加名称，指定了是使用Runnable这个接口
     executor.execute(Runnable { println("This runs in a thread pool") })
 ```
 
 **以上对于函数式接口的简化仅适用于在Kotlin与Java交互的情况，即Kotlin中调用Java的函数式接口时，适用上面的简化，对于Kotlin自身并不适用，或者说并不需要，因为Kotlin有更好的替代方案：函数类型！**
 
 ## Kotlin的函数类型
 在Kotlin中，lambda与FunctionX**接口实例**、具名函数以及匿名函数相对应：
 
 比如：
 
 ```
 //具名函数
 fun funN(a:Int):Int = a  // 对应的引用(::funN)
 
 //匿名函数
 var funM: (Int) -> Int = fun(a) = a
 var funM2= fun(a: Int) = a
 
 //FuctionX接口实例
 var funI: (Int) -> Int = object : Function1<Int, Int> {
    override fun invoke(p1: Int): Int {
        TODO("not implemented") //To change body of created functions use File | Settings | File Templates.
    }
 }
 var funI2 = object :(Int) ->Int{
        override fun invoke(p1: Int): Int {
            TODO("not implemented") //To change body of created functions use File | Settings | File Templates.
        }
    }
    
 //Lambda
 var funL : (Int) -> Int ={a -> a}
 var funL2 = { a: Int -> a }
 
 ```
 
 注意： 此处的类型(Int) -> Int 对应 接口Function1<Int , Int>
 
 
 ## 适配器函数（Adapter Function）
 
 在Kotlin中，一般我们不需要定义函数式接口，因为完全可以使用函数类型来代替；但是，也有特殊情况，比如Iterable接口：
 
 ```
 public interface Iterable<out T> {
    /**
     * Returns an iterator over the elements of this object.
     */
    public operator fun iterator(): Iterator<T>
}
 
```
 
 由于Iterator也是接口，所以如果要实现Iterable接口，必须使用两层实现：
 
 ```
 fun testInterface2():Iterable<Long>{
    var count=0L;
    return  object :Iterable<Long>{
        override fun iterator(): Iterator<Long> =object : Iterator<Long>{
            override fun hasNext(): Boolean {
                TODO("not implemented") //To change body of created functions use File | Settings | File Templates.
            }

            override fun next(): Long {
                TODO("not implemented") //To change body of created functions use File | Settings | File Templates.
            }
        }
    }
}
 ```
 
 通过增加一个适配器函数，可以把第一层去掉：
 ```
 public inline fun <T> Iterable(crossinline iterator: () -> Iterator<T>): Iterable<T> = object : Iterable<T> {
    override fun iterator(): Iterator<T> = iterator()
}
 ```
 
 这样，可以以函数的形式创建Iterable：
 
 ```
     Iterable{
        object : LongIterator() {
            var count = 0
            override fun nextLong(): Long {
                count++
                return count
            }

            override fun hasNext(): Boolean = true
        }
    }
 ```