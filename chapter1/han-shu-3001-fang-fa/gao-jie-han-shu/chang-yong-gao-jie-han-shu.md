# 常用高阶函数

## apply

```
public inline fun <T> T.apply(block: T.() -> Unit): T { block(); return this }
```

apply传入的参数是调用者的扩展方法，而扩展方法中是可以直接使用调用者的公有方法和公有属性的；

比如：我们直接使用了list的size方法；

```
    val list = listOf(1, 2, 3)
    list.apply {
        print(size)
    }
```

## let

```
public inline fun <T, R> T.let(block: (T) -> R): R = block(this)
```
与apply类似，同样是调用者的扩展方法，不同在于：
1、参数是普通方法，并且将调用者作为参数，而apply是调用者的扩展方法，没有参数；
2、返回值可以是任意，而apply返回值必须是自身；

> 类似的还有run、with、also、takeIf、takeUnless、repeat；在Standard.kt中定义。

## use

use与let操作类似，use可以用来操作可关闭的实例，比如数据流，文件流等，use要求调用实例必须实现了closeable接口，结束之后会自动关闭。
```
public inline fun <T : Closeable?, R> T.use(block: (T) -> R): R {
    var closed = false
    try {
        return block(this)
    } catch (e: Exception) {
        closed = true
        try {
            this?.close()
        } catch (closeException: Exception) {
        }
        throw e
    } finally {
        if (!closed) {
            this?.close()
        }
    }
}
```

## map

map，与RxJava中的map功能一样，相当于将原来的集合元素替换成新的元素
```
public inline fun <T, R> Iterable<T>.map(transform: (T) -> R): List<R> {
    return mapTo(ArrayList<R>(collectionSizeOrDefault(10)), transform)
}

internal fun <T> Iterable<T>.collectionSizeOrDefault(default: Int): Int = if (this is Collection<*>) this.size else default
```

## takeWhile

takeWhile与filter类似，当遍历到第一个不符合条件的数据时，停止遍历，只返回之前符合条件的数据;
```
public inline fun <T> Iterable<T>.takeWhile(predicate: (T) -> Boolean): List<T> {
    val list = ArrayList<T>()
    for (item in this) {
        if (!predicate(item))
            break
        list.add(item)
    }
    return list
}
```

## flatMap

遍历集合的每一个元素，并根据每一个元素，生成新的集合，最终将这些所有的集合汇总到一个总的集合，并返回；
```
public inline fun <R> CharArray.flatMap(transform: (Char) -> Iterable<R>): List<R> {
    return flatMapTo(ArrayList<R>(), transform)
}

/**
 * Appends all elements yielded from results of [transform] function being invoked on each element of original array, to the given [destination].
 */
public inline fun <T, R, C : MutableCollection<in R>> Array<out T>.flatMapTo(destination: C, transform: (T) -> Iterable<R>): C {
    for (element in this) {
        val list = transform(element)
	//每一个元素对应的集合最终又被放到一个总的集合中
	//如果原先是二维的集合，相当于将每个集合中的元素都放到一个集合中，变为一维
        destination.addAll(list)
    }
    return destination
}
```

  从flatMap的源码可以看出，当调用者是一维的集合时，flatMap对于每个遍历的元素都要求生成一个新的集合，而这每个新的集合最终又会被汇合到一个总的集合中，不同于RxJava的flatMap，Kotlin的flatMap添加的元素是有序的,相当于concatMap；
  
## fold

fold可以用来对集合中的所有数据进行统计操作，类似于reduce，都可以做累计操作，fold可以设置初始值，并且不限制返回的数据类型；
```
public inline fun <T, R> Iterable<T>.fold(initial: R, operation: (acc: R, T) -> R): R {
    var accumulator = initial
    for (element in this) accumulator = operation(accumulator, element)
    return accumulator
}
```
fold不同于reduce，其不限制返回的数据类型，所以我们可以返回任意的数据类型；
使用fold进行字符串拼接：
```
list.fold(StringBuilder()){ acc, i ->
    acc.append(i)
}
```

## reduce

reduce适合用来做集合的累积、累加操作：
```
public inline fun <S, T: S> Array<out T>.reduce(operation: (acc: S, T) -> S): S {
    if (isEmpty())
        throw UnsupportedOperationException("Empty array can't be reduced.")
    var accumulator: S = this[0]
    for (index in 1..lastIndex) {
        accumulator = operation(accumulator, this[index])
    }
    return accumulator
}
```
  从返回的数据类型可以看出，reduce返回的数据类型S只能是T的父类，也就是说，如果是操作的数据
是Int，只能是Int，Number或Any类型的返回值；(逆变)

## joinTo

joinTo能够方便的连接集合中的所有元素，可以设置前缀prefix，后缀postfix，连接符separator，如果设置了limit则可以设置省略符truncated，并且可以为每个元素做函数操作转换transform；

```
public fun <T, A : Appendable> Iterable<T>.joinTo(buffer: A, separator: CharSequence = ", ", prefix: CharSequence = "", postfix: CharSequence = "", limit: Int = -1, truncated: CharSequence = "...", transform: ((T) -> CharSequence)? = null): A {
    buffer.append(prefix)
    var count = 0
    for (element in this) {
        if (++count > 1) buffer.append(separator)
        if (limit < 0 || count <= limit) {
            buffer.appendElement(element, transform)
        } else break
    }
    if (limit >= 0 && count > limit) buffer.append(truncated)
    buffer.append(postfix)
    return buffer
}

internal fun <T> Appendable.appendElement(element: T, transform: ((T) -> CharSequence)?) {
    when {
        transform != null -> append(transform(element))
        element is CharSequence? -> append(element)
        element is Char -> append(element)
        else -> append(element.toString())
    }
}
```
