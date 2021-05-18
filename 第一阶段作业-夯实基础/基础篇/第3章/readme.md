# 第三章节

## Class类
- Java文件必须以.java作为拓展名

- 一个Java文件只能有一个public class
- public class 的名字必须和文件名字一样，大小写都要完全一致
- 一个文件可以有多个class但是之能有一个是public
- Java的最小独立单元
- Java项目就是由一个个类组成的
- 类的构成
>成员变量/属性
>成员方法/函数

## Main函数
- 一个class最多只能有一个main函数
- 程序的入口都是main函数
- main 函数前 public static void等不能省略
- main 函数无法被其他方法/类调用
- 一个Java程序可以调用多个其他Java class
- String[] args是main函数的形参

## 基本数据类型

### boolen 布尔
- 只有ture和false默认为false

### byte 字节
- 1byte = 8bits
- 储存是有符号的，以二进制补码表示的整数
- 最小值-128，最大值127，默认值是0
- byte类型用在大型数组中可以显著节约空间byte占用的空间只有int的1/4
- byte在二进制文件中读写使用的较多

### short/int/long 短整数/整数/长整数
>short
>>16位，2字节，有符号的以二进制补码表示的整数-2^15~2^15-1,默认值为0

>int
>>32位，4个字节有符号的以二进制补码表示的整数-2^31~2^31-1,默认值为0

>long
>>64位，8字节，有符号的以二进制补码表示的整数-2^63~2^63-1,默认值为0

### float/double 浮点数
>float
>>float是32位宽，其范围约为 1.4e-045 至 3.4e + 038 。Java中的浮点字面值默认为双精度。要指定浮点字面值，必须在该常量后面附加一个 F 或 f 

>double
>>Java double类型表示双精度数字,double是64位宽，其范围大约从4.9e-324到1.8e + 308

### char 字符
>char数据类型是16位无符号Java基元数据类型。它表示Unicode字符。char是无符号数据类型。因此，char变量不能为负值。字符数据类型的范围为0到65535，这与Unicode集的范围相同

- **常见**

| 操作符      | 描述 |
| ----------- | ----------- |
| +      | 加法 - 相加运算符两侧的值       |
| -   | 减法 - 左操作数减去右操作数        |
| *	   | 	乘法 - 相乘操作符两侧的值        |
| /   | 	除法 - 左操作数除以右操作数        |
| ％   | 	取模 - 左操作数除以右操作数的余数        |
| ++   | 	自增 - 操作数的值增加1        |
| --   | 	自减 - 操作数的值减少1        |

## 选择结构
```
//单分支结构
if  （判断条件）{

        主体语句

}

//双分支结构 

if （判断条件）{

        代码块1；

}else{

        代码块2；

}

//多重if选择结构

if（判断条件1）{

        代码块1；

}else if （判断条件2）{

        代码块2；

}else{

        代码块3；

}

//switch选择结构

switch（变量）{

case 常量值1:

        代码块1；

        break；

case 常量值2:

        代码块2；

        break；

default:

        代码块3；

        break；

}

//嵌套条件结构 

if （判断条件1）{

        if （判断条件2）{

        代码块1；

}else{

        代码块2；

}

}else{

        代码块3

}



```

## Java自定义方法
```
public 返回值类型 方法名(参数列表) {
//方法体
//return 返回值类型;
}
```
## 作业

```
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        System.out.println("输入行数");
        int height = sc.nextInt();
        int cnt=1;
        if (height>0){
            for (int i = 0; i < height; i++) {
                for (int j = 0; j < height; j++) {
                    System.out.print(cnt+++" ");
                }
                System.out.println("");
            }
        }
    }

}
```
```
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        System.out.println("输入塔的层数");
        int row = sc.nextInt();
        if ((row > 4 && row < 12) && (row & 1) != 0) {
            int gap = row / 2 + 1;
            int star = 1;
            for (int i = 0; i < row; i++) {
                for (int j = 0; j < gap; j++) {
                    System.out.print(" ");
                }
                for (int j = 0; j < star; j++) {
                    System.out.print("*");
                }
                System.out.println("");

                if (i < row / 2) {
                    star += 2;
                    gap--;
                } else {
                    star -= 2;
                    gap++;
                }
            }
        }
    }

}
```