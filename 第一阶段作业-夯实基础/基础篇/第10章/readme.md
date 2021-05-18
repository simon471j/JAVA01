#第十章节
##数组
- 概念：同一种类型数据的集合。其实数组就是一个容器。
- 格式1：元素类型[] 数组名 = new 元素类型[元素个数或数组长度];
- 格式2：元素类型[] 数组名 = new 元素类型[]{元素，元素，……}


- 数组的遍历
1. fori循环
2. foreach循环

##JCF(Java Collections Framework) 集合框架
- 容器 能存放数据的空间结构
- 容器框架 为表示和操作容器而规定的一种标准体系结构
1. 对外的接口 容器中所能存放的抽象数据类型
2. 接口的实现 可复用的数据结构
3. 算法 对数据的查找和排序

- 容器框架的优点：提高数据存取效率，避免程序员重复劳动

## 列表
1. List
2. ArrayList 非同步的
3. LinkedList 非同步的



- 集合
1. Set
2. HashSet
3. TreeSet
4. LinkedHashSet

- 映射
1. Map
2. HashMap
3. TreeSet
4. LinkedHashMap

### List
- 有序的Collection
- 允许重复的元素

####LinkedList
1. 以双向链表实现的列表，不支持同步
2. 可被当作堆栈、队列和双端队列进行操作
3. 顺序访问高效，随机访问较差，中间插入和删除高效
4. 适用于经常变化的数据

##集合
- HashSet 基于散列函数的集合 无序 不支持同步
- TreeSet 基于树结构的集合 可排序的 不支持同步
- LinkedHashSet 基于散列函数和双向链表的集合 可排序的 不支持同步
- 确定性
- 无异性
- 无序性

###HashSet
- 基于HashMap实现的 可容纳null 不支持同步

###LinkedHashSet
- 继承HashSet
- 也是基于HashMap实现的，可以容纳null
- 不支持同步
- 方法和HashSet基本一致
- 通过一个双向链表维护插入顺序

###TreeSet
- 基于TreeMap实现的 **不可以**容纳null元素 不支持同步

##映射Map
- 键值对
- HashTable 同步 慢 数据量小
- HashMap 不支持同步 快 数据量大
- Properties 同步 文件形式 数据量小

###LinkedHashMap
- 基于双向链表的维持插入顺序的HashMap

###TreeMap
- 基于红黑树的Map 可以根据key的自然排序或者compareTo方法进行排序输出

##Code
- 注意Comparable接口的使用
- 实现Comparable<Currency>接口 并且继承了Currency 那么就继承了Currency中的属性 则不需要再自定义名字和值 直接用Currency提供的get和set方法 就可以进行compareTo方法的比较


```
import java.util.Arrays;

import java.util.Scanner;


class Currency {

    private String name;        //货币名称

    private int originalValue;  //原始值

    private int value;          //转换为人民币后的值


    public String getName() {

        return name;

    }

    public void setName(String name) {

        this.name = name;

    }


    public int getOriginalValue() {

        return originalValue;

    }

    public void setOriginalValue(int originalValue) {

        this.originalValue = originalValue;

    }


    public int getValue() {

        return value;

    }

    public void setValue(int value) {

        this.value = value;

    }

}


class HKD extends Currency implements Comparable<Currency> {


    public HKD(int a) {
        setValue(a);
    }

    @Override
    public String toString() {
        return getName() + getValue();
    }

    @Override
    public int compareTo(Currency o) {
        if (getValue() < o.getValue())
            return 1;
        else
            return -1;

    }


    // 实现你的构造函数与Comparable中的接口

}


class USD extends Currency implements Comparable<Currency> {


    public USD(int b) {
        setValue(b);
    }

    @Override
    public int compareTo(Currency o) {
        if (getValue() < o.getValue())
            return 1;
        else
            return -1;

    }

    @Override
    public String toString() {
        return getName() + getValue();
    }
// 实现你的构造函数与Comparable中的接口

}


class EUR extends Currency implements Comparable<Currency> {


    @Override
    public String toString() {
        return getName() + getValue();
    }

    public EUR(int c) {
        setValue(c);
    }


    @Override
    public int compareTo(Currency o) {
        if (getValue() < o.getValue())
            return 1;
        else
            return -1;

    }

    // 实现你的构造函数与Comparable中的接口

}


public class Main {

    public static void main(String[] args) {

        Currency[] cs = new Currency[3];

        //初始化

        Scanner sc = new Scanner(System.in);

        //利用hasNextXXX()判断是否还有下一输入项

        int a = 0;

        int b = 0;

        int c = 0;

        if (sc.hasNext()) {

            a = sc.nextInt();

            cs[0] = new HKD(a);

        }


        if (sc.hasNext()) {

            b = sc.nextInt();

            cs[1] = new USD(b);

        }


        if (sc.hasNext()) {

            c = sc.nextInt();

            cs[2] = new EUR(c);

        }


        //初始化结束


        //请补充排序
        Arrays.sort(cs);


        //请补充输出结果
        for (Currency value : cs) {
            System.out.println(value);

        }
    }

}
```