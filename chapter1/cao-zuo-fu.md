# 操作符

## spread 操作符 (*)

Spread Operator（星号）用于展开数组，只能用来赋值给变长参数vararg，并且不能够被重载:

    fun main(args: Array<String>) {
        hello(0.5,1,2,3,4,name = "5")
        //or 或者这么写 * 号表示把参数展开
        var array = intArrayOf(1,2,3,5)
        hello(list = *array,name = "张三")
        //默认参数 类似 c
        hello(2.5,*array,name = "逗比")
    }
    
    fun hello(double: Double = 3.0 ,vararg list: Int, name: String) {
        list.forEach(::println)
    }
    
    
 ## is 类型判断
 
 is 运算符检测一个表达式是否某类型的一个实例。 
 如果一个不可变的局部变量或属性已经判断出为某类型，那么检测后的分支中可以直接当作该类型使用，无需显式转换：
 
 该操作符类似于Java 的instanceOf，不同的是，Java判断之后还得进行显式的类型转换。
 
    fun getStringLength(obj: Any): Int? {
        // `obj` 在 `&&` 右边自动转换成 `String` 类型
        if (obj is String && obj.length > 0) ｛
          return obj.length
        }
    
        return null
    }
