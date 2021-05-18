#第六章节
##static
- static可以修饰：属性，方法，代码段，内部类（静态内部类或嵌套内部类）
- 有static修饰：静态变量
- 无static修饰：实例变量
- 只在类第一次被加载的时候调用，这段代码只运行一次
- 执行顺序：static块>匿名块>构造函数
- 可以直接用类名引用，无需new

##final
- final可以修饰：属性，方法，类，局部变量（方法中的变量）
- 利用关键字final对一个变量赋值的时候，表示这个变量只能被赋值一次，一旦被赋值以后将不能再进行更改
- 如果是基本型的变量，不能修改其值
- 如果是对象实例，不能修改它的指针
- final类不能被子类继承
- final方法不能被子类改写
- final变量不能被修改值，对象类型不能被修改指针

##单例模式
- 限定某一个类在整个程序运行过程中，之恶能保留一个实例在内存空间
- 属于创建型模式类型
- 采用static来共享对象实例
- 采用private构造函数防止new操作

###饿汉模式
```
public class Singleton{
    //类加载时就初始化
    private static final Singleton instance = new Singleton();
    
    private Singleton(){}
 
    public static Singleton getInstance(){
        return instance;
    }
}
```

###懒汉模式

```

public class Singleton {
    
    private static Singleton instance;
 
     //外界不能造对象 把无参构造方法私有
    private Singleton (){}
 
    //通过公共的方式对外提供 public修饰
    public static Singleton getInstance() {
     if (instance == null) {
         //类本身要造一个  调用构造方法即可
         instance = new Singleton();
     }
     return instance;
    }
}

```