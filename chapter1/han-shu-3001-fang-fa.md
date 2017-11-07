# 函数、方法

## 基本概念

方法即成员方法，定义在类中的跟类相关的，表示类的功能，特性等；

函数衍生自C语言（C语言本身没有类的概念），没有所谓类的归属，是一个独立的功能单位；


## 局部函数

Kotlin 支持局部函数，比如在一个函数包含另一函数。


    fun dfs(graph: Graph) {
      fun dfs(current: Vertex, visited: Set<Vertex>) {
        if (!visited.add(current)) return
        for (v in current.neighbors)
          dfs(v, visited)
      }
    
      dfs(graph.vertices[0], HashSet())
    }