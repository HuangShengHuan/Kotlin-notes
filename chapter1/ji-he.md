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