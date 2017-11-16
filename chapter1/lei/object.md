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
     
### object作为对象声明

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
                                 
## 实现与Java的静态互访

Kotlin中的伴生对象companion实际上也是一个普通类，其中定义的方法和变量如果想要被Java访问，Java比如通过Companion实例才能进行，如果想要于Java互访问，方法需要变为静态，使用@JvmStatic注解，静态变量通过@JvmField注解;

**注意：**在Kotlin中，不提倡使用静态方法，当一个变量或者函数不需要依赖一个具体的类时，应该使用包级函数和包级变量；