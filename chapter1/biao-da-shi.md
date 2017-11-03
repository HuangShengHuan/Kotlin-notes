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