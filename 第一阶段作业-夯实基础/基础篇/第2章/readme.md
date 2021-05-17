#第二章节
##安装JDK
- 设置一个专门放Java的文件
- Win系统变量优先级大于用户变量

##安装Eclipse
- Eclipse市场占有率最高
- IDE的使用
>创建工程
>编写、格式化代码
>编译、运行
>调试
>发布
- 编写第一个Java程序

```
public class HelloWorld {

	public static void main(String[] args) {

		System.out.println("Hello World!");

		int a = 1;

		a=a+1;

		a=a+2;

		System.out.println("a is " + a);

		a=a+3;  //断点行

		a=a+4;

		System.out.println("a is " + a);

	}

}
```
- 断点处为a=4