# var、val

## 基本概念

 var ： variable，表示Kotlin中的变量；
 
 val ： value，表示Kotlin中的常量；
 
## val

val用来定义Kotlin中的常量，但是val并不完全等价于Java中的Final，需要添加const关键字，才会完全变为常量；

  在Kotlin中，val并不能完全等价于Java中的Final常量，因为val常量并不是编译器就确定的，而是在运行期才确定的常量，所以，通过反射，我们仍然能够改变val的值；

  在Kotlin中要定义类似Java的Final常量，可以通过const关键字修饰：

const val aa = "hahh"

## 清晰的表示数字常量

表示数字常量时，如果数字很长，可以用下划线“_”来进行分割，这样更容易阅读，比如：

1111111111=1_111_111_111，下划线的位置是任意的！