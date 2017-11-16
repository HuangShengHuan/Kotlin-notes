# 属性代理

属性代理实际就是将属性的值的设置（set）和获取（get）的流程交给了其他的对象代理，相当于为原对象增加了一个backing field（理解为存储值的内存），变量的实际值被保存在代理对象中；


## var与val设置代理

对于var变量，要求代理必须实现getValue和setValue操作符方法，而val只需要实现getValue；

比如常用的lazy代理，只能用在val常量上：

```
    val lazyStr :String by lazy{
        "lazyStr"
    }
	
```

只能用来代理val常量，var变量则不行，因为其缺少setValue方法；

以下的代理，在val和var上都可以用：

```
    //在被使用前，如果没有先赋值，则会报IllegalStateException异常
    var nnv by Delegates.notNull<String>()

    //监测一个属性，如果其被进行赋值操作，则会回调下面的字面函数，打印log
    var obserV :Int by Delegates.observable(0){_,old, new ->
        println("obserV : $old -> $new")
    }

    //在属性要被进行赋值操作之前，回调下面的字面函数，如果返回FALSE，则属性不会被赋值
    var vetoableV:Int by Delegates.vetoable(0){_,old, new ->
        println("obserV : $old -> $new")
        true
    }

    //Delegate的observable和vetoable其实都是实现ObservableProperty，我们可以自己实现，两个
    //方法一起用
    var ovV : Int by object : ObservableProperty<Int>(0) {
        override fun beforeChange(property: KProperty<*>, oldValue: Int, newValue: Int): Boolean {
            return super.beforeChange(property, oldValue, newValue)
        }

        override fun afterChange(property: KProperty<*>, oldValue: Int, newValue: Int) {
            super.afterChange(property, oldValue, newValue)
        }
    }
```


## 使用Map、MutableMap做代理

查看MapAccessors.kt可以发现，其中扩展了Map的getValue和MutableMap的getValue、setValue方法：

```
@kotlin.internal.InlineOnly
public inline operator fun <V, V1: V> Map<in String, @Exact V>.getValue(thisRef: Any?, property: KProperty<*>): V1
        = @Suppress("UNCHECKED_CAST") (getOrImplicitDefault(property.name) as V1)

@kotlin.jvm.JvmName("getVar")
@kotlin.internal.InlineOnly
public inline operator fun <V> MutableMap<in String, in V>.getValue(thisRef: Any?, property: KProperty<*>): V
        = @Suppress("UNCHECKED_CAST") (getOrImplicitDefault(property.name) as V)

@kotlin.internal.InlineOnly
public inline operator fun <V> MutableMap<in String, in V>.setValue(thisRef: Any?, property: KProperty<*>, value: V) {
    this.put(property.name, value)
}
```

那么可以用Map来做代理，存储值：

```
    //定义不可变的Map，只有getValue
    val map = mapOf<String, String>("cat" to "fish", "monkey" to "banana")
    //将属性交给map代理
    val cat by map
    //调用getValue方法，map会根据属性的名字"cat"找到对应的value "fish"
    print(cat)
	
```

用可变的Map，向其中put键值对时，会先找是否有相同属性名的key，有则将值赋给这个属性：

```
    val relMap = HashMap<String, String?>()
    //交给relMap代理，如果向relMap中设置key为"first"、"second"的键值对，则
    //会自动将值赋给以下两个属性；
    val first by relMap
    val second by relMap

    relMap.put("first", "first value")
    relMap.put("second", "second value")

    println(first)
    println(second)
    
    //输出：
    //first value
    //second value
```

## 自定义代理

既然实现了getValue和setValue操作符就可以作为代理，那么我们也可以自定义代理；

可以直接实现这两个方法，也可以通过实现ReadOnlyProperty和ReadWriteProperty接口：

