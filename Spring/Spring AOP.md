



# 1、什么是面向切面编程

## 1.1 AOP术语

### 通知

定义了切面是什么以及何时使用，Spring切面可用应用5种通知：

* 前置通知（Before）：在目标方法被调用之前执行
* 后置通知（After）：在目标方法被调用之后执行
* 返回通知（After-returning）：在目标方法成功返回之后执行
* 异常通知（After-throwing）：在目标方法抛出异常后执行
* 环绕通知（Around）：在目标方法前后自定义行为

### 连接点

连接点是应用程序在执行时能插入切面的一个点。这个点可用是调用方法的前后或抛出异常等等。切面代码可用通过这些点插入到程序的正常流程中，并添加新的行为。

### 切点

切点是一个或多个连接点的集合。通常使用明确的类或方法名称，或是利用正则表达式来指定切点。

### 切面

切面是切点和通知的结合。切面通过通知和切点定义了它的内容、在何时、何地完成其功能。

### 织入

把切面应用到目标对象并创建新的代理对象的过程。切面在指定的连接点被织入到目标对象中，织入的时期：

* 编译期：切面在目标被编译时被织入
* 类加载器：切面在目标被加载到JVM时被织入，这种方式需要类加载器（ClassLoader）
* 运行期：切面在应用程序的某个时刻被织入，AOp容器会为目标对象创建一个代理对象，Spring AOP就是这种实现方式。

## 1.2 SPring对AOP的支持

### 4种类型的AOP支持

* 基于代理的Spring AOP
* 纯POJO切面
* @AspectJ注解驱动的切面
* 注入式AspectJ切面

**注：Spring对AOP的支持局限于方法拦截，即方法级别**



### Spring在运行时通知对象

Spring AOP的实现方式其实是在目标对象外面包裹一层代理类，这个代理类拦截被通知方法的调用，会先调用切面逻辑，然后再执行目标对象的方法。



# 2、切点的定义

| AspectJ指示器 | 描述                                          |
| ------------- | --------------------------------------------- |
| arg()         | 限制连接点匹配参数为指定类型的执行方法        |
| @args()       | 限制连接点匹配参数为指定注解标注的的执行方法  |
| execution()   | 匹配连接点的执行方法                          |
| this()        | 限制连接点匹配AOP代理的bean引用为指定类型的类 |
| within()      | 限制连接点匹配指定的参数                      |
| @within()     | 限制连接点匹配指定注解标注的类型              |

## 2.1 编写切入点

示例：

```java
execution(* concert.Performance.perform(..))
```

* *：返回任意类型

* `concert.Performance：方法所属的类`

* `perform()`：方法名

* `(..)`：参数任意

## 2.2 在切点中选择bean

使用bean()通过beanID和bean名称作为参数来限制切点只匹配特定的bean

示例：

```java
//限定bean为person
execution(* concert.Performance.perform() and bean('person'))
    
//限定不为为person的bean
execution(* concert.Performance.perform() and !bean('person'))
```



# 3、使用注解创建切面

在类上通过使用@AspectJ注解来表明这是一个切面类

相应的通知注解：

| 注解            | 通知                             |
| --------------- | -------------------------------- |
| @After          | 通知方法在目标方法被调用之后执行 |
| @Before         | 通知方法在目标方法被调用之前执行 |
| @AfterReturning | 通知方法在目标方法返回后执行     |
| @AfterThrowing  | 通知方法在目标方法抛出异常后执行 |
| @Around         | 通知方法将目标方法封装起来       |
| @PointCut       | 定义一个切点                     |

示例：

```java
//定义一个切点
@PointCut("execution(** concert.Performance.perform(..))")
public void performance(){}

//定义一个前置通知，@Before表达式指定performance()
@Before("performance()")
public void takeStates(){
    System.out.println("Taking States")
}
```

