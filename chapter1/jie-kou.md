# 接口、抽象类

## 让类拥有接口特性的方式

 让类拥有接口的特性，一般的方式是通过实现该接口，然后在类中实现对应的接口方法，这种方式适合于自定义的类；
 但是，有时候，类的代码我们不能修改，任何让其也具备某些原先不具备的接口特性呢？
 
 答案是通过扩展方法，让方法返回值实现接口：
 
     //通过该方式，类无需实现Iterator接口，即可实现被迭代；
    fun ViewGroup.children() = object : Iterable<View> {
     override fun iterator() = object : Iterator<View> {
       var index = 0
       override fun hasNext() = index < childCount
       override fun next() = getChildAt(index++)
     }
    }
    
    
    val views = // ...
    
    for (view in views.children()) {
     // TODO do something with view
    }
    
    val visibleHeight = views.children()
     .filter { it.visibility == View.VISIBLE }
     .sumBy { it.measuredHeight }

 这种方式其实是对装饰模式的运用！


## 基本概念

1. 程序设计角度抽象类与接口的区别：抽象类是事物的半成品，接口是事物功能的一种描述；

        抽象类与接口的关系表述：能放音乐的Mp3
        能放音乐的，描述的是功能，这“能”就是通过接口来实现的；
        
        Mp3描述的是一个事物，但并不具体，只能看为半成品，因为还有多种类型，具体的Mp3，
        可以认为是Mp3是一个抽象类；  
        
        抽象类：表示带有部分属性的事物的半成品，抽象类是描述具体事物的；
        抽象类是具体的，所以其中的变量是有状态的；（部分具体，不能认为是完全具体）
        抽象类只能被单继承，因为具体实例只能有一个；
        
        接口：接口是用来表现功能的，带有修饰属性，接口不是具体事物，而是用来描述具体事物；
        接口是抽象的，其中的变量不能带有状态；
        接口可以被多实现，因为一个事物可以有多个功能，可以被多个特性描述；
    
2. 从程序设计角度，父类继承决定了子类是什么，接口实现则决定子类能够干什么，类中应该定义自身相关的属性方法，功能函数通过接口赋予


 ## 接口中定义变量和方法体
 
Kotlin中的接口是可以定义变量和方法体，但变量不可以初始化，也就是不能有状态，只能有定义（Java中的变量可以被初始化），在被继承时必须初始化接口变量，赋予状态，而方法表示的是实现过程，本身就没有状态（Java中不能有方法体）

## 为什么接口中的变量不能被初始化
为什么接口的定义的变量不能被初始化：
因为接口中的变量没有backing field，也就无法存储变量，所以只能做为一种声明；

 backing field + get、set 方法 等价于属性Property，这也是Kotlin中独有的；
 
 在Kotlin中只需声明一个变量，就相当于属性，backing field + get、set 方法由Kotlin自动生成；
 在Java中，这些都必须自己手动声明；

## 继承、接口多实现解决方法名冲突

当继承和实现操作中，方法出现了重复，在Java中由于接口方法没有方法体，所以不会有问题（只会有一个方法被实现！），而Kotlin中必须通过super<T>进行显式指定调用方（Kotlin和Java都必须保证签名返回值一致）

当一个类继承了多个接口，这些接口出现了两个完全一致的方法声明：

1. 在Java中，由于接口没有实现方法体，所以这种冲突不会有问题；

2. 在Kotlin中，接口中可以定义方法体，所以在冲突时必须通过super<T>显示的指定调用的是哪个接口的方法；
比如：

        interface n1{
            fun nn(): Int = 1
        }
        
        interface n2{
            fun nn(): Int = 2
        }
        
        abstract class n3{
            open fun nn(): Int = 3
        }
        
        class n0 : n1, n2, n3() {
            val i = 0
            override fun nn(): Int {
                if (i > 0) {
                    return super<n2>.nn()
                }else if (i < 0) {
                    return super<n3>.nn()
                }
                return super<n1>.nn()
            }
        }


  注意，无论是在Java还是Kotlin中，接口和类中如果出现了相同名称的方法，必须保证其签名（方法名，参数，返回值）一致，否则会报错，并且无法通过重载来解决的；
  
  
## 函数式接口：Single Abstract Method interfaces (SAM Interfaces)

lambda表达式对应的就是函数式接口 
所以首先来了解下函数式接口：functional interface 
functional interface 又被叫做：Single Abstract Method interfaces (SAM Interfaces)

只有一个抽象方法的接口(接口里的方法默认就是抽象的abstract)

备注：该接口还可以包含default方法，static方法及Object类的public方法。

