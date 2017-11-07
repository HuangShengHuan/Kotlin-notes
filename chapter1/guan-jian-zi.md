# 关键字

## as ：类型转换/重命名

Kotlin中的as关键字不仅可以用来对类进行类型转换，还可以用来对相同类名不同包的类进行重命名；
对于在同一文件中引入相同类名的类情况，在Kotlin中通过as关键字可以对其进行重命名：

    import java.lang.AlerDialog as SimpleDialog
    import suport.v7.AlerDialog as FineDialog
    

## field

成员变量默认就有get和set方法，在这两个方法中通过field关键字表示当前要访问的变量；


## 解决关键字冲突

当定义的变量名与默认的关键字存在冲突，可以通过：

    
    `关键字`的形式来定义变量名，比如可以定义：

    `class`作为变量名，不会与关键字class冲突！
    
## internal：module内可见

类可见性修饰：internal，表示module内可见；

与Java存在兼容性问题，Kotlin中没有Java中的default修饰；

## typealias为类型声明别名

    //为函数类型：(Double) -> Double 声明别名为Discount
    typealias Discount = (Double) -> Double
    
    
## in

迭代集合：

    fun main(args: Array<String>) {
        val items = listOf("apple", "banana", "kiwi")
        //sampleStart
        for (item in items) {
            println(item)
        }
        //sampleEnd
    }
    
迭代区间：

    for (x in 1..5) {
        print(x)
    }
    

判断是否在集合：

    fun main(args: Array<String>) {
        val items = setOf("apple", "banana", "kiwi")
    //sampleStart
        when {
            "orange" in items -> println("juicy")
            "apple" in items -> println("apple is fine too")
        }
    //sampleEnd
    }