# 扩展属性

## 扩展属性的原理

类的扩展属性原理其实与扩展方法是一样的，只是定义的形式不同，扩展属性必须定义get和set方法，
并且类似于接口中定义的变量，没有backingfield，即没有field关键字，不能用来存储变量。(一般的类属性，在其对象实例中都会分配一点内存来存储属性的值。)

```
fun main(args: Array<String>) {
    val str = "aa"
    //没有backing field，不能存储值，其实际是通过setXXX(str，10)操作str
	//输出：aa10
    str.s = 10
	
	//输出：2
    println(str.s)
}

var String.s: Int
    get() = this.length
    set(value){
        //set方法并没有field可以用来存储value，
        //其实际作用是使用通过value来操作调用者，即this
        println(this.plus(value))
    }
	
```

对应的Java代码：

```
public final class ExtendsKt {
   public static final void main(@NotNull String[] args) {
      Intrinsics.checkParameterIsNotNull(args, "args");
      String str = "aa";
      setS(str, 10);
      int var2 = getS(str);
      System.out.println(var2);
   }

   public static final int getS(@NotNull String $receiver) {
      Intrinsics.checkParameterIsNotNull($receiver, "$receiver");
      return $receiver.length();
   }

   public static final void setS(@NotNull String $receiver, int value) {
      Intrinsics.checkParameterIsNotNull($receiver, "$receiver");
      String var2 = $receiver + value;
      System.out.println(var2);
   }
}

```

可以看出，为什么扩展属性会没有backing field，其实际仍然是工具方法，并不是在原类内部扩展。

## Children：扩展ViewGroup获取ChildView

```
val ViewGroup.children: List
    get() = (0..childCount -1).map { getChildAt(it) }

parent.children.forEach { it.visible() }
```





