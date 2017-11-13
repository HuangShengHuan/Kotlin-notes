# 属性

## 禁止get或set方法

在Kotlin中，属性默认有get和set方法，并且由于变量和方法默认是public，所以可以直接设置或获取变量；

如果想禁止用户获取或设置变量，可以直接用private修饰get或set方法：

```
companion object {
    lateinit var instance: App
       private set
}
```







