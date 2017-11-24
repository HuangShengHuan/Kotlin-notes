# 操作符重载

## ComponentX：通过()从类实例中获取值

 在类中，可以重写多个ComponentX操作符方法，实现从类实例中抽取值，用来一次性为多个变量赋值；

```
 class ComponentX{
    operator fun component1():String{
        return "hh"
    }
    
    operator fun component2():Int{
        return 1
    }
}
```

使用：
```
    val (a, b) = ComponentX()
```

 其中，a对应component1，b对应component2，依此类推；
 
 可以只拿component1或component2：
 
 ```
    val (a) = ComponentX()
    
    //"_"是占位符，用于错过component1
    val (_ , a) = ComponentX()
```

 注意： 
1. data类默认已经为每个属性都重写了ComponentX操作符方法；
2. 实现了Iterable接口的集合和数组，都通过调用扩展withIndex方法，返回IndexedValue 类，该类为data类，所以在遍历时可以通过(k,v)的方式获取索引（key）和值；
 
 
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