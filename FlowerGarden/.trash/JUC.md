
## 线程和进程的区别

当一个程序被运行，从磁盘加载这个程序的代码至内存，这时就开启了一个进程。

一个进程之内可以分为一到多个线程。 一个线程就是一个指令流，将指令流中的一条条指令以一定的顺序交给 CPU 执行




## 并发和并行的区别

并发是同一时间段做多件事情
并行是同一时间做多件事情

## 创建线程的四种方式

1. 继承Thread
2. 实现runnable
3. 实现Callable
4. 线程池创建

## Runnable和Callable有什么区别

1. Runnable 接口run方法没有返回值；Callable接口call方法有返回值，是个泛 型，和Future、FutureTask配合可以用来获取异步执行的结果 
2. Callalbe接口支持返回执行结果，需要调用FutureTask.get()得到，此方法会阻 塞主进程的继续往下执行，如果不调用不会阻塞。 
3. Callable接口的call()方法允许抛出异常；而Runnable接口的run()方法的异常 只能在内部消化，不能继续上抛


## run和start有什么区别

start(): 用来启动线程，通过该线程调用run方法执行run方法中所定义的逻辑代码。start方法只能被调用一次。 
run(): 封装了要被线程执行的代码，可以被调用多次。


## Synchronized底层原理

Monitor

字节码文件中会对代码块进行monitorenter和monitorexit

![](Attachments/Images/Pasted%20image%2020240319180442.png)


## jmm

![](Attachments/Images/Pasted%20image%2020240319183204.png)


## cas你知道吗

![](Attachments/Images/Pasted%20image%2020240319214347.png)

## AQS

![](Attachments/Images/Pasted%20image%2020240319214527.png)

![](Attachments/Images/Pasted%20image%2020240319214537.png)

![](Attachments/Images/Pasted%20image%2020240319214554.png)

## volatile

![](Attachments/Images/Pasted%20image%2020240319214437.png)
![](Attachments/Images/Pasted%20image%2020240319214452.png)

## ReentrantLock

![](Attachments/Images/Pasted%20image%2020240319214710.png)

![](Attachments/Images/Pasted%20image%2020240319214829.png)

## synchronized和lock有什么区别

![](Attachments/Images/Pasted%20image%2020240319215041.png)

## ThreadLocal

ThreadLocal 主要功能有两个，第一个是可以实现资源对象的线程隔离，让 每个线程各用各的资源对象，避免争用引发的线程安全问题，第二个是实现 了线程内的资源共享

## 底层

**![](Attachments/Images/Pasted%20image%2020240319223514.png)

## 内存泄露问题


