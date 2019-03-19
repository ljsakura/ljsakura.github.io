R语言在画图形的时候，经常遇到颜色设定问题，用户可以根据color、rgb值和hsv值来设定不同的颜色。第一列是颜色，第二列是hsv，第三列是rgb
 

在颜色设定时，最好是根据自己想要的颜色来设定。这样我们能做到心中有数。  
所有可以用下面方法来设定颜色，简单有效。
```r
library(grDevices);
ramp <- colorRamp(c("red", "white","blue"));

ramp(seq(0, 1, length = 5))
      [,1]  [,2]  [,3]
[1,] 255.0   0.0   0.0
[2,] 255.0 127.5 127.5
[3,] 255.0 255.0 255.0
[4,] 127.5 127.5 255.0
[5,]   0.0   0.0 255.0

color<-rgb( ramp(seq(0, 1, length = 5)), max = 255)
color
[1] "#FF0000" "#FF7F7F" "#FFFFFF" "#7F7FFF" "#0000FF"

barplot(1:5,col=color)
```
 


研究者可以根据这个函数colorRamp(c("red", "white","blue"));  
设定不同的颜色。

如果你对颜色不了解。你也可以参考http://research.stowers-institute.org/efg/R/Color/Chart/index.htm确定颜色。

除此之外，研究者还可以根据下面函数设定不同的颜色。
```r
rainbow(n, s = 1, v = 1, start = 0, end = max(1,n - 1)/n,gamma = 1, alpha = 1);
heat.colors(n, alpha = 1);
terrain.colors(n, alpha = 1);
topo.colors(n, alpha = 1);
cm.colors(n, alpha = 1);
```
函数中n代表你需要颜色的数目。
