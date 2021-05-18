#第四章节
---
##面向对象思想

- 面向对象编程是当今主流的程序设计思想，已经取代了过程化程序开发技术，Java 是完全面向对象编程语言，所以必须熟悉面向对象才能够编写 Java 程序。

- 面向对象的程序核心是由对象组成的，每个对象包含着对用户公开的特定功能和隐藏的实现部分。程序中的很多对象来自 JDK 标准库，而更多的类需要我们程序员自定义。

- 从理论上讲，只要对象能够实现业务功能，其具体的实现细节不必特别关心。


##类
- 是抽象的概念集合，表示的是一个共性的产物，类之中定义的是属性和行为（方法）


##对象
- 对象是一种个性的表示，表示一个独立的个体，每个对象拥有自己独立的属性，依靠属性来区分不同对象。



##构造函数

- 作用：一般用来初始化成员属性和成员方法的，即new对象产生后，就调用了对象的属性和方法。
- 构造函数是对象一建立就运行，给对象初始化，就包括属性，执行方法中的语句。

1. 函数名与类名相同

2. 不用定义返回值类型。（不同于void类型返回值，void是没有具体返回值类型；构造函数是连类型都没有）

3. 不可以写return语句。（返回值类型都没有，故不需要return语句）

- 默认构造函数
1. 当一个类中没有定义构造函数时，系统会给该类中加一个默认的空参数的构造函数，方便该类初始化。只是该空构造函数是隐藏不见的

2. 子类所有的 构造函数 默认调用父类的无参构造函数（构造函数不会被继承，只是被子类调用而已），父类参数是private的，无法直接访问。

##this和super
- this
1. this是自身的一个对象，代表对象本身

2. 普通直接引用当前对象本身

3. 形参和成员名重名，用this来区分

4. 引用构造方法 ，this(参数) ，应该为构造函数中的第一条语句，调用的是本类中另外一种形式的构造方法

- super
1. super可以理解为是指向自己超（父）类对象,这个超类指的是离自己最近的一个父类。也大致分为3中中用法

2. 普通的直接引用，与this类似，只不过它是父类对象，可以通过它调用父类成员。

3. 子类中的成员变量或方法与父类中的成员变量或方法同名，可以使用super区分。

4. 引用构造方法，super（参数）：调用父类中的某一个构造方法（应该为构造方法中的第一条语句）。

##信息隐藏原则
- Getter
- Setter



## 代码作业


```
import java.util.Scanner;


public class Main {

    public static void main(String[] args) {

        //创建Scanner对象

        //System.in表示标准化输入，也就是键盘输入

        Scanner sc = new Scanner(System.in);

        //利用hasNextXXX()判断是否还有下一输入项

        int a = 0;

        int b = 0;

        int c = 0;

        if (sc.hasNext()) {

            a = sc.nextInt();

        }

        if (sc.hasNext()) {

            b = sc.nextInt();

        }

        if (sc.hasNext()) {

            c = sc.nextInt();

        }

        if (a == 0 || b == 0 || c == 0) {

            System.out.println("输入不能为0");

            System.exit(-1);

        }


        MyNumber obj1 = new MyNumber(), obj2 = new MyNumber(), obj3 = new MyNumber();
        //从这里开始，基于swap函数，完善你的程序

        //

        //

        //

        //程序结束


        obj1.num = a;
        obj2.num = b;
        obj3.num = c;

        swap(obj1, obj2);
        swap(obj2, obj3);
        swap(obj1, obj2);
        System.out.println(obj1.num + " " + obj2.num + " " + obj3.num);

    }


    public static void swap(MyNumber m, MyNumber n) {

        if (m.num > n.num) {

            int s = m.num;

            m.num = n.num;

            n.num = s;

        }

    }

}


class MyNumber {

    int num;

}

```