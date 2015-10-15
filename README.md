## 本文主要是记录一下在日常开发中的一些总结和一些规范性的东西

###目录
* android命名规范
* android注意事项
* android小技巧
* android兼容性bug

##android命名规范

##android注意事项
1. Handler的使用：
Handler机制有一个特点是不会随着Activity、Service的生命周期结束而结束。也就是说，如果你Post了一个Delay的Runnable，然后在Runnable执行之前退出了Activity，Runnable到时间之后还是要执行的。如果Runnable里面包含更新View的操作，程序崩溃了。

&ensp;&ensp;&ensp;&ensp;解决方案：
##android小技巧
##android兼容性bug 