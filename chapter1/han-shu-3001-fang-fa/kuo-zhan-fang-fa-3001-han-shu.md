# 扩展方法、函数

## 扩展方法的原理

  Kotlin 中类的扩展方法并不是在原类的内部进行拓展，通过反编译为Java代码，可以发现，其原理是使用装饰模式，对源类实例的操作和包装，其实际相当于我们在Java中定义的工具类方法，并且该工具类方法是使用调用者为第一个参数
的，然后在工具方法中操作该调用者；

该调用者在Kotlin中使用this关键字表示；

比如：定义一个String的操作符函数，其中的this表示调用者本身；

```
fun String.times(t:Int){
    val sb = StringBuilder()
    for (i in 0 until t) {
        sb.append(this)
    }
    println(sb.toString())
}
```

Kotlin中的调用方式： "aaaa".times(10)

反编译为对应的Java代码：

```
public final class TestObjectKt {
   public static final void times(@NotNull String $receiver, int t) {
      Intrinsics.checkParameterIsNotNull($receiver, "$receiver");
      StringBuilder sb = new StringBuilder();
      IntRange var10000 = RangesKt.until(0, t);
      int i = var10000.getFirst();
      int var4 = var10000.getLast();
      if(i <= var4) {
         while(true) {
            sb.append($receiver);
            if(i == var4) {
               break;
            }

            ++i;
         }
      }

      String var5 = sb.toString();
      System.out.println(var5);
   }
}
```

Java中的调用方式： TestObjectKt.times("aaaa",10);

可见Kotlin中实际是将调用者"aaaa"作为方法times的第一个参数；

**扩展方法将原先要用：方法(类实例) 方式调用方法转为了：类实例.方法()**


## 扩展方法实现迭代遍历

 实现了Iterable接口的类，可以使用for循环进行遍历；
 
 通过扩展方法，类无需实现Iterator接口，即可实现被迭代；
 
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


如果想让类拥有类的某些接口特性，但又不想通过让类实现该接口的方式来实现，可以通过扩展方法，并设置接口作为扩展方法的返回值，并且可以通过object匿名类来简化。

## 通过操作符扩展类的方法、属性

在给类定义扩展方法时除了自定义的方法，还可扩展类的操作符方法，这样可以直接对类实例使用操作符进行操作:

```
operator fun ViewGroup.get(index: Int): View? = getChildAt(index)
operator fun ViewGroup.minusAssign(child: View) = removeView(child)
operator fun ViewGroup.plusAssign(child: View) = addView(child)
operator fun ViewGroup.contains(child: View) = indexOfChild(child) != -1
val ViewGroup.size: Int
 get() = childCount


val views = // ...
val first = views[0]
views -= first
views += first
if (first in views) doSomething()
Log.d("MainActivity", "View count: ${views.size}")
```