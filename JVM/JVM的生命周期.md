## 虚拟机的启动

Java虚拟机的启动是通过类加载器`bootstrap class loader`创建一个初始化`initial class`来完成的，这个类是由虚拟机的具体实现指定的。

## 虚拟机的执行

程序开始执行时JVM执行，程序结束时才结束

执行一个Java程序的时候，真正执行的是一个Java虚拟机的进程。

## 虚拟机的退出

有以下几种情况：

* 程序正常执行结束
* 程序在执行过程中遇到异常或错误而异常终止
* 由于操作系统出现错误而导致虚拟机进程终止
* 某个线程调用Runtime类或System类的`exit`方法，或Runtime类的`halt`方法，并且Java安全管理器也允许这次exit或halt操作
* JNI规范描述了用`JNI Invocation API`来加载或卸载Java虚拟机时，Java虚拟机的退出情况