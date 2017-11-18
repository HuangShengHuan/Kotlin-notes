# 高阶函数

## 替代回调监听

  在Java中，我们要对外暴露一个回调接口时，步骤很麻烦，需要先定义一个接口，然后
声明该接口的实例，添加set方法，在使用时还必须进行非空判断，再进行调用；

  在Kotlin中只需通过声明一个可为空的函数类型变量，在需要的地方，调用该变量的invoke
方法即可：

```
  var scrollListener: ((x: Int, y: Int) -> Unit)? = null

```

使用：

```
  scrollListener?.invoke(l, t)
```

两行代码即可搞定；使用时，赋给该变量匿名函数即可；

```
  scrollListener = {x,y -> println("xxx")}
  
```

> 函数类型实质就是FunctionX接口，比如上面(x: Int, y: Int) -> Unit对应Function2<Int,Int,Unit>接口，也就是说我们实质还是对外暴露了一个回调接口。这种方式在Java中也可以借鉴，比如在项目中引入了RxJava，那么可以发现其中包含了大量的Function接口（1.x），同样可以在Java中借鉴Kotlin的这种使用方式。