# 扩展方法、函数

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
