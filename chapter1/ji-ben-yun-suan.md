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