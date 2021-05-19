# 第三章节
## Java字符编码
- Ascii码
1. 用一个字节（1 Byte = 8 bits）来存储a~z A~Z 0~9和一些常用的符号
2. 用于显示英语西欧语言
3. 常用的ascii码 回车键（13，00001101） 0（48，00110000） A（65，01000001） a（97，01100001）
4. 无法适应其他地方 如汉字数量有几十万

### 拓展编码（加字节）
- ISO8859(1-15)西欧语言
- **GB2132 GBK GB18030 ASCII + 中文**
- Big5 ASCII + 繁体中文
- Shift_JIS ASCII + 日文
- **Unicode 编码**

#### Unicode
- **UTF-8 兼容ASCII 变长（1-4个字节存储字符） 经济 方便传输**
- UTF-16 用变长（2-4个字节）来存储所有字符
- UTF-32 用32bits（4个字节）存储所有字符

#### ANSI编码
- 可以变的编码格式 用什么打开就是上面编码

### Java的字符编码
- **源文件编码 采用UTF-8编码**
- 程序内部采用UTF-16来编码
- **和外界的输入输出尽量采用UTF-8**
- **不能用一种编码写入 又用另一种编码读出
**

## Java国际化编程
- 多语言版本的软件
- 一套软件 多个语言包
- 根据语言设定 可以切换显示文本
- java.util.ResourceBundle 用于加载一个语言 国家语言包
- java.util.Locale定义一个语言 国家
- ResourceBundle会根据不同地区加载不同的properties文件
- 不同语言编码的properties文件会存放不同的语言编码内容
- 根据Locale要求 加载语言文件（properties文件）
- 储存语言集合中所有的键值对


```
ResourceBundle bundle  = ResourceBundle.getBundle("msg",myLocale);

System.out.println(bundle.getString("name"));
//name为键值对中的键
```

### Locale类
- Locale(zh_CN,en_US,...)
- 语言，zh，en等
- 国家/地区，CN，US等
- getDefault()返回默认的Locale（比如当前的jdk的编码）

### 语言文件
- 一个properties文件
- 包含K-V对
- 命名规则
- 包名+语言+国家地区.properties
1. message.properties
2. message_zh.properties
3. message_zh_CN.properties
- 存储文件必须是ASCII编码
- 如果是ASCII意外的文字 必须用Unicode表示
- 可以采用native2ascii.exe(%JAVA_HOME%\bin目录下)进行转码
- c:\temp>nativi2ascii a.properties message_zh_CN.properties 
- **其中temp为存储a.properties的文件夹**

## 高级字符串处理
### 正则表达式
- java.util.regex包
- Pattern 正则表示的编译表示
- Matcher
- \\b表示边界
- a* 代表a可以又0个或者多个
- Matcher的replaceAll();方法可以替换指定的pattern为指定的值

```
private static final String REGEX = "\\\\bdog\\\\b"; // \\b表示边界
private static final String INPUT = "dog dog dog doggie dogg";
Pattern p = Pattern.compile(REGEX);
Matcher m = p.marcher(INPUT);
int count = 0;
while(m.find()){
	count++;
	System.out.println{"Match number" + count};
	System.out.println{"start():" + m.start()};
	System.out.println{"end():" + m.end()};
}

执行结果：
Match Number 1
start(): 0 
end(): 3
Match Number 2
start(): 4
end(): 7
Match Number 3
start(): 8
end(): 11
```

## String字符串

```
String.join(",",names); //将names(ArrayList)中的元素以，拼接变为字符串
```
```
List<String> names = Arrays.asList(str.split(",")); //把str以，分开并且存入数组

```
```
google的第三方包GuavaUtil可以用Splitter.on(',');方法用逗号分开 并且去掉空和空格
```
## Code
```
import java.util.Locale;
import java.util.ResourceBundle;

class Main{
    public static void main(String[] args)  {
        Locale myLocale = Locale.getDefault();
        System.out.println(myLocale);
        ResourceBundle nameResourceBundle = ResourceBundle.getBundle("msg",myLocale);
        ResourceBundle  universityResourceBundle= ResourceBundle.getBundle("msg",myLocale);
        System.out.println(nameResourceBundle.getString("name"));
        System.out.println(universityResourceBundle.getString("university"));
        myLocale= new Locale("en","US");
        nameResourceBundle = ResourceBundle.getBundle("msg",myLocale);
        System.out.println(nameResourceBundle.getString("name"));
    }
}
```