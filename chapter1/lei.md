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

Kotlin中的枚举类实际就是普通类中封装了一个companion object，其中包含类多个本类得到实例，用来表现枚举

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

 **与enum的对比：**
     
1. enum适合用来表示状态，因为状态无论何时，全局只有一个，而sealed适合用来表示类似指令，这种全局同一时间可能存在多个的情况。
2. sealed类似于枚举，其规定了子类的种类的限制，当子类可以有多个实例时，定义为class，否则定义为object（enum：实例可数，sealed：子类可数）。

3. sealed的子类当不需要保存状态时，即所有的实例都保有相同的状态时用object，如果同一种子类需要有多种状态，则用class。

## sealed定义子类种类的限制

enum定义的时候，声明了enum类自身的多个静态实例，用来表示多种状态，每种状态只能有一个实例，由于每个实例都是枚举自身，所以其构造参数一致；

sealed定义了子类种类的限制，不同于enum，其中并没有子类的实例，而只是子类的声明，子类可以自定义构造方法参数列表，并且子类**可以根据自身情况，决定要存在一个实例（object），还是多个实例（class）.**
比如：播放音乐
```
sealed class PlayerCmd
//如果该类需要定义多个实例，则声明为class
class Play(val url: String, val position: Long = 0): PlayerCmd()

class Seek(val position: Long): PlayerCmd()

//如果该类只能定义一个实例，则声明为object
object Pause: PlayerCmd()

object Resume: PlayerCmd()

object Stop : PlayerCmd()
```
  我们需要新的播放状态时，我们只需要new一个新的Play实例，传进来即可，对于停止stop操作，是全局的操作，我们只需要一个实例.
  
  
   
     

   
 ## inner 内部类
 
### 静态内部类与非静态内部类

内部类持有外部类的引用，可以在内部类中直接访问外部类的成员变量，如果要初始化内部类成员，必须通过外部类的引用实例进行初始化；

静态内部类除了必须使用外部类名+.引起，其他基本与外部类没太大区别；

```
public class OutterClass {
    private int a = 0;

    public class InnerClass{
        private void hh(){
            //可以直接获取到外部类的成员变量，因为内部类拥有外部类的引用
            System.out.println(a);
        }
    }

    public static class InnerClassStatic{
        private void hh(){
            //静态内部类与类实例本身没有任何关系
            //只与类相关
        }
    }

    public static void main(String ... args){
        //要初始化内部类，必须通过外部类的实例才行
        OutterClass outterClass = new OutterClass();
        //内部类相当于外部类实例的成员属性
        InnerClass innerClass = outterClass.new InnerClass();

        InnerClassStatic innerClassStatic = new InnerClassStatic();
    }
}

```

### Kotlin与Java的区别

1. 在Java中在类内部定义类默认是内部类，要静态则必须使用static进行修饰，而Kotlin中，
在类内部定义的类默认是静态类，如果要声明为内部类，必须通过inner进行修饰;
2. 在Java中，内部类方法调用外部类的变量时，需要通过外部类的引用来调用，而在Kotlin中，则是通过标签的方式来调用；

比如：

```
class OutterClassKt{
    val a: Int = 0
    
    inner class innerClass{
        val a: Int = 5
        fun test(){
            //要访问外部类，通过@Outter指定
            println(this@OutterClassKt.a)
        }
    }
}
```                                                  
                                        
3.在Java的方法中，声明匿名内部类，内部类的方法如果要访问外部类方法的局部变量，该变量必须声明为final；
   在Kotlin中，上述情况，内部类可以直接访问局部变量，无需声明为final，因为该声明操作Kotlin已经帮我们处理了；

比如：
```
    inner class innerClass{
        fun innerTest(){
            var mm1 = 1
            var mm2 = "dddd"
            var bibib = object : View.OnClickListener{
                override fun onClick(v: View?) {
                    print(mm1)
                    print(mm2)
                }
            }
        }
    }

   // 对应的Java代码：

   public final class innerClass {
      public final void innerTest() {
         final IntRef mm1 = new IntRef();
         mm1.element = 1;
         final ObjectRef mm2 = new ObjectRef();
         mm2.element = "dddd";
         OnClickListener var10000 = new OnClickListener() {
            public void onClick(@Nullable View v) {
               int var2 = mm1.element;
               System.out.print(var2);
               String var3 = (String)mm2.element;
               System.out.print(var3);
            }
         };
         mm2 = null;
         mm1 = null;
      }
   }

```
可以发现，多了一个叫ObjectRef 的东西，其为final，element属性为我们声明的变量值；

                                            
                                                                                                                               
                                                  
                                                       
                                                            
                                                                 
                                                                      
                                                                           
                                                                                     
     
     