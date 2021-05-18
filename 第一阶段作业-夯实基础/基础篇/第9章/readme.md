#第九章节
##异常概述
>异常
>>int a = 5/0;
>>数组越界访问
>>读取不存在的文件

- 异常处理 ：程序返回到安全的状态， 允许用户保存结果，并以适当方式关闭程序

##异常分类
>Throwable 所有错误的祖先
>>Error 系统内部错误或者资源耗尽 

>>**Exception** 程序有关的异常
>>>IOException 外界相关的错误
>>>RuntimeException 程序自身错误的异常

##异常处理
- 允许用户即使保存结果
- 抓住异常，分析异常内容
- 控制程序返回到安全状态

```
try{
	
	//代码执行区域
	
}catch(Exception e){
	
	//异常处理区域
	
}finally {
		
	//无论如何,都会执行的代码块
 
}
```
- try 正常业务逻辑代码
- catch 当try发生异常将执行catch代码，若无异常就绕过
- finally 执行了try或catch之后必须执行finally
- catch块可以有多个，每个有不同的入口形参
- 进入catch之后并不会返回到try发生异常的位置，也不会执行后续的catch代码块，一个异常只能进入一个catch代码块
- 一个方法被覆盖，那么覆盖她的方法必须抛出相同的异常或者异常的子类
- 如果父方法抛出多个异常，那么重写的子类方法必须抛出那些异常的子类，也就是不能抛出新的异常

###throws
- 方法存在可能异常的语句，但不处理，那么可以使用throws来声明异常
- 调用带有throws异常的方法，要么处理这些异常，要么再次向外throws，直到main函数为止

##自定义异常
- 需要继承Exception类或者其子类
1. 继承自Exception就变成Checked Exception
2. 继承RuntimeException就变成Unckecked Exception

- 自定义重点在构造方法
1. 调用父类Exception的message构造函数
2. 可以自定义自己的成员变量

- 在程序中采用throw主动抛出异常

