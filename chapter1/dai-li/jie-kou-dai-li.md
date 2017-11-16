# 接口代理

 在实现接口时，我们可以通过by代理，将另一个实现接口的类的实例作为当前的接口实现，也就是把另一个接口实现类作为默认的实现；

```
class MyListener : Transition.TransitionListener by EmptyTransitionListener {
    override fun onTransitionStart(transition: Transition) {
    }
}

object EmptyTransitionListener : Transition.TransitionListener {
    override fun onTransitionEnd(transition: Transition) {}
    override fun onTransitionResume(transition: Transition) {}
    override fun onTransitionPause(transition: Transition) {}
    override fun onTransitionCancel(transition: Transition) {}
    override fun onTransitionStart(transition: Transition) {}
}

//也可以直接使用class，但是作为代理时就必须创建class的实例；
class MyListener : Transition.TransitionListener by EmptyTransitionListener() {
    override fun onTransitionStart(transition: Transition) {
    }
}

class EmptyTransitionListener : Transition.TransitionListener {
    override fun onTransitionEnd(transition: Transition) {}
    override fun onTransitionResume(transition: Transition) {}
    override fun onTransitionPause(transition: Transition) {}
    override fun onTransitionCancel(transition: Transition) {}
    override fun onTransitionStart(transition: Transition) {}
}
```

## 在代理模式中的运用

  在使用代理模式时，我们将接口实例注入到类的构造方法中，在Kotlin中通过使用by代理，我们可以不用
在类中实现对应类的方法：

比如：
一般的构造注入方式：

```
interface C{
    fun cc()
}


//在构造方法中，我们必须实现C的方法cc
class BiBi(val c:C):C{
    override fun cc() {
        c.cc()
    }
}

```

通过by代理实现，我们调用方法cc会直接指向到接口实例中

```
class BiBi(c: C):C by c

```