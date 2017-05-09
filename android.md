# 为面试做的一些准备

> The Chance should be taked by the one who was ready !  

下面是一些面试中常考的Android知识点归纳。

持续更新中~~

## Activity 生命周期

在Activity的生命周期中，如下的方法会被系统回调：

onCreate(Bundle savedInstanceState)            Activity被创建调用。

onStart()					  Activity已经启动，但还不可以与用户进行交互。

onResume()					  当Activity可见，并准备与用户交互。

onPause()				  	  暂停Activity时被调用，调用了该方法后，Activity变得不可交互。

onStop()					  停止Activity时被调用，Activity变得不可见。

onDestroy()					  销毁Activity时被调用。

onRestart()					  重启Activity时被调用，当Activity从不可见重新变为可见时，就会调用该方法。

![图片1](E:\资料\MarkDown引用\图片1.png)

![图片2](E:\资料\MarkDown引用\图片2.png)

---

## Activity的四种加载模式以及使用场景

**standard 模式**

这是默认模式，每次激活Activity时都会创建Activity实例，并放入任务栈中。使用场景：大多数Activity。

**singleTop 模式**

如果在任务的栈顶正好存在该Activity的实例，就重用该实例( 会调用实例的 onNewIntent() )，否则就会创建新的实例并放入栈顶，即使栈中已经存在该Activity的实例，只要不在栈顶，都会创建新的实例。使用场景如新闻类或者阅读类App的内容页面。

**singleTask 模式**

如果在栈中已经有该Activity的实例，就重用该实例(会调用实例的 onNewIntent() )。重用时，会让该实例回到栈顶，因此在它上面的实例将会被移出栈。如果栈中不存在该实例，将会创建新的实例放入栈中。使用场景如浏览器的主界面。不管从多少个应用启动浏览器，只会启动主界面一次，其余情况都会走onNewIntent，并且会清空主界面上面的其他页面。

**singleInstance 模式**

在一个新栈中创建该Activity的实例，并让多个应用共享该栈中的该Activity实例。一旦该模式的Activity实例已经存在于某个栈中，任何应用再激活该Activity时都会重用该栈中的实例( 会调用实例的 onNewIntent() )。其效果相当于多个应用共享一个应用，不管谁激活该 Activity 都会进入同一个应用中。使用场景如闹铃提醒，将闹铃提醒与闹铃设置分离。singleInstance不要用于中间页面，如果用于中间页面，跳转会有问题，比如：A -> B (singleInstance) -> C，完全退出后，在此启动，首先打开的是B。

---

## 如何理解Activity，View，Window三者之间的关系？

打个比方。Activity像一个工匠（控制单元），Window像窗户（承载模型），View像窗花（显示视图）LayoutInflater像剪刀，Xml配置像窗花图纸。

1：Activity构造的时候会初始化一个Window，准确的说是PhoneWindow。

2：这个PhoneWindow有一个“ViewRoot”，这个“ViewRoot”是一个View或者说ViewGroup，是最初始的根视图。

3：“ViewRoot”通过addView方法来一个个的添加View。比如TextView，Button等

4：这些View的事件监听，是由WindowManagerService来接受消息，并且回调Activity函数。比如onClickListener，onKeyDown等。

---

## Activity与Fragment通信

## Service生命周期

## Binder机制原理

## Handler实现原理



## 自定义View 		

## View的绘制流程



## Touch事件的传递机制


## Android中的几种动画


## 跨进程通讯有几种方式


## AIDL


## 热修复的原理




