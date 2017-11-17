# 函数类型

 在Kotlin中，除了基本的数据类型和类外，还有另一种独有的类型，称为函数类型（Function type）；
 
 函数类型可以用来表示：具名函数，匿名函数，函数式接口，lambda表达式等；
 
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