# 关键字

## as ：类型转换/重命名

Kotlin中的as关键字不仅可以用来对类进行类型转换，还可以用来对相同类名不同包的类进行重命名；
对于在同一文件中引入相同类名的类情况，在Kotlin中通过as关键字可以对其进行重命名：

    import java.lang.AlerDialog as SimpleDialog
    import suport.v7.AlerDialog as FineDialog
    

## field

成员变量默认就有get和set方法，在这两个方法中通过field关键字表示当前要访问的变量；