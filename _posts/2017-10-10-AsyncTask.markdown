---
layout:		post
title:		"AsyncTask基本用法"
subtitle:		"异步任简化线程处理"
date:		2017-10-11 12:00:00
author:		"Droodsunny"
header-img: "img/in-post/Android.jpg"
---




本篇随笔将讲解一下Android的多线程的知识，以及如何通过AsyncTask机制来实现线程之间的通信。

一、Android当中的多线程

在Android当中，当一个应用程序的组件启动的时候，并且没有其他的应用程序组件在运行时，Android系统就会为该应用程序组件开辟一个新的线程来执行。默认的情况下，在一个相同Android应用程序当中，其里面的组件都是运行在同一个线程里面的，这个线程我们称之为Main线程。当我们通过某个组件来启动另一个组件的时候，这个时候默认都是在同一个线程当中完成的。当然，我们可以自己来管理我们的Android应用的线程，我们可以根据我们自己的需要来给应用程序创建额外的线程。

二、Main Thread 和 Worker Thread

在Android当中，通常将线程分为两种，一种叫做Main Thread，除了Main Thread之外的线程都可称为Worker Thread。

当一个应用程序运行的时候，Android操作系统就会给该应用程序启动一个线程，这个线程就是我们的Main Thread，这个线程非常的重要，它主要用来加载我们的UI界面，完成系统和我们用户之间的交互，并将交互后的结果又展示给我们用户，所以Main Thread又被称为UI Thread。

Android系统默认不会给我们的应用程序组件创建一个额外的线程，所有的这些组件默认都是在同一个线程中运行。然而，某些时候当我们的应用程序需要完成一个耗时的操作的时候，例如访问网络或者是对数据库进行查询时，此时我们的UI Thread就会被阻塞。例如，当我们点击一个Button，然后希望其从网络中获取一些数据，如果此操作在UI Thread当中完成的话，当我们点击Button的时候，UI线程就会处于阻塞的状态，此时，我们的系统不会调度任何其它的事件，更糟糕的是，当我们的整个现场如果阻塞时间超过5秒钟(官方是这样说的)，这个时候就会出现 ANR (Application Not Responding)的现象，此时，应用程序会弹出一个框，让用户选择是否退出该程序。对于Android开发来说，出现ANR的现象是绝对不能被允许的。

# AsyncTask基本用法
1
```java
public abstract class AsyncTask<Params, Progress, Result> {  
```
**Params**在执行AsyncTask时需要传入的参数，可用于在后台服务中使用<br/>
**Progress**后台任务执行时，用于显示进度<br/>
**Result**当后台任务执行后返回的结果<br/>
##  常用的重写方法
1 onPreExecute() 一般用来在执行后台任务前对UI做一些标记。<br/>
2 doInBackground(Params...)  在onPreExecute()完成后立即执行，用于执行较为费时的操作，此方法将接收输入参数和返回计算结果。在执行过程中可以调用publishProgress(Progress... values)来更新进度信息,将进度发送给onProgressUpdate(Result...)<br/>
3 onProgressUpdate(Progress... values),在调用publishProgress(Progress... values)时，此方法被执行，直接将进度信息更新到UI组件上。参数为Progress...,在这可以执行接口的回调函数用于给通知更新进度。<br/>
4 onPostExecute(Result result)  当后台操作结束时，此方法将会被调用，计算结果将做为参数传递到此方法中，直接将结果显示到UI组件上。接收doInBackground(Params...) 的返回数据，通常根据数据执行接口的回调函数。

## 启动任务
**excute(Params...)** 参数与doInBackground中的参数相同。 

[详细代码](https://github.com/LuffyBySunny/DownloadService/blob/master/app/src/main/java/com/Util/DownloadTask.java)


