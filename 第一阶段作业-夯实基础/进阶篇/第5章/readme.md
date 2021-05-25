# 第五章节
## 多进程的概念
- 当前的操作系统都是多任务OS
- 每个独立执行的任务就是一个进程
- OS将事件划分为多个时间片（事件很短）
- **每个时间片内将CPU分配给某一个任务 时间片结束 CPU将自动回收 再分配给另外任务** 如果是多核 多个进程任务可以并行 但是单个核上 多进程只能串行执行
### 多进程的优点
- 可以同时运行多个任务程序
- 程序因为IO堵塞的时候 可以释放CPU 让CPU为其他程序服务
- 当有多个CPU时 可以为多个程序同时服务
 - CPU不在提高频率 而是提高核数
 - 多核和并行程序才是提高程序性能的唯一办法
### 多进程的缺点
- 太笨重 不好管理
- 太笨重 不好切换

## 多线程的概念
- 一个程序可以包括多个子任务 可串/可并
- 每个子任务可以称为一个线程
- 如果一个子任务阻塞 程序可以将CPU调度另外一个子任务进行工作 这样CPU还是保留再本程序中 而不是调度到别的程序(进程)去 提高本程序所获得的CPU时间和利用率

## 多进程和多线程的对比
- 线程共享数据
- 线程通讯更高效
- 线程更轻量级 更容易切换
- 多个线程更容易管理

## 多线程实现

----------
**Java的四个主要接口：**
- Clonable 用于对象克隆
- Comparable 用于对象比较
- Serializable 用于对象序列化
- Runnable 用于对象线程化

----------

- java.lang.Thread 线程继承Thread方法 实现run方法
- java.lang.Runnable 线程实现Runnable接口 实现run方法

### 多线程启动
- **start方法 会自动以新线程调用run方法**
- **直接调用run方法 将变成串行执行（先执行完里面的代码再接着执行其他的）**
- 同一个线程 多次start会报错 只执行一次start方法
- **多个线程启动 其启动的先后顺序时随机的**
- 线程无需关闭 只要其run方法执行结束之后 自动关闭 
- main函数（线程）可能早于新线程结束 整个程序并不中止
- **整个程序终止是等所有的线程都中止（包括main函数线程）**
- 多个线程对象都start之后、睡眠之后恢复回来 哪一个先执行有JVM/操作系统来主导 程序员无法指定
- **Thread占据了父类的名额 不如Runnable方便**
- Thread实现Runnable
- Runnable启动的时候需要Thread支持
- Runnable更容易实现多线程中资源共享 Thread类要使用static才行 Runnable使用普通的变量就行
- **结论：建议实现runnable接口**

## 多线程信息共享
- 前面提到的方法都是**粗粒度**：子线程与子线程之间、main线程之间缺乏信息交流
- **细粒度**：线程之间有信息交流通讯
 - **通过共享变量达到信息贡献**
 - JDK原生库不支持发送消息(类似MPI并行库直接发送消息)

### 共享变量
- static变量
- 同一个Runnable类的普通成员变量
- 如果一个类继承了Thread类 那么他的信息共享只能通过static变量

## 多线程共享数据不一致问题
- **工作缓存副本**
	- 线程中修改数据是：线程1<-工作缓存1<-主存RAM
	- 这会导致线程1和线程2拿到的是同一个主存里的值在工作缓存中进行操作 就会导致数据重复（例如买票）
- **关键步骤缺乏加锁限制**
- i++ 并非原子性操作（最小单元不可再分）
	- 读取**主存i（正本）**到**工作缓存(副本)**中
	- 数据从**工作缓存（副本）**刷到**主存（正本）**中

- 变量副本问题的解决方案
	- **采用volatile关键字修饰变量**
	- **保证不同线程对共享变量操作时的可见性**

- 关键步骤加锁限制
	- 互斥：某一个线程运行一个**代码段（关键区）**，其他线程不能同时运行这个代码段
	- 同步：多个线程的运行 必须按照某一种规定的先后顺序来运行
	- 互斥是同步的一种特例

- 互斥的关键字是synchonized
	- **synchronized代码块/函数，只能一个线程进入**
	- synchronized加大性能负担(一次只能有一个线程进入访问) 但是使用便捷

## 多线程状态管理
- 粗粒度：子线程和子线程之间、main线程之间缺乏同步
- 细粒度：线程之间有同步协作
	- 等待
	- 通知/唤醒
	- 中止

- 线程状态
	- NEW 刚创建(new)
	- RUNNABLE 就绪态(start)
	- RUNNING 运行中(run)
	- BLOCK 阻塞(sleep)
		- 以下方法会使线程进入BLOCK状态
		- suspend
		- sleep
		- wait
	- TERMINATE 结束

- Thread的部分API已经废弃
	- 暂停和恢复suspend/resume
	- 消亡stop/destroy

- 线程阻塞/唤醒
	- sleep 时间一到自己会醒来
	- wait/notify/notifyAll 等待 需要别人来唤醒 / 唤醒
	- join 等待另一个线程结束
	- interrupt 向另一个线程发送中断信号 该线程收到信号 会出发InterruptedException(可解除阻塞) 并进行下一步处理

- 线程被动地暂停和中止
	- 依靠别的线程来拯救自己 **不推荐**
	- 没有及时释放资源

- 线程主动暂停和中止
	- 定期检测共享变量

## 多线程死锁
- 每个线程互相持有别人需要的锁(哲学家吃面问题)
- **预防死锁 对资源进行等级排序**

## 守护后台线程
- 普通线程的结束 是run方法运行结束
- 守护线程的结束 是run方法运行结束 或main函数结束(共命运)
- **守护线程永远不要访问资源 如文件或数据库
**
## 线程查看工具：jvisualvm

### Code
```
/*请分成6个线程，计算m到n的值(以1到100000000为例)的总和。要求每个线程计算的数字量之差不超过1.
* 我为了方便把范围改掉了  如果超出用BigInteger就好了
* */
class Main {
    //1~10000的变量
    volatile static int num = 1;
    //计算之后总的值
    volatile static int total = 0;

    public static void main(String[] args) {
        //这两部计算每个线程需要执行的次数
        int remain = 10000 / 6;
        int mod = 10000 % 6;


        for (int i = 0; i < 6; i++) {
            if (mod > 0) {
                //如果还有剩余计算的次数 那么就给新的线程加上
                new Thread(new CountThread(remain + 1), String.valueOf(i)).start();
                //加上之后别忘了减去剩余的总数
                mod--;
            } else
                //相应的 如果没有则就按照本身的执行次数来算
                new Thread(new CountThread(remain), String.valueOf(i)).start();
        }
    }

}

class add {
    //同步操作
    public static synchronized void add() {

        Main.total += Main.num;
        System.out.println(Thread.currentThread().getName() + " " + Main.total);
        Main.num += 1;

    }
}

class CountThread implements Runnable {
    //创建add对象 里面包含同步的计算方法
    add add = new add();
    volatile int cnt;

    //初始化需要执行的次数

    public CountThread(int cnt) {
        this.cnt = cnt;
    }

    @Override
    public void run() {
        while (cnt != 0) {
            if (cnt > 0) {
                //执行累加操作
                add.add();
                //输出还剩余的执行次数
                cnt--;
                System.out.println(Thread.currentThread().getName() + "还剩" + cnt + "次输出");
            }
        }
    }
}

```

# 多线程和并发编程（续）
## 并行计算
- 业务：任务多 数据量大
- 串行vs并行
	- 串行编程简单 并行编程困难
	- 单个计算核频率下降 计算核数增多 整体性能变高

- 并行困难（任务分配核执行过程**高度耦合**）
	- 如何控制粒度 切割任务
	- 如何分配任务给线程 监督线程执行过程

- 并行模式
	- 主从模式（master-slave）
	- Worker模式（worker-worker）

- Java并发编程
	- Thread/Runnable/Thread组管理
	- **Executor(本节重点)**
	- Fork-Join框架

## ThreadGroup

- 线程组管理
	- 线程组ThreadGroup
		- 线程的集合
		- 树形结构 大线程组可以包括小线程组
		- 可以通过enumerate方法遍历组内的线程 执行操作
		- 能够有效管理多个线程 但是**管理效率低**
		- 任务分配和执行过程**高度耦合**
		- 重复创建线程、关闭线程操作，**无法重用线程**
			- 线程和线程组内的线程都是new产生出来 但是**start一次之后 就不能再次使用**（即再次start） **new的代价很昂贵** 只运行一次 **性价比过低**

```
Thread[] threads = new Thread[threadGroup.activeCount()];
threadGroup.enumerate(threads);
```
- activeCount返回线程组中还处于active的线程数（估计数）
- enumerate 将线程组中active的线程拷贝到数组中
- interrupt 对线程组中所有的线程发出interrupt信号
- list 打印线程组中所有的线程信息

## Executor
- 从JDK 5开始提供Executor FrameWork **(java.util.concurrent.*)**
	- 分离任务的创建和执行者的创建
	- 线程重复利用(之所以重复利用是因为new的代价很大)

- 理解**共享线程池**的概念
	- 预设好的多个Thread 可弹性增加
	- 多次执行很多很小的任务
	- 任务创建和执行过程解耦
	- **程序员无需关心线程池执行任务过程**

- 主要类： ExecutorService ThreadPoolExecutor Future
	- Executor.newCachedThreadPool/newFixedThreadPool 创建线程池
	- ExecutorService 线程池服务
	- Callble 具体的逻辑对象
	- Future 返回结果

