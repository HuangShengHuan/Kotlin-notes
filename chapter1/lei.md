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
     
     
## object

### 匿名内部类

Kotlin的匿名内部类不同于Java，因为写的时候是以实现接口的形式：object ：xxxInterface，所以我们可以实现多个接口，还可以继承其他类：object：XXXInterface，YYYInterface （当然，如果是匿名内部类，实现多个接口没什么意义！）

    window.decorView.setOnClickListener(object:View.OnClickListener{
        override fun onClick(v: View?) {
        }
    })
 
 如果是函数式接口，可以通过SAM转换进行简化：
 
    window.decorView.setOnClickListener { 
            
    }  
     
 在Kotlin中，如果参数只有一个，可以省略，用it代理，此处用it代替View；    
     
### object作为对象表达式
 
 对象表达式，类似于初始化一个变量，这个变量在使用它的地方立即执行，其实就是匿名内部类

 作为参数：
 
     public interface Haha2 {
        void test1();
    
        int test2(int a);
     }
     
    fun testhah(haha2: Haha2) {}
  
    使用时：
    
    testhah(object : Haha2 {
        override fun test1() {

        }

        override fun test2(a: Int): Int {
            return 0
        }
    }) 
     
 作为返回值：
 
    fun ggg():Haha2 = object : Haha2 {
        override fun test2(a: Int): Int {
            TODO("not implemented") //To change body of created functions use File | Settings | File Templates.
        }

        override fun test1() {
        }
    } 
     
 **注意：**当object没有实现或继承任何其他类时，被认为是继承了Any（或者认为等价于Java的Object类），同样可以被作为方法、函数的返回值，也可以作为值赋给属性和变量；
 
 并且，此时的object被认为是一个Object类，其中声明的方法和属性都不能被调用：
 
     fun publicFoo() = object {
        val x: String = "x"
     }
    
     val xx=object {
        val x = "aa"
     }
     
 通过反编译为对应的Java代码：
 
 
    @NotNull
    private static final Object xx = new Object() {
      @NotNull
      private final String x = "aa";
    
      @NotNull
      public final String getX() {
         return this.x;
      }
    };
    
    @NotNull
    public static final Object publicFoo() {
      return new Object() {
         @NotNull
         private final String x = "x";
    
         @NotNull
         public final String getX() {
            return this.x;
         }
      };
    }    
     
 可以发现都被强制转换为Object类型了，也就是说其中定义的方法和属性都被强制擦除掉了。
 
 但是，当作为类的方法和属性，并且声明为private时，却是另一种情况：
 
     class objectTest {
    
        fun main(args: Array<String>) {
            xx.x
            
            publicFoo().x
        }
    
        private fun publicFoo() = object {
            val x: String = "x"
        }
    
        private val xx = object {
            val x = "aa"
        }
    
    }    
     
  对应的Java代码：
  
     public final class objectTest {
       private final <undefinedtype> xx = new Object() {
          @NotNull
          private final String x = "aa";
    
          @NotNull
          public final String getX() {
             return this.x;
          }
       };
    
       public final void main(@NotNull String[] args) {
          Intrinsics.checkParameterIsNotNull(args, "args");
          this.xx.getX();
          this.publicFoo().getX();
       }
    
       private final <undefinedtype> publicFoo() {
          return (<undefinedtype>)(new Object() {
             @NotNull
             private final String x = "x";
    
             @NotNull
             public final String getX() {
                return this.x;
             }
          });
       }
     }   
 
 可以发现此时不再强转为Object类型，而是“不确定类型”undefinedtype，所以其中的属性和方法可以被使用：
 
    fun main(args: Array<String>) {
        xx.x
        publicFoo().x
    }    
     
## object作为对象声明

对象声明效果类似于Java中声明静态类和静态变量，但其实际原理是利用单例模式中的饿汉模式：

    object ff {
        val a = 1
        fun mm(){
        }
    }

 对应的Java代码：

    public final class ff {
       private static final int a = 1;
       public static final ff INSTANCE;
    
       public final int getA() {
          return a;
       }
    
       public final void mm() {
       }
    
       private ff() {
          INSTANCE = (ff)this;
          a = 1;
       }
    
       static {
          new ff();
       }
    }       

伴生对象也是：
             
    class objectTest {
    
        companion object{
            val aa = 1
    
            fun myfun(){}
    
            var cc:String? = "ii"
        }
    
    }                
                         
对应的Java代码：

    public final class objectTest {
       private static final int aa = 1;
       @Nullable
       private static String cc = "ii";
       public static final objectTest.Companion Companion = new objectTest.Companion((DefaultConstructorMarker)null);
       
       public static final class Companion {
          public final int getAa() {
             return objectTest.aa;
          }
    
          public final void myfun() {
          }
    
          @Nullable
          public final String getCc() {
             return objectTest.cc;
          }
    
          public final void setCc(@Nullable String <set-?>) {
             objectTest.cc = <set-?>;
          }
    
          private Companion() {
          }
    
          // $FF: synthetic method
          public Companion(DefaultConstructorMarker $constructor_marker) {
             this();
          }
       }
    }
                                  
                                   
                                        
                                             
                                                  
                                                       
                                                            
                                                                 
                                                                      
                                                                           
                                                                                     
     
     