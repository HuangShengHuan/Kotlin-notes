# 类

## 基本概念
 在Kotlin中，类默认是final，即不可继承，通过添加open关键字可以声明为可继承的；

## 类属性 Property

 Kotlin的类属性相当于Java中的成员变量+get+set方法

 成员变量默认就有get和set方法，在这两个方法中通过field关键字表示当前要访问的变量；

## 构造方法参数添加var于不添加的区别

在类的构造方法中，val和var修饰的参数变量为成员属性变量，而没有这两者修饰的表示普通的方法参数；

 两者的区别在于：
 val，var修饰的变量可以用于类的全局，而不用这两者修饰的变量只能用于构造函数中；

比如：
  class Delegete(var c: Int, b: Boolean) 

需要注意的是：二级构造函数是不支持var和val修饰的变量；
    constructor(c: Int, d: String) : this(c, false) {}
  