# 表达式

## try..catch..finally
在Kotlin中try..catch..finally也是表达式，我们可以通过该表达式实现在正常和异常情况下赋不同的值；

    var num = try {
        1
    } catch (ex: Exception) {
        2
    }finally {
        print("finally")
    }
    
    
## if表达式

### if表达式实现分条件为val常量赋值

  对于val类型的常量，在定义时就必须进行初始化，

  如果是需要分情况的来进行初始化，一般情况下我们只能使用var类型的变量，并且初始化一个默认值；

  比如：

    //开始时设置默认值，后面根据情况再变化
    var a = -1
    
    if(xxx){
     a=1
    }else{
     a =-1
    }

  这种情况下，我们无法使用val类型常量，
通过if表达式则可以解决该问题：

    val a = if(xxx) 1  else -1

## when表达式

when函数类似于Java的switch，但比Java的switch更加强大，when方法的参数支持多种类型，从变量到常量，再到表达式，方法都适用，并且when方法还支持返回值；

## 类似switch的使用方式

    when (HelloKotlin.compVar) {
        1-> print(1)
        2-> print(2)
        3-> print(4)
        4-> print(5)
    }
 
 根据情况执行，可能出现无匹配条件的情况；
 
 ## when带返回值
 
 when在带有返回值时，必须是条件的情况是限定的；
 即，无论when的参数是何值，都必须确保有返回值，这在参数类型种类已经确定的情况下，比如枚举，或者接口注解，  使用时无需else条件；
 
     val result = when (HelloKotlin.compVar) {
        1 -> 1
        2 -> 2
        3 -> 3
        4 -> 4
        else -> 0
    }



