## 本文主要是记录一下在日常开发中的一些总结和一些规范性的东西

###目录
* android命名规范
* android注意事项
* android小技巧
* android兼容性bug

##android命名规范

<table>
	<tr>
		<td>TextView
		</td>
		<td>
		</td>
	</tr>
</table>
##android注意事项
###1. Handler的使用
&ensp;&ensp;&ensp;&ensp;Handler机制有一个特点是不会随着Activity、Service的生命周期结束而结束。也就是说，如果你Post了一个Delay的Runnable，然后在Runnable执行之前退出了Activity，Runnable到时间之后还是要执行的。如果Runnable里面包含更新View的操作，程序崩溃了。

&ensp;&ensp;&ensp;&ensp;解决方案：

&ensp;&ensp;&ensp;&ensp;1.采用EventBus的开发方式，线程中的数据都可以通过消息的形式传到主Activity中。

&ensp;&ensp;&ensp;&ensp;2.采用[android-weak-handler](https://github.com/badoo/android-weak-handler),具体使用方式可以点击连接进行查看。

&ensp;&ensp;&ensp;&ensp;3.`@Override
    protected void onDestroy() {
        super.onDestroy();
        handler.removeCallbacksAndMessages(null);
    }`
##android小技巧
###1. SharedPreferences小技巧
&ensp;&ensp;&ensp;&ensp;SharedPreferences.Editor.commit这个方法是同步的，一直到把数据同步到Flash上面之后才会返回，由IO操作的不可控，尽量使用apply方法代替。apply只在API Level>=9才会支持，需要做兼容。
###2. ADB 命令技巧1
&ensp;&ensp;&ensp;&ensp;如果你用的是mac或者是linux再或者是windows上面得git shell，那么你就可以使用如下文件adb.sh,把下面的内容放入到adb.sh中，然后通过执行./adb.sh+空格+包名的形式去过滤定位到当前应用：

	
	#!/bin/bash
	packageName=$1
	pid=`adb shell ps | grep $packageName | awk '{print $2 }'`
	real_pid=`echo $pid | awk '{print $1}'`
	echo $real_pid+"#######"
	adb logcat | grep --color=auto $real_pid

###2. ADB 命令技巧2
&ensp;&ensp;&ensp;&ensp;当你发现你的adb命令不能使用或者是打不开的时候，可以试着采用如下命令去查看下是否有进程占用了你的端口：

&ensp;&ensp;&ensp;&ensp;linux or mac：

&ensp;&ensp;&ensp;&ensp;`netstat -ano | grep "5037"`

&ensp;&ensp;&ensp;&ensp;window：

&ensp;&ensp;&ensp;&ensp;`netstat -ano | findstr "5037"`

###3.string.xml技巧
&ensp;&ensp;&ensp;&ensp;在string.xml中定义html代码，可以采用如下的方式：

&ensp;&ensp;&ensp;&ensp;`<string name="effect">影响力：<![CDATA[<font color="#ff484a">%1$s</font>]]></string>
`

&ensp;&ensp;&ensp;&ensp;在string.xml中使用空格可以通过代码 `&#160;:&#160;`
去实现。

###4.android studio使用技巧
&ensp;&ensp;&ensp;&ensp;1. 检查无用的资源，在项目中点击右键，在出现的右键菜单中有“Analyze” --> “run inspaction by Name ...”。在弹出的搜索窗口中输入想执行的检查类型，如“Unused Resources”
##android兼容性bug 