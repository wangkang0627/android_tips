## 本文主要是记录一下在日常开发中的一些总结和一些规范性的东西

###目录
* android命名规范
* android注意事项
* android小技巧
* android兼容性bug

##android命名规范

##android注意事项
###1. Handler的使用
&ensp;&ensp;&ensp;&ensp;Handler机制有一个特点是不会随着Activity、Service的生命周期结束而结束。也就是说，如果你Post了一个Delay的Runnable，然后在Runnable执行之前退出了Activity，Runnable到时间之后还是要执行的。如果Runnable里面包含更新View的操作，程序崩溃了。

&ensp;&ensp;&ensp;&ensp;解决方案：

&ensp;&ensp;&ensp;&ensp;1.采用EventBus的开发方式，线程中的数据都可以通过消息的形式传到主Activity中。

&ensp;&ensp;&ensp;&ensp;2.采用[android-weak-handler](https://github.com/badoo/android-weak-handler),具体使用方式可以点击连接进行查看。
##android小技巧
###1. SharedPreferences小技巧
&ensp;&ensp;&ensp;&ensp;SharedPreferences.Editor.commit这个方法是同步的，一直到把数据同步到Flash上面之后才会返回，由IO操作的不可控，尽量使用apply方法代替。apply只在API Level>=9才会支持，需要做兼容。
###2. ADB 命令技巧
&ensp;&ensp;&ensp;&ensp;如果你用的是mac或者是linux再或者是windows上面得git shell，那么你就可以使用如下的命令：

	
		#!/bin/bash
		packageName=$1
		pid=`adb shell ps | grep $packageName | awk '{print $2 }'`
		real_pid=`echo $pid | awk '{print $1}'`
		echo $real_pid+"#######"
		adb logcat | grep --color=auto $real_pid
    
 
##android兼容性bug 