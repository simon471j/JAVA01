# 第十一章节
## 文件概述
- 文件系统是由操作系统管理的
- 文件系统和Java进程是**平行**的，是两套系统
- 系统文件是由文件夹和文件递归组合而成的
- 文件目录分隔符
1. Linux/Unix 用/隔开
2. Windows用\隔开 涉及到转义 在程序中需要用**/或\\\\**代替

- 文件包括文件里面的内容和文件的基本属性
- 文件的基本属性：名称、大小、拓展名、修改时间等

## 文件类File
- java.io.File是文件和目录的重要类(JDK6以前是唯一)
- File类与OS无关，但会受到OS的权限限制
- File**不涉及具体的文件内容**，只涉及属性

## Java NIO
- Java 7 提出的NIO包是新的文件系统类

## Java IO包
- Java读写文件只能以数据流的形式进行读写
- 字节：byte，8bit，最基础的储存单位
- 字符：a，10000，我，の
- 数据类型：3，5，25，abcdef
- 文件是以字节保存，因此程序变量保存到文件需要转化

### 节点类
- 直接操作文件
- InputStream OutputStream
- FileInputStream FileOutputStream
- FileReader FileWriter

### 包装类

----------


#### 转换类
- 字符到字节之间的转换
- InputStreamReader 文件读取时字节，转化到Java能理解的字符
- OutputStreamWriter Java将字符转化为字节处输入到文件中

#### 装饰类
- 装饰节点类
- DataInputStream DataOutputStream 封装数据流
- BufferedInputStream BufferOutputStream 缓存字节流
- BufferedReader BufferedWriter 缓存字符流

## 文本文件读写
- 输出：数据从Java到文件中，写操作
- 输入：数据从文件到Java中，读操作

### 写文件
- 先创建文件 写入数据 关闭文件
- FileOutputStream OutputStreamWriter BufferedWriter 
- BufferWriter： write newLine
- try-resource语句 自动关闭资源
- 关闭最外层的数据流 将会把其上的所有的数据流关闭
```
//用try-catch包裹
BufferedWriter in = new BufferedWriter(new OutputStreamWriter(new FileOutputStream("c:/temp/abc.text")))
```

### 读文件
- 先打开文件 逐行读入数据 关闭文件
- FileInputStream BufferedReader InputStreamReader
- BufferReader:readLine
- try-resource语句 自动关闭资源
- 关闭最外层的数据流 将会把其上的所有的数据流关闭
```
//用try-catch包裹
BufferedReader in = new BufferedReader(new InputStreamReader(new FileInputStream("c:/temp/abc.text")))
```

### 二进制文件读写
- 二进制文件
1. 狭义上说 采用字节编码 非字符编码的文件
2. 广义上说 一切文件都是二进制文件
3. 用记事本等无法打开阅读
 
- 二进制文件读写
1. 输出数据到文件中
2. 从文件中读取数据

#### 写文件
- 先创建文件 写入数据 关闭文件
- FileOutputStream BufferedOutputStream DataOutputStream
- try-resource语句 自动关闭资源
- 关闭最外层的数据流 将会把其上的所有的数据流关闭

#### 读文件
- 先创建文件 写入数据 关闭文件
- FileInputStream BufferedInputStream DataInputStream
- try-resource语句 自动关闭资源
- 关闭最外层的数据流 将会把其上的所有的数据流关闭

```
DataInputStream dis = new DataInputStream(new BufferedInputStream(new FileInputStream("c:/temp/def.dat")))
```

#### Zip文件读写
```
public static void main(String[] args) throws IOException {
        String path = "F:\\*******\\201707\\78641695079026649.zip";
        ZipFile zf = new ZipFile(path);
        InputStream in = new BufferedInputStream(new FileInputStream(path));
        Charset gbk = Charset.forName("gbk");
        ZipInputStream zin = new ZipInputStream(in,gbk);
        ZipEntry ze;
        while((ze = zin.getNextEntry()) != null){
            if(ze.toString().endsWith("txt")){
                BufferedReader br = new BufferedReader(
                        new InputStreamReader(zf.getInputStream(ze)));
                String line;
                while((line = br.readLine()) != null){
                    System.out.println(line.toString());
                }
                br.close();
            }
            System.out.println();
        }
        zin.closeEntry();

```

