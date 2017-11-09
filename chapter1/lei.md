# 类

## open
 在Kotlin中，类默认是final，即不可继承，通过添加open关键字可以声明为可继承的；

## 类属性 Property

 Kotlin的类属性相当于Java中的成员变量+get+set方法

 成员变量默认就有get和set方法，在这两个方法中通过field关键字表示当前要访问的变量；

## 构造方法参数添加var于不添加的区别

在类的构造方法中，val和var修饰的参数变量为成员属性变量，而没有这两者修饰的表示普通的方法参数；

 两者的区别在于：
 val，var修饰的变量可以用于类的全局，而不用这两者修饰的变量只能用于构造函数中；

比如：
  class Delegete(var c: Int, b: Boolean) 

需要注意的是：二级构造函数是不支持var和val修饰的变量；
    constructor(c: Int, d: String) : this(c, false) {}
  
## data类

### data类的局限性

data class 存在的问题：不符合JavaBean的规范；

1、是Final的不能被继承；
2、没有默认的构造方法；

 这两种情况导致在于某些第三方框架混合使用时，会出现兼容性问题，比如Gson；

解决方案是通过插件注解反射；

### data类的componentX操作符
  
data类为其每一个属性自动生成了componentX操作符方法，通过这些方法支持直接通过data类对象
将其成员属性的值直接赋给多个变量：

比如：

    data class H(val id:Int,val name:String)
    
    val h = H(1,"ww")
    
    //可以直接将h的成员属性赋给多个变量
    val (id,name) = h
  
    //集合遍历中通过withIndex方法返回data类型的IndexedValue类
    for((index,value) in arrays.withIndex())

## enum类

     enum class MyName(val i: Int){
         //枚举其实相当于普通类定义了一个companion object
         //并且在Object中声明了多个本类的示例
         JIM(0),JAN(1), MIKE(2);
     
         fun test(){
             JAN.test()
         }
     }
     
 
 等价于：
 
     class TestEnum(val i: Int){
        companion object{
            val JIM: TestEnum = TestEnum(0)
            val JAN: TestEnum = TestEnum(1)
            val MIKE: TestEnum = TestEnum(2)
        }
    
        fun test(){
            JAN.test()
        }
    }
    
  我们可以把enum当做companion object使用：
    
     fun main(args: Array<String>) {
        //获取到枚举变量对应的常量的值
        MyName.JAN.ordinal
    
        //获取所有的枚举变量
        MyName.values()
    
        //获取指定的枚举变量
        MyName.valueOf("JIM")
    }
     
_使用枚举开销较大，因为枚举类中实际包含了多个本类的对象实例；_

     
## sealed类
     
enum适合用来表示状态，因为状态无论何时，全局只有一个，而sealed适合用来表示类似指令，这种全局同一时间可能存在多个的情况。
     
     
## object匿名内部类

Kotlin的匿名内部类不同于Java，因为写的时候是以实现接口的形式：object ：xxxInterface，所以我们可以实现多个接口，还可以继承其他类：object：XXXInterface，YYYInterface （当然，如果是匿名内部类，实现多个接口没什么意义！）

    window.decorView.setOnClickListener(object:View.OnClickListener{
        override fun onClick(v: View?) {
        }
    })
 
 如果是函数式接口，可以通过SAM转换进行简化：
 
    window.decorView.setOnClickListener { 
            
    }  
     
 在Kotlin中，如果参数只有一个，可以省略，用it代理，此处用it代替View；    
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     