## lambda

 参考：[Lambda 表达式](http://blog.csdn.net/huyuchaoheaven/article/details/51085638)
 
 ## 函数式接口：Single Abstract Method interfaces (SAM Interfaces)

lambda表达式对应的就是函数式接口 
所以首先来了解下函数式接口：functional interface 
functional interface 又被叫做：Single Abstract Method interfaces (SAM Interfaces)

只有一个抽象方法的接口(接口里的方法默认就是抽象的abstract)

备注：该接口还可以包含default方法，static方法及Object类的public方法。

 
 ## Kotlin的lambda约定
 
 不同于Java8的lambda，Kotlin的lambda中做了一些简化的约定：
 
 1、如果函数的最后一个参数是函数类型，则可以把该参数放到括号外边：
 
    window.decorView.setOnTouchListener { v, event ->
        false
    }
 
 2、如果最后一个参数是函数类型，并且该函数只有一个参数，则传递过来的字面函数可以省略参数，用it代替：
 
    window.decorView.setOnClickListener { 
        //view 用it代替！
    }
 
 