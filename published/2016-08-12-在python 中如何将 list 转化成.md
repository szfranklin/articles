#在python 中如何将 list 转化成 dictionary
###标签： python zip函数
*** 

####问题1：如何将一个list转化成一个dictionary？

#####问题描述：比如在python中我有一个如下的list，其中奇数位置对应字典的key，偶数位置为相应的value

![1](http://static.datartisan.com/upload/attachment/2016/08/IWzqWMkj.png)

#####解决方案:

######1.利用zip函数实现  

![2](http://static.datartisan.com/upload/attachment/2016/08/4qfSIDAS.png)

######2.利用循环来实现

![3](http://static.datartisan.com/upload/attachment/2016/08/m9iSWtjD.png)  

######3.利用 enumerate 函数生成index来实现  

![4](http://static.datartisan.com/upload/attachment/2016/08/4q6q68w6.png)

*** 

####问题2 我们如何将两个list 转化成一个dictionary？

#####问题描述：假设你有两个list

![5](http://static.datartisan.com/upload/attachment/2016/08/jh3dxDfh.png)

#####解决方案：还是常见的zip函数
![6](http://static.datartisan.com/upload/attachment/2016/08/LI53PNpH.png) 

这里我们看到了zip函数确实在配对上面起到了很不错的效果，如果两个list都很大，你需要引入itertools.izip来解决问题

![7](http://static.datartisan.com/upload/attachment/2016/08/Gu9VoxIZ.png)

或者下面的直接使用dict函数  
![8](http://static.datartisan.com/upload/attachment/2016/08/dOGgJbb3.png)

#####那么如果我们有三个lsit呢？比如我们有时候会遇到这样的问题比如在一个经纬度下面记录某个数据，这个时候又该怎么实现呢？

![9](http://static.datartisan.com/upload/attachment/2016/08/GFuOE3gq.png)

我们可以看到这个时候 zip函数还是可以帮助我们成功的实现所需要的功能，首先将经纬度一一配对整合到一起，随后再将val连起来，最后使用dict函数放在一起。

通过上面的例子，我们知道可以通过zip函数的多次调用来整合数据，最终解决问题

*** 
![](http://static.datartisan.com/upload/attachment/2016/05/xKM5xlV4.png)
