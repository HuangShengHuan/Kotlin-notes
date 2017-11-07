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
