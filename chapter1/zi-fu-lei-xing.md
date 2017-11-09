# 字符串
## 三引号修饰原始字符串

""" 这里输入原始字符串 """" 
三引号括起，中间能够输入原始字符串，即使是输空格也能够被识别，但不支持转义字符，能够使用字符串模板；

## Char

Kotlin的Char类型不支持直接表示为数字，可以通过toInt()来进行转换;


## 字符串模板

字符串模板除了能够使用普通变量还支持运算和调用其他函数,可以支持任意表达式，但是这些复杂额表达式必须使用${}：
//sampleStart
fun sum(a: Int, b: Int) = a + b
//sampleEnd

fun main(args: Array<String>) {
    println("sum of 19 and 23 is ${sum(19, 23)}")
}

