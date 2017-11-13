# 扩展属性

## Children：扩展ViewGroup获取ChildView

```
val ViewGroup.children: List
    get() = (0..childCount -1).map { getChildAt(it) }

parent.children.forEach { it.visible() }
```