```
public interface ReadOnlyProperty<in R, out T> {
    /**
     * Returns the value of the property for the given object.
     * @param thisRef the object for which the value is requested.
     * @param property the metadata for the property.
     * @return the property value.
     */
    public operator fun getValue(thisRef: R, property: KProperty<*>): T
}

public interface ReadWriteProperty<in R, T> {
    /**
     * Returns the value of the property for the given object.
     * @param thisRef the object for which the value is requested.
     * @param property the metadata for the property.
     * @return the property value.
     */
    public operator fun getValue(thisRef: R, property: KProperty<*>): T

    /**
     * Sets the value of the property for the given object.
     * @param thisRef the object for which the value is requested.
     * @param property the metadata for the property.
     * @param value the value to set.
     */
    public operator fun setValue(thisRef: R, property: KProperty<*>, value: T)
}
```

此处就直接实现这两个方法：

```
//定义一个叫X的代理类
class X{
    var value: String? = null
	//实现getValue操作符
    operator fun getValue(thisRef: Any?, property: KProperty<*>): String {
        return value?:"hello"
    }
	//实现setValue操作符
    operator fun setValue(thisRef: Any?, property: KProperty<*>,value:String){
        this.value = value
    }
}

    val x: String by X()
    var x2: String by X()

    println(x)
    println(x2)

    //输出
    //hello x
    //hello x2
```

## 自定义File代理

属性代理让变量的设置和赋值过程能做更多的事，将变量的获取和设置流程封装到代理对象中，这样我们在使用对象的设置和赋值过程就能更加简洁，比如文件的读取和设值，直接通过访问变量就可以完成；

```
var file: File by FileDelegate()

class FileDelegate{
    var file: File? = null

    operator fun getValue(thisRef: Any?, property: KProperty<*>): File {
        if (file == null) {
            file = File("/root")
        }
        //在这里可以为file属性做更多的初始化条件
        return file!!
    }

    operator fun setValue(thisRef: Any?, property: KProperty<*>, value: File){
		//初始化...
        //设置更多的条件
        this.file = value
    }
}

使用：

    println(file.path)
    file = File("/root/help")
    println(file.path)
```

## 自定义SharedPreference 的代理

```
class Preference<T>(val context: Context, val name: String, val default: T, val prefName: String = "default") : ReadWriteProperty<Any?, T> {
    constructor(context: Context, default: T, prefName: String = "default"): this(context, "", default, prefName)
    val prefs by lazy { context.getSharedPreferences(prefName, Context.MODE_PRIVATE) }
    override fun getValue(thisRef: Any?, property: KProperty<*>): T {
        return findPreference(findProperName(property), default)
    }
    override fun setValue(thisRef: Any?, property: KProperty<*>, value: T) {
        putPreference(findProperName(property), value)
    }
    private fun findProperName(property: KProperty<*>) = if(name.isEmpty()) property.name else name
    private fun <U> findPreference(name: String, default: U): U = with(prefs) {
        val res: Any = when (default) {
            is Long -> getLong(name, default)
            is String -> getString(name, default)
            is Int -> getInt(name, default)
            is Boolean -> getBoolean(name, default)
            is Float -> getFloat(name, default)
            else -> throw IllegalArgumentException("Unsupported type")
        }
        res as U
    }
    private fun <U> putPreference(name: String, value: U) = with(prefs.edit()) {
        when (value) {
            is Long -> putLong(name, value)
            is String -> putString(name, value)
            is Int -> putInt(name, value)
            is Boolean -> putBoolean(name, value)
            is Float -> putFloat(name, value)
            else -> throw IllegalArgumentException("Unsupported type")
        }.apply()
    }
}

inline fun <reified R, T> R.pref(default: T) = Preference(AppContext, default, R::class.jvmName)
object Settings {
    var lastPage by pref(0)
}
```

（参考：[用 Map 为你的属性做代理](https://blog.kotliner.cn/2017/07/16/delegate-by-map/)）

## 和扩展属性配合使用

类的扩展属性是没有backing field的，并且需要实现set和get方法（对应setValue和getValue操作符），而属性代理刚好能够补充这两者，那么我们可以尝试将两者混合使用。

```
var String.nonull: Int by Delegates.notNull<Int>()

var String.obser: String by Delegates.observable("obser") { _, old, new ->
    println("obser had change $old -> $new")
}

```