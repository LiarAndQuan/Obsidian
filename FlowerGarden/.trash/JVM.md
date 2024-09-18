
## jvm组成

![](Attachments/Images/Pasted%20image%2020240319224939.png)

![](Attachments/Images/Pasted%20image%2020240319224946.png)
## 类加载器种类

![](Attachments/Images/Pasted%20image%2020240319230903.png)


## 双亲委派模型

如果一个类加载器在接到加载类的请求时，它首先不会自己尝试去加载这个类， 而是把这个请求任务委托给父类加载器去完成，依次递归，如果父类加载器可以 完成类加载任务，就返回成功；只有父类加载器无法完成此加载任务时，才由下 一级去加载。

## 为什么要采用双亲委派机制

![](Attachments/Images/Pasted%20image%2020240319231603.png)
## 类的加载过程

加载，验证，准备，解析，初始化，使用，卸载

![](Attachments/Images/Pasted%20image%2020240319232151.png)

![](Attachments/Images/Pasted%20image%2020240319232202.png)

![](Attachments/Images/Pasted%20image%2020240319232213.png)

![](Attachments/Images/Pasted%20image%2020240319232244.png)
![](Attachments/Images/Pasted%20image%2020240319232421.png)
![](Attachments/Images/Pasted%20image%2020240319232430.png)

![](Attachments/Images/Pasted%20image%2020240319232440.png)

## 串行垃圾回收期

![](Attachments/Images/Pasted%20image%2020240319233646.png)

![](Attachments/Images/Pasted%20image%2020240319233902.png)
![](Attachments/Images/Pasted%20image%2020240319234130.png)



## G1

![](Attachments/Images/Pasted%20image%2020240319234858.png)
![](Attachments/Images/Pasted%20image%2020240319235049.png)

![](Attachments/Images/Pasted%20image%2020240319235148.png)

![](Attachments/Images/Pasted%20image%2020240319235244.png)

老年代占用内存超过阈值（45%），触发并发标记和重新标记，类似cms
![](Attachments/Images/Pasted%20image%2020240319235412.png)