## 代码
```
//读取以下的a.txt，统计相同单词的次数，最后按照次数的大小降序排列，输出到b.txt中。
//
//        a.txt (===算分隔符，不算单词，不考虑单词大小写)
//        ==============
//        hello world
//        hello
//        java world   hello
//        ==============
//        b.txt
//        ============
//        hello,3
//        world,2
//        java,1
//        ============
//        提示：需要用到的内容有文本文件读和写操作。还要加上数据结构，有ArrayList, HashMap,Comparable等。

import java.io.*;
import java.util.*;

class Main {
    public static void main(String[] args) {
        HashMap<String, Integer> map = map = new HashMap<>();

        try {
            BufferedReader br = new BufferedReader(new FileReader("d:/test.txt"));
            String temp;
            while ((temp = br.readLine()) != null) {
                String[] tempArr = temp.split(" ");
                for (String str : tempArr) {
                    map.put(str, map.getOrDefault(str, 0) + 1);		//计数
                }
                map.remove("");		//移除为空的key

            }
            ArrayList<Record> records = new ArrayList<>();
            for (Map.Entry<String, Integer> entry : map.entrySet()) {
                records.add(new Record(entry));
            }
            Collections.sort(records);
            for (Record record : records
            ) {
                System.out.println(record);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

	
    static class word {
        public String getKey() {
            return key;
        }

        public void setKey(String key) {
            this.key = key;
        }

        public int getValue() {
            return value;
        }

        public void setValue(int value) {
            this.value = value;
        }

        private String key;
        private int value;
    }

	//注意要继承word类
    static class Record extends word implements Comparable<word> {

        public Record(Map.Entry<String, Integer> entry) {
            setKey(entry.getKey());
            setValue(entry.getValue());
        }

		//重写Comparable接口 比较大小 为排序做准备
        @Override
        public int compareTo(word o) {
            if (o.getValue() < getValue())
                return 1;
            else
                return -1;
        }

		//重写toString方法
        @Override
        public String toString() {
            return getKey() + " " + getValue();
        }
    }
}
```
## 期末作业程序题
### 题目
- **题目内容**：

通常来说动物的物理年龄与其生理年龄是不匹配的，如果要严格的根据动物的生理年龄排序需要有一个具体的参照（例如人）。下面列出了狗和人以及猫和人的年龄对照表。


人|狗|猫
----|----|----
1|18|15
2|23|24
3|23|28
4|32|32
5|36|36
6|40|40
7|45|44
8|50|48
9|55|52
10|60|56

- 例如，1岁的狗与1岁的猫相比，1岁的狗生理年龄较大。请利用继承机制与Comparable接口实现不同狗与猎豹对象的排序

```

import java.util.Arrays;
import java.util.Scanner;

class Animal {

    int age;//动物年龄


    int mage; //映射到人的年龄

    String name; //名字

    static int[] dog = {0, 18, 23, 28, 32, 36, 40, 45, 50, 55, 60};

    static int[] cat = {0, 15, 24, 28, 32, 36, 40, 44, 48, 52, 56};

    @Override
    public String toString() {
        return name + " " + age;
    }
}

class Dog extends Animal implements Comparable<Animal> {
    public Dog(String name, int age) {
        super.name = name;
        super.mage = dog[age];
        super.age = age;
    }

    @Override
    public int compareTo(Animal o) {
        if (mage < o.mage)
            return -1;
        else
            return 1;
    }

    //实现你的构造函数与Comparable中的接口

}

class Cat extends Animal implements Comparable<Animal> {
    public Cat(String name, int age) {
        super.name = name;
        super.mage = cat[age];
        super.age = age;
    }

    @Override
    public int compareTo(Animal o) {
        if (mage < o.mage)
            return -1;
        else
            return 1;
    }

    //实现你的构造函数与Comparable中的接口

}


public class Main {


    public static void main(String[] args) {

        Animal[] as = new Animal[4];


        // 初始化

        Scanner sc = new Scanner(System.in);


        // 利用hasNextXXX()判断是否还有下一输入项

        if (sc.hasNext()) {

            as[0] = new Dog("dog1", sc.nextInt());

        }

        if (sc.hasNext()) {

            as[1] = new Cat("cat1", sc.nextInt());

        }

        if (sc.hasNext()) {

            as[2] = new Dog("dog2", sc.nextInt());

        }

        if (sc.hasNext()) {

            as[3] = new Cat("cat2", sc.nextInt());

        }


        // 请补充排序
        Arrays.sort(as);

        // 请补充升序输出结果

        for (Animal animal : as) {
            System.out.println(animal);
        }

    }

}
```
