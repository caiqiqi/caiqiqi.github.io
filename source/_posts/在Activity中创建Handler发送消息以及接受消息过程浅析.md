---
title: 在Activity中创建Handler发送消息以及接受消息过程浅析
date: 2016-07-11 03:23:21
tags: Android
---

在Github上：https://github.com/caiqiqi/Notes/blob/master/%E5%9C%A8Activity%E4%B8%AD%E5%88%9B%E5%BB%BAHandler%E5%8F%91%E9%80%81%E6%B6%88%E6%81%AF%E4%BB%A5%E5%8F%8A%E6%8E%A5%E5%8F%97%E6%B6%88%E6%81%AF%E8%BF%87%E7%A8%8B%E6%B5%85%E6%9E%90.md

ActivityThread::main() ->
...
Looper.prepareMainLooper();
```java
public static void prepareMainLooper() {
        prepare(false);    // sThreadLocal.set(new Looper(quitAllowed));
        synchronized (Looper.class) {
            if (sMainLooper != null) {
                throw new IllegalStateException("The main Looper has already been prepared.");
            }
            sMainLooper = myLooper();
        }
    }
	
/**
* Return the Looper object associated with the current thread.  Returns
* null if the calling thread is not associated with a Looper.
*/
public static Looper myLooper() {
    return sThreadLocal.get();
}

private Looper(boolean quitAllowed){
    mQueue = new MessageQueue(quitAllowed);
	mRun = true;
	mThread = Thread.currentThread();
}
```

...

Looper最重要的一个方法是loop()方法，只有调用了loop后，消息循环系统才会真正地起作用。
```java
public static void loop(){
    final Looper me = myLooper();
	if (me == null){
	    throw new RuntimeException("No Loooper; Looper.prepare() wasn't called on this thread.");
	}
	final MessageQueue queue = me.mQueue;
	...
	for(;;){
	    Message msg = queue.next();    //might block
		// No message indicates that the message queue is quitting.
		return;
	}
	...
	msg.target.dispatchMessage(msg);    //这里的msg.target指的就是发送这条消息的Handler对象，这样Handler发送的消息最终又交给它的dispatchMessage方法来处理了。
	//但这里不同的是Handler的dispatchMessage()是在创建Handler时所使用的Looper的Looper.loop()方法中执行的，这样就成功将代码逻辑切换到指定的线程中去执行了。
	...
}
```
那Handler的dispatchMessage()是怎样的呢？
```java
    /**
     * Subclasses must implement this to receive messages.
     */
    public void handleMessage(Message msg) {
    }
    
    /**
     * Handle system messages here.
     */
    public void dispatchMessage(Message msg) {
        if (msg.callback != null) {
            handleCallback(msg);
        } else {
            if (mCallback != null) {
                if (mCallback.handleMessage(msg)) {
                    return;
                }
            }
            handleMessage(msg);    //即最终还是调动handlerMessage()来处理接收到的消息，不过这个方法得创建Handler时由程序员自己复写。
        }
    }
```



当在Activity中
```java
private Handler handler = new Handler();
```
时，

```java
public Handler(){
    ...
	mLooper = Looper.myLooper();    //取出由ActivityThread类的main()中prepare(false)之后存储在sThreadLocal中的mLooper对象，然后赋值给Handler对象的mLooper，即，拿到与主线程关联的Looper对象
	mQueue = mLooper.mQueue;    //将mLooper中的mQueue(MessageQueue)对象赋值给Handler对象的mQueue，之后Handler就可以把发送的消息存储在这个MessageQueue之中
	mCallback = null;
}
```

在Activity中调用handler对象的
`handler.sendMessage(...)`;
最终调用的是：
```java
    public boolean sendMessageAtTime(Message msg, long uptimeMillis) {
        MessageQueue queue = mQueue;
        if (queue == null) {
            RuntimeException e = new RuntimeException(
                    this + " sendMessageAtTime() called with no mQueue");
            Log.w("Looper", e.getMessage(), e);
            return false;
        }
        return enqueueMessage(queue, msg, uptimeMillis);
    }
	
	private boolean enqueueMessage(MessageQueue queue, Message msg, long uptimeMillis) {
        msg.target = this;    //也就是说，这个消息是发送给Handler这个对象自己的！！！
        if (mAsynchronous) {
            msg.setAsynchronous(true);
        }
        return queue.enqueueMessage(msg, uptimeMillis);    //作用就是往消息队列中插入一条消息(单链表的插入操作)
    }
```