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

