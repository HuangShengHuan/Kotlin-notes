# 基本运算

## 位运算

Kotlin中的位运算不能使用特殊字符，比如 << 或 >>，只支持字符串缩写的形式，比如shl

* 这是完整的位运算列表（只用于 Int 和 Long）：

    shl(bits) – 有符号左移 (Java 的 <<)
    
    shr(bits) – 有符号右移 (Java 的 >>)
    
    ushr(bits) – 无符号右移 (Java 的 >>>)
    
    and(bits) – 位与
    
    or(bits) – 位或
    
    xor(bits) – 位异或
    
    inv() – 位非
    
    
## 判断相等 "=="与"==="
判断是否相等：==，比如数值相等;

判断是否是同一个变量，适用于对象：===（数字类型，比如Int不保留同一性，而String类型始终保留同一性）

** 同一性：=== **

    val a: Int = 10000
    print(a === a) // 输出“true”
    val boxedA: Int? = a
    val anotherBoxedA: Int? = a
    print(boxedA === anotherBoxedA) // ！！！输出“false”！！！


** 相等性：== **

    val a: Int = 10000
    print(a == a) // 输出“true”
    val boxedA: Int? = a
    val anotherBoxedA: Int? = a
    print(boxedA == anotherBoxedA) // 输出“true”
    

** String类型比较特殊： **

    val b: String = "a"
    println(b === b)      //true
    var boxb: String? = b
    val plus = boxb.plus("b")
    println(plus)
    var boxc: String? = b
    println(boxc)
    println(boxb === boxc) //true
    println(boxb == boxc) //无论是==还是===都是true