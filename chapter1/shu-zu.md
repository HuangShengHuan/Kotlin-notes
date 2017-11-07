# 数组

## 区间range
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

### step设置区间步长

    for (x in 1..10 step 2) {
        print(x)
    }

### 反向区间

    for (x in 9 downTo 0 step 3) {
        print(x)
    }

### until左闭右开区间

    for (i in 1 until 100) { // 半开区间：不包含 100
        …… 
    } 
