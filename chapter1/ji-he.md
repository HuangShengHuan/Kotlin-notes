# 集合

## in关键字遍历Map

  in关键字可以非常方便的遍历map：

比如：

 for ((k, v) in map) {
     println("$k -> $v")
 }

能这么写的原因是：在Maps类中，扩展了component操作符：
public inline operator fun <K, V> Map.Entry<K, V>.component1(): K = key

public inline operator fun <K, V> Map.Entry<K, V>.component2(): V = value


## in带索引的遍历集合

遍历集合，数组可以使用in关键字，当要在每次遍历时，获取当前遍历的索引，可以通过直接遍历集合的索引indices来实现：

    fun main(args: Array<String>) {
        //sampleStart
            val items = listOf("apple", "banana", "kiwi")
            for (index in items.indices) {
                println("item at $index is ${items[index]}")
            }
        //sampleEnd
    }
    
 或
 
    fun main(args: Array<String>) {
        //sampleStart
            val items = listOf("apple", "banana", "kiwi")
            var index = 0
            while (index < items.size) {
                println("item at $index is ${items[index]}")
                index++
            }
        //sampleEnd
    }

## mapOf、listOf定义只读集合

只读的list：

    val list = listOf("a", "b", "c")

只读的map：

    val map = mapOf("a" to 1, "b" to 2, "c" to 3)

## xxxMapOf、xxxListOf创建可读可写集合

在Kotlin中可以使用xxxMapOf，或xxxListOf来方便创建可变的集合和Map；
在Kotlin中访问map可以方便的使用数组下标的形式来进行访问；

    val map1 = hashMapOf<String, Int>("a" to 1, "b" to 2, "c" to 3)
    map1["a"] = 2

    val mutableMapOf = mutableMapOf("a" to 1, "b" to 2, "c" to 3)
    mutableMapOf["a"] = 2

    val sortedMapOf = sortedMapOf("a" to 1, "b" to 2, "c" to 3)
    sortedMapOf["a"] = 3

    val arrayListOf = arrayListOf(1, 2, 3)
    
    
 ## 迭代数组集合
 
 对于实现了Iterable接口的集合、数组可以使用以下方式进行遍历：
 
    for((index,value) in arrays.withIndex())
    
注意：此处之所以能够使用(i,v)的形式，是因为IndexValue是一个data 类；

或者：

    for(indexValues in arrays.withIndex()){
    	indexValues.index;
    	indexValues.value;
    }
    
### 另一种方式

 凡是实现了Iterable接口的类都可以使用forEach或者forEachIndexed进行遍历，其实就是对for循环遍历的一层包装：
 
     /**
     * Performs the given [action] on each element.
     */
    @kotlin.internal.HidesMembers
    public inline fun <T> Iterable<T>.forEach(action: (T) -> Unit): Unit {
        for (element in this) action(element)
    }
    
    /**
     * Performs the given [action] on each element, providing sequential index with the element.
     * @param [action] function that takes the index of an element and the element itself
     * and performs the desired action on the element.
     */
    public inline fun <T> Iterable<T>.forEachIndexed(action: (index: Int, T) -> Unit): Unit {
        var index = 0
        for (item in this) action(index++, item)
    }
    
    
    
### 扩展方法实现迭代遍历

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
    
