# 第七章节
## 数据库和SQL
### DB（1）
- DB: Database = Data + Base
- 数据库：数据+库 存放数据的库（场所）
- 数据：规范 半规范 不规范数据
- 库
	- 一个空间 一个场所
	- 停车场 钱包 教室
	- 文件

### DB（2）
- DB：保存数据的地方
	- **数据安全**
	- 存取效率
	- 性价比高

### DB分类（1）
- DB：文件集合 类似doc docx文件
- DBMS：Database Management System(类似Office/WPS)
	- 操纵和管理数据库的软件 可建立、使用和维护数据库

- DB种类
	- 文本文件/二进制文件
	- Xls文件
	- Access（包含在office里面 收费 只能运行在Windows上 32和64位） 
	- Mysql/Postgresql/Berkely DB（免费 但是也有收费版 多平台 32和64位）
	- SQL Server(收费 只能运行Windows 32和64位 中文文档)
	- Oracle/DB2(收费 全平台 32和64 英文文档 也有免费版 但也有CPU和内存限制)
	- SQLite（免费 手机上使用）

### 表
- 表：table 实体
	- 列：属性，字段
	- 行：记录，元组tuple，数据

- 数据值域：
	- 数据的取值范围

- 字段类型：
	- int:整数-2147483648~2147483647 四个字节
	- double: 小数 8个字节
	- datetime：时间 7个字节
	- varchar：字符串 可变字节

### SQL（1）简介
- 结构化查询语言 简称SQL
	- 是一种特殊目的的编程语言　是一种数据库查询和程序设计语言　用于存取数据以及查询　更新和管理关系数据库系统　同时也是数据库脚本文件的拓展名

- SQL标准
	- SQL-86/SQL-89/SQL-92
	- SQL:1999/SQL:2003/SQL:2008/SQL:2011/SQL:2016
	- 基础的部分 所有的标准都是一样的
	- 标准仅仅是标准 每个厂商的数据库实现可能有一些不一致

### SQL（2） 常用语句
- create table t1(a int,b varchar(20));
	- 创建了一张表 叫做t1 这个表有两列 a、b

- insert into t1(a,b) values(1,'abc');
	- 往表里放值

- select a from t1;
	- 选择语句 把t1表的值都取出来

- select a,b from t1 where a>1;
	- 取出ab两列 条件范围是a>1

- delete from t1 where a=10 and b='ab';
	- 删除a是10同时b是ab的的数据

- update t1 set a=2,b='cd' where a=1 and b='ab';
	- 修改数据

- drop table t1;
	- 删除表t1

## JDBC
- Java和数据库是平行的两套系统
- Java和数据库的连接方法
	- Native API**（不能跨平台）**
	- ODBC/JDBC-ODBC**（效率很差 也无法跨平台）**
	- JDBC（主流）：Java Database Connectivity
		- JDBC1 JDK1.1
		- JDBC2 JDK1.3~1.4
		- JDBC3 JDK5
		- JDBC4 JDK6 (JDBC4.1 JDK7  JDBC4.2 JDK8)

### Java SQL操作类库
- java.sql.\*,javax.sql.\*
- 根据数据库版本和JDBC版本合理选择
- 一般数据库发行包都会提供jar包 同时也要注意区分32位和64位（数据库分32位和64位 JDK也分）
- 连接字符串**（样例）**

```
jdbc:oracle:thin:@127.0.0.1:1521:dbname
jdbc:mysql://localhost:3306/mydb
jdbc:sqlserver://localhost:1433; DatabaseName=dbname

```
- 构建链接
	- 注册驱动 寻找材质 class.forName("...")
	- 确定对岸目标 建桥Connection

- 执行操作（派个人过桥 提着篮子 去拿数据）
	- Statement(执行者)
	- ResultSet(结果集)

- 释放连接（拆桥）connection.close()

### Statement
- Statement执行者类
	- 使用executeQuery()执行select语句 返回结果放在ResultSet
	- 使用executeUpdate()执行insert/update/delete 返回修改的行数
	- 一个Statement对象一次只能执行一个命令

- ResultSet结果对象
	- next()判断是否还有下一条记录
	- getInt/getString/getDouble/……获取单元格里面的数据
		- 可以按索引位置 可以按照列名

### Navicat 
- 是一个支持MySQL、SQL Server、Oracle等主流数据库的客户端工具
- 详见[http://www.navicat.com.cn](http://www.navicat.com.cn)

### 注意事项
- ResultSet不能多个做笛卡尔积连接 例如


```
//两个ResultSet做笛卡尔积
while(resultSet1.next){
	while(resultSet2.next){
		//这个循环次数为rs1的条数*rs2的条数
	}

}

```
- ResultSet**最好不要超过百条** 否则及其影响性能
- ResultSet也**不是一口气加载所有**的select结果数据
- **Connection很贵 需要及时close**
- Connection使用的**jar包和数据库**要匹配

## JDBC高级操作
### 事务
- 数据库事务 Database Transaction
- 作为单个逻辑工作单元执行的一系列操作 **要么完全执行 要么完全不执行**
- 事务必须满足所谓的ACID（原子性　一致性　隔离性　持久性）属性
- 事务是数据库运行中的逻辑工作单位　由DBMS中的事务处理

### JDBC事务
- 关闭自动提交 实现多语句同一事务（JDBC默认自动提交）
- connection.setAutoCommit(false);
- connection.commit();提交事务
- connction.rollback();回滚事务
- 保存点机制
	- connection.setSavepoint()
	- connection.roolback(Savepoint)

### PreparedStatement
- Java提供PreparedStatement 更为安全执行SQL
- 和Statement区别是用“？”代替字符串拼接
- 使用setXXX(int,Object)的函数来实现对于？的替换
	- 注意：不需要考虑字符串两侧的单引号
	- 参数赋值 清晰明了 拒绝拼接错误

- 提供addBatch批量更新功能
- Select语句一样用ResultSet接收结果
- **使用PreparedStatement的好处：**
	- 防止注入攻击
	- 防止繁琐的字符串拼接和错误
	- 直接设置对象而不需要转换为字符串
	- PreparedStatement使用预编译速度相对Statement快很多

### ResultSetMetaData
- ResultSet可以来承载所有的select语句返回的结果集
- ResultSetMetaData来获取ResultSet返回的属性（如每一行的名字类型等）
	- getColumnCount() 返回结果的列数
	- getColumClassName(i) 返回第i列的数据的Java类名
	- getColumnTypeName(i) 返回第i列的数据库类型名称
	- getColumnType(i) 返回第i列的SQL类型
	
使用ResultSetMetaData解析ResultSet

## 数据库连接池
###  享元模式（1）
- Connection是Java和数据库两个平行系统的桥梁
- **桥梁构建不易 成本很高 单次使用成本昂贵**
- 运用共享技术来实现数据库连接池（享元模式）
	- 降低系统中数据库连接Connection对象的数量
	- 降低数据库服务器的连接响应消耗
	- 提高Connection获取的响应速度

### 享元模式（2）
- 享元模式,FLyweight Pattern
	- 经典23个设计模式的一种 属于结构型的模式
	- 一个系统中有大量相同的对象
		- 由于这类对象的大量使用 会造成内存的耗费 可以使用享元模式来减少系统中的对象数量

### 数据库连接池
- 理解池Pool的概念
	- 初始数、最大数、增量、超时时间等参数

- 常用的数据库连接池
	- DBCP（Apache [http://commons.apache.org/](http://commons.apache.org/)，性能较差）
	- C3P0([https://www.mchange.com/projects/c3p0/](https://www.mchange.com/projects/c3p0/))
	- Druid(Alibaba,[https://github.com/alibaba/druid](https://github.com/alibaba/druid))
	- ……

### c3P0连接池

```
<c3p0-config>
    <!--使用默认的配置读取数据库连接池对象 -->
    <default-config>
        <!--  连接参数 -->
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/website?serverTimezone=Asia/Shanghai</property>
        <property name="user">root</property>
        <property name="password">123456</property>
 
        <!-- 连接池参数 -->
        <!--初始化申请的连接数量-->
        <property name="initialPoolSize">5</property>
        <!--最大的连接数量-->
        <property name="maxPoolSize">10</property>
        <!--超时时间-->
        <property name="checkoutTimeout">3000</property>
    </default-config>
 

</c3p0-config>
```

- driverclass驱动class 这里为mysql的驱动
- jdbcUrl jdbc连接
- user password 数据库用户名密码
- initialPoolSize 初始数量：一开始创建多少条连接
- maxPoolSize 最大数：最多有多少条连接
- acquireIncrement增量： 用完每次增加多少个
- maxIdleTime最大空闲时间：超出的连接就会被放弃

## Code
> 假设数据库有一张表t_mail (id, from, to, subject, content), 里面存储着具体的邮件发件人、收件人、标题和内容。采用Druid连接池，读取id为1的记录，并基于Java Mail将该邮件发送出来。

```
import factory.DruidFactory;
import factory.SendEmail;

import javax.mail.MessagingException;
import javax.mail.NoSuchProviderException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class Main {

    private static String from;
    private static String to;
    private static String content;
    private static String subject;
    private static int id;

    public static void main(String[] args) throws SQLException, MessagingException {
        Connection connection = null;

        try {
            connection = DruidFactory.getConnection();
            String sql = "select * from t_mail where _id = 1";
            PreparedStatement preparedStatement = connection.prepareStatement(sql);
            ResultSet resultSet = preparedStatement.executeQuery();
            while (resultSet.next()) {
                id = resultSet.getInt("_id");
                from = resultSet.getString("_from");
                to = resultSet.getString("_to");
                subject = resultSet.getString("_subject");
                content = resultSet.getString("_content");
//                System.out.println(id+from+to+subject+content);
            }
            resultSet.close();
            preparedStatement.close();
        } catch (SQLException throwable) {
            throwable.printStackTrace();
        } finally {
            if (connection != null)
                connection.close();
        }
        SendEmail sendEmail = new SendEmail(from,to,subject,content);
        sendEmail.init();
        sendEmail.sendMessage();
        sendEmail.close();
    }

}

```

```
package factory;

import javax.mail.Message;
import javax.mail.MessagingException;
import javax.mail.Session;
import javax.mail.internet.AddressException;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;
import java.util.Date;
import java.util.Properties;

public class TextMessage {
    static String from,to,subject,content;

    public TextMessage(String from, String to,String subject,String content) {
        this.from = from;
        this.to = to;
        this.subject = subject;
        this.content = content;
    }

    public static Message generate() throws MessagingException {
        //创建Session
        Session session = Session.getDefaultInstance(new Properties());
        MimeMessage message = new MimeMessage(session);
        message.setFrom(new InternetAddress(from));
        message.setRecipients(Message.RecipientType.TO,InternetAddress.parse(to));
        //发送日期
        message.setSentDate(new Date());
        //设置邮件主题
        message.setSubject(subject);
        //设置文本内容
        message.setText(content);
        //保存邮件内容
        message.saveChanges();
        return message;
    }
}

```

```
package factory;

import com.alibaba.druid.pool.DruidDataSource;

import java.sql.Connection;
import java.sql.SQLException;

public class DruidFactory {
    public static DruidDataSource dataSource= null;

    public static void init() {
        dataSource = new DruidDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUsername("root");
        dataSource.setPassword("123456");
        dataSource.setUrl("jdbc:mysql://127.0.0.1:3306/db_mail");
        dataSource.setInitialSize(5);
        dataSource.setMinIdle(1);
        dataSource.setMaxActive(10);
    }

    public static Connection getConnection() throws SQLException {
        if (null==dataSource){
            init();
        }
        return dataSource.getConnection();
    }
}

```

```
package factory;

import javax.mail.*;
import java.util.Properties;

public class SendEmail {
    private Session session;
    private Transport transport;
    String username;
    String password = "011209Sc";
    //SMTP服务器（端口465或587）
    String SMTPserver = "smtp.qq.com";
    String to;
    String subject,content;

    public SendEmail(String username, String to,String subject,String content) {
        this.username = username;
        this.to = to;
        this.subject = subject;
        this.content = content;

    }

    public void init() throws NoSuchProviderException {
        Properties properties = new Properties();
        properties.put("mail.transport.protocol", "smtp");
        properties.put("mail.smtp.class", "com.sun.mail.smtp.SMTPTransport");
        properties.put("mail.smtp.host", SMTPserver);
        properties.put("mail.smtp,port", "25");//按照mooc上面的教程是25 不知道这个是本机的还是网络上的 如果有需要在修改为qq.com的
        properties.put("mail.smtp.auth", "true");//SMTP服务器需要身份验证

//        创建Session类
        session = Session.getInstance(properties, new Authenticator() {
            @Override
            protected PasswordAuthentication getPasswordAuthentication() {
                return new PasswordAuthentication(username, password);
            }
        });
        session.setDebug(true);//输出跟踪日志

        transport = session.getTransport();

    }

    public void sendMessage() throws MessagingException {
        TextMessage textMessage = new TextMessage(username,to,subject,content);
        Message message = TextMessage.generate();
        transport.connect("smtp.qq.com","1471157489@qq.com","aionjdcympkujadd");
        transport.sendMessage(message, message.getAllRecipients());
        System.out.println("邮件发送成功");
    }
    public void close() throws MessagingException {
        transport.close();
    }
}


```