# Kotlin必知点

[基本数据类型](/chapter1/ji-ben-shu-ju-lei-xing.md)

## 字符串
### 三引号修饰原始字符串

""" 这里输入原始字符串 """" 
三引号括起，中间能够输入原始字符串，即使是输空格也能够被识别，但不支持转义字符，能够使用字符串模板；

### Char

Kotlin的Char类型不支持直接表示为数字，可以通过toInt()来进行转换;


## 数组

### range
 IntRange，LongRange等表示数值区间，0..10表示闭区间[0,10]，0 until 10表示左闭右开区间[0,10)
 
 在Kotlin中，区间可以使用变量来表示：

 val intRange :IntRange =0 ..10

 val intRange2 : IntRange = 0 until 10

 **无效的区间：** 0 .. -1 = [0,-1]
 
 通过转换为Java代码可以发现，range在被遍历时会判断初值是否小于末值，小于才会开始遍历：
 
      IntRange intRange3 = new IntRange(1, -1);
      System.out.println(intRange3);
      int i = intRange3.getFirst();
      int var5 = intRange3.getLast();
      if(i <= var5) {
         while(true) {
            String var6 = "in range";
            System.out.println(var6);
            System.out.println(i);
            if(i == var5) {
               break;
            }

            ++i;
         }
      }
 
 ## 关键字
 
 ### as ：类型转换/重命名
 
 Kotlin中的as关键字不仅可以用来对类进行类型转换，还可以用来对相同类名不同包的类进行重命名；
 
 对于在同一文件中引入相同类名的类情况，在Kotlin中通过as关键字可以对其进行重命名：
 
     import java.lang.AlerDialog  as SimpleDialog
     import suport.v7.AlerDialog  as FineDialog
 