### 总结
- 掌握共享线程池原理
- 熟悉Executor框架 提高多线程执行效率

## Fork-Join(本节重点)
- Java 7提供另一种并行框架：分解、治理、合并**（分治编程）**
- 适用于整体任务量不好确定的场合**（最小任务可确定）**
- 关键类
	- ForkJoinPool 任务池
	- RecursiveAction
	- RecursiveTask

### 总结
- 了解Fork-Join和Executor的区别
- 控制任务分解粒度
- 熟悉Fork-Join框架 提高多线程执行效率

## 并发数据结构
- 常用的数据结构是线程不安全的
	- ArrayList HashMap HashSet 非同步的
	- 多个线程同时读写 可能会抛出异常或者数据错误

- 传统Vector HashTable等同步集合性能过差
- 并发数据结构：数据添加和删除
	- 阻塞式集合：当集合为空或者满时 等待
	- 非阻塞式集合：当集合为空或者满时 不等待 直接返回null或异常

- List
	- Vector 同步安全 写多读少
	- ArrayList 不安全
	- Collections.synchronizedList(List list) (将非同步的变为同步的)基于synchronized 效率差
	- CopyOnWriteArrayList **读多写少** 基于复制机制 非阻塞

- Set
	- HashSet 不安全
	- Collections.synchronizedSet(Set set) 基于synchronized 效率差
	- CopyOnWriteArrayList(基于CopyOnWriteArrayList实现) **读多写少** 非阻塞

- Map 
	- HashTable 同步安全 写多读少
	- HashMap 不安全
	- Collections.synchronizedMap(Map map) 基于synchronized 效率差
	- ConcurrentHashMap 读多写少 非阻塞

- Queue & Deque**（队列,JDK 1.5提出）**
	- ConcurrentLinkedQueue 非阻塞
	- ArrayBlockingQueue/LinkedBlockingQueue 阻塞
	- Deque是一个双向的队列两头都可以插入元素

## 线程协作
- Thread/Executor/Fork-Join
	- 线程启动，运行，结束
	- 线程之间缺少协作

- synchronized同步
	- 限定只有一个线程才能进入关键区
	- 简单粗暴 **性能损失大**

### Lock
- Lock也可以实现同步的效果
	- 实现更复杂的临界区结构
	- tryLock方法可以预判锁是否空闲（tryLock中包含两个操作 一个是看是否空闲 lock操作）
	- 允许分离读写的操作 多个读 一个写
	- 性能更好

- ReentrantLock类 可重入的锁
- ReentrantReadWriteLock类 可重入的读写锁
	- readLock是可以共享的 是可以多个线程共享的
	- writeLock是排他的 只能一个线程拥有
- lock和unlock函数

### Semaphore
- 信号量：本质上是一个计数器
- 计数器大于零可以使用 等于零不能使用
- 可以设置多个并发量 例如限制10个访问
- Semaphore
	- acquire获取
	- release释放

- 比Lock更进一步 可以控制多个同时访问关键区

### Latch
- 等待锁 是一个同步辅助类
- 用来同步执行任务的一个或者多个线程
- 不是用来保护临界区或者共享资源
- CountDownLatch
	- countDown() 计数减一
	- await() 等待latch变成0 变成零之后就结束等待

### Barrier 
- 集合点 也是一个同步辅助工具
- 允许多个线程在某一个点上进行同步
- CyclicBarrier
	- 构造函数是需要同步的线程数量
	- await等待其他线程 到达数量后就放行

### Phaser
- 允许执行并发多阶段任务 同步辅助类
- 在每一个阶段结束的位置对线程进行同步 当所有的线程都到达这步 在进行下一步
- Phaser
	- arrive()
	- arriveAndAwaitAdvance() 所有线程等待 并到规定的数量再进行

### Exchanger
- 允许再并发线程中互相交换消息
- 允许在2个线程中定义同步点 当两个线程都到达同步点 他们交换数据结构
- Exchanger
	- exchange() 线程双方互相交互数据
	- 交换数据是双向的

### 本节总结
- java.util.concurrent 包提供了许多并发编程的控制类
- 根据业务特点 使用正确的线程并发控制

## 定时任务
- 固定某一个时间点运行
- 某一个周期

### 简单定时器机制
- 设置计划任务 也就是在指定的时间开始执行某一个任务
- TimeTask 封装任务（本身就是一个实现了Runnable的类）
- Timer类 定时器

### Executor+定时器机制
- **ScheduledExcutorService**
	- 定时任务
	- 周期任务
- scheduleWithFixedDelay() 周期任务 固定延时 **是以一个任务结束时开始计时** period时间过去以后 立即执行
- scheduledAtFixedRate() 周期任务 固定速率 **以上一个任务开始的时间计时** period时间过去之后 检测上一个任务是否执行完毕 如果**上一个任务执行完毕** 则当前任务立即执行 否则等待上一个任务执行完毕之后立即执行 
- schedule() 在一个指定的时间之后运行

### Quartz (第三方库)
- Quartz是一个较为完善的任务调度框架
- 解决程序中Timer零散管理的问题
- 功能更加强大
	- Timer执行周期任务 如果中间某一次有异常 整个任务中止执行
	- Quartz执行周期任务 如果中间某一次有异常 不影响下次任务执行

- Trigger 相当于定义规则
- JobDetail 任务/作业

### Code
```
/**
 * 给定一个int数组，假设有10000个长度，里面放满1-100的随机整数。需要用串行循环计算、Executors框架和Fork-Join框架三种方法，实现查找并输出该数组中50的出现个数。
 *
 *
 *
 * 提交源程序和和执行结果截图。
 *
 * 预期执行结果如下(具体数量根据每个程序随机赋值决定)
 *
 * 串行搜索得到50的个数是5个。
 *
 * Executors搜索得到50的个数是5个。
 *
 * Fork-Join搜索得到50的个数是5个。
 */

import java.util.concurrent.*;

public class Main {
    static int[] array = new int[10000];

    public static void main(String[] args) throws ExecutionException, InterruptedException {

        int result;
//创建随机数组
        for (int i = 0; i < array.length; i++) {
            array[i] = (int) (Math.random() * 100 + 1);
        }
        result = getByForLoop(array);
        System.out.println("串行搜索得到50的个数是" + result);
        System.out.println("========================");
        result = getByExecutor();
        System.out.println("Executors搜索得到50的个数是" + result);
        System.out.println("========================");
        result = getByForkJoin().get();
        System.out.println("Fork-Join搜索得到50的个数是" + result);

    }

    //    用串行循环输出50的个数
    static int getByForLoop(int[] array) {
        int cnt = 0;
        for (int i : array) {
            if (i == 50) {
                cnt++;
            }
        }
        return cnt;
    }

    //用Executor框架分成十个线程 每个线程执行1000个判断、计数
    static int getByExecutor() {
        ThreadPoolExecutor executor = (ThreadPoolExecutor) Executors.newFixedThreadPool(10);
        ExecutorCountTask executorCountTask;
        Future<Integer> result;
        int total = 0;
        for (int i = 0; i < 10; i++) {
            executorCountTask = new ExecutorCountTask(i * 1000, (i + 1) * 1000);
            result = executor.submit(executorCountTask);
            try {
                total += result.get();
            } catch (InterruptedException | ExecutionException e) {
                e.printStackTrace();
            }
        }
        executor.shutdown();
        return total;
    }

    //用ForkJoin框架进行递归计数 最小的模块是1000
    private static ForkJoinTask<Integer> getByForkJoin() {
        ForkJoinPool forkJoinPool = new ForkJoinPool();
        ForkJoinCountTask forkJoinCountTask = new ForkJoinCountTask(0, array.length);
        ForkJoinTask<Integer> cnt = forkJoinPool.submit(forkJoinCountTask);
        forkJoinPool.shutdown();
        return cnt;
    }
}

//创建Executor的计数任务
class ExecutorCountTask implements Callable<Integer> {

    int start, end;

    public ExecutorCountTask(int start, int end) {
        this.start = start;
        this.end = end;
    }

    @Override
    public Integer call() {
        Integer cnt = 0;
        for (int i = start; i < end; i++) {
            if (Main.array[i] == 50) {
                cnt++;
            }
        }
        return cnt;

    }
}

//ForkJoin的计数任务
class ForkJoinCountTask extends RecursiveTask<Integer> {

    int start, end;
    final int minModule = 1000;
    int cnt = 0;

    public ForkJoinCountTask(int start, int end) {
        this.start = start;
        this.end = end;
    }

    @Override
    protected Integer compute() {
        if ((end - start) <= minModule) {
            for (int i = start; i < end; i++) {
                if (Main.array[i] == 50)
                    cnt++;
            }
        } else {
//            如果不是在最小模块之中的 那么就再次细分
            ForkJoinCountTask forkJoinCountTask1 = new ForkJoinCountTask(start, start + (end - start) / 2);
            ForkJoinCountTask forkJoinCountTask2 = new ForkJoinCountTask(start + (end - start) / 2, end);
//            JDK内部自带的方法 开始这两个任务
            invokeAll(forkJoinCountTask1, forkJoinCountTask2);
//            join方法返回计算之后的值 如果其中某一个阻塞了那么就阻塞了
            cnt += forkJoinCountTask1.join();
            cnt += forkJoinCountTask2.join();
        }
        return cnt;
    }
}

```