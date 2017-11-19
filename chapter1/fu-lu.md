# 附录


> ？定义可为空的变量或参数，？：空情况下的默认值，？.let{}指定在变量不为null时执行的代码;


### 测量代码块的运行时间

```
public inline fun measureTimeMillis(block: () -> Unit) : Long {
    val start = System.currentTimeMillis()
    block()
    return System.currentTimeMillis() - start
}

/**
 * Executes the given block and returns elapsed time in nanoseconds.
 */
public inline fun measureNanoTime(block: () -> Unit) : Long {
    val start = System.nanoTime()
    block()
    return System.nanoTime() - start
}
```

### 验证逻辑与非空

```
// throws IllegalArgumentException
require(value: Boolean)
require(value: Boolean, lazyMessage: () -> Any)
requireNotNull(value: T?): T
requireNotNull(value: T?, lazyMessage: () -> Any): T
// throws IllegalStateException
check(value: Boolean)
check(value: Boolean, lazyMessage: () -> Any)
checkNotNull(value: T?): T
checkNotNull(value: T?, lazyMessage: () -> Any): T
// throws AssertionError
assert(value: Boolean)
assert(value: Boolean, lazyMessage: () -> Any)
```