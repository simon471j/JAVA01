# 第七章节

----------


## package

* 同一个目录下两个类名字不能相同
* 同一个目录下类之间的互相调用不需要显式声明调用
* 类的完整名字是包名+类名

## import
* 可以用**import**关键字来引入需要的类
* **import**必须全部放在package之后定义类之前
* 可以用*来引入一个目录下的所有类，比如import java.lang.** 意思是包括java.lang下面所有的类文件，但是不包括所有子目录文件
* import尽量精确，不推荐使用*

## jar文件导出和导入
- jar文件是Java所特有的一种文件格式，可用于执行文件的传播
- jar文件实际上是一组class文件的压缩包
- jar文件可以包括多个class，比多层目录更加简介实用
- jar文件只包括class而没有包含Java文件，在保护源文件和知识版权方面可以起到更好的作用
- 可以把多个class文件压缩成jar文件，可以规定给一个版本号，更容易进行版本控制

## 访问权限
- private 私有的，只能本类访问
- default 通常忽略不写，同一个包类访问
- protected 同一个包，子类均可以访问
- public 公开的，所有类都可以访问
>四种都可以用来修饰成员变量、成员方法、构造函数
>default和public可以修饰类

## 代码作业
- bussiness包

```
package com.bussiness;

import com.test.Worker;

public class Boss {
    public static void main(String[] a)

    {

        Worker w = new Worker();

        w.sayHello();

    }
}
```
- test包


```

package com.test;


public class Worker {

    public void sayHello() {

        System.out.println("hello");

    }

}

```