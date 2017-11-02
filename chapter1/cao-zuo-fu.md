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