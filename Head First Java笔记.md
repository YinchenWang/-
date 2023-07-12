# Head First Java笔记

## 基本概念

### Java工作方式：

源码->编译器->输出（字节码 .class）-> JVM

### Java程序结构：

Sourcefile（class file（methods）））

源文件：带有类的定义的 .java 文件。
类：类中带有一个或多个方法。
方法：函数或者过程，由一组代码语句构成。

### 编译器与JVM：

java 是强类型语言，编译器不容许变量保存类型的数据。
JVM会遇到错误类型的数据而抛出，这是编译器为了容许动态绑定的功能而造成。

### 类与对象：

#### 类：

类是对象的蓝图。

#### 对象：

对象是类的实例

对象是已知的事物、对象会执行动作

对象本身已知的事物称为：实例变量 instance variable。 代表对象的状态（数据）

对象可执行的动作称为：方法 methods。

```java
class Dog{
  int size;
  String bread;
  String name;
  
  void bark(){
    System.out.println("汪！汪！");
  }
}

class DogTestDrive{
  public static void main(String[] args){
    Dog d = new Dog();
    s.size = 40;
    d.bark();
  }
}
```

#### Main 的用途：

测试真正的类。

启动java程序

#### Java内存回收：

创建对象时，对象被存储在堆中，这是个可回收垃圾的堆（garbage-collectible heap）

Java根据对象大小分配内存空间，当对象被标记为可回收时，垃圾收集器就开始启动。

## Java变量：

变量类型：

primitive主数据类型、引用

变量声明：

使用驼峰法、变量必须有类型、变量必须有名字、避开关键字

### Primitive主数据类型：

| 类型    | 位数    | 值域                   |
| ------- | ------- | ---------------------- |
| boolean | JVM决定 | True/False             |
| char    | 16 bits | 0 ~ 65535              |
| byte    | 8 bits  | -128 ~ 127             |
| short   | 16 bits | -32768 ~ 32767         |
| int     | 32 bits | -2147483648 2147483647 |
| Long    | 64 bits | 很大                   |
| flaot   | 32 bits | 可变                   |
| double  | 64 bits | 可变                   |

对象的使用不存在对象变量，只有**引用到对象的变量**，对象引用变量**保存的是存取对象的方法**（类似于指向对象的指针），只有JVM知道如何引用来取得对象

