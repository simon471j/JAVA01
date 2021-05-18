#第八章节

##Java类库
- 包名以java开始的包是Java核心包
- 包名以Javax开始的包是Java拓展包

###数字相关类
- 整数 Short,Int,Long
- 浮点数 Float,Double
- 大整数 BigInteger(大整数),BigDecimal(大浮点数)
- 随机数类 Random
- 工具类 Math

###字符串相关类
- String Java中是由频率最高的类

####可变字符串
- StringBuffer (字符串加减，同步，性能好)
- StringBuilder(字符串加减，不同步，性能更好)

###时间类
- java.util.Date（基本废弃）
- java.sql.Date（和数据库对应的时间类）
- Calendar是目前程序中最常用的，但是是抽象类

####Calendar
 方法名|具体描述
 ----|----
get(int firld)|返回给指定日历字段的值
set(int firld,int value)|将给定的日历字段设置为给定值
set(int firld,int value)|将给定的日历字段设置为给定值
add(int firld,int amount)|根据日历的规则，为给定的日历字段添加或减去指定的时间向量
getTime()|返回一个表示此Calendar时间值（从历元到现在的毫秒偏移量）的Date对象

####java.time
- 新的Java日期，时间API的基础包

###格式化类
>java.test.Format
>>NumberFormat:数字格式化，抽象类
>>>eg. 1234567 -> 1,234,567
>
>>MessageFormat:字符串格式化
>>>eg. "Hello{1}"格式化为Hello World
>
>>DateFormat:日期/时间格式化，抽象类
>>>eg. 将当前时间转化为YYYY-MM-DD HH24: MI:SS

```

import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入十八位的身份证号码");
        String id = sc.next();
        String year = id.substring(6, 10);
        String month = id.substring(10, 12);
        String day = id.substring(12, 14);
        System.out.println(year + "-" + month + "-" + day);
    }

}
```

```

import java.math.BigInteger;
import java.util.Locale;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入十八位的身份证号码");
        String id = sc.next();
        String year = id.substring(6, 10);
        String month = id.substring(10, 12);
        String day = id.substring(12, 14);
        int[] testNum = {7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2};
        //7－9－10－5－8－4－2－1－6－3－7－9－10－5－8－4－2
        try {
            int total = 0;
            for (int i = 0; i < 17; i++) {
                total += id.charAt(i) * testNum[i];
            }
            int result = total % 11;
            //余数只可能有0－1－2－3－4－5－6－7－8－9－10这11个数字。其分别对应的最后一位身份证的号码为1－0－X－9－8－7－6－5－4－3－2。
            boolean legal = false;
            System.out.println(result);
            switch (result) {
                case 0:
                    if ("1".equals(id.substring(17)))
                        legal = true;
                case 1:
                    if ("0".equals(id.substring(17)))
                        legal = true;
                case 2:
                    if ("x".equals(id.substring(17).toLowerCase()))
                        legal = true;
                case 3:
                    if ("9".equals(id.substring(17)))
                        legal = true;
                case 4:
                    if ("8".equals(id.substring(17)))
                        legal = true;
                case 5:
                    if ("7".equals(id.substring(17)))
                        legal = true;
                case 6:
                    if ("6".equals(id.substring(17)))
                        legal = true;
                case 7:
                    if ("5".equals(id.substring(17)))
                        legal = true;
                case 8:
                    if ("4".equals(id.substring(17)))
                        legal = true;
                case 9:
                    if ("3".equals(id.substring(17)))
                        legal = true;
                case 10:
                    if ("2".equals(id.substring(17)))
                        legal = true;
                default:

            }
            if (legal) {
                System.out.println(year + "-" + month + "-" + day);
            } else
                System.out.println("0000 - 00 - 00");
        } catch (Exception e) {
            e.printStackTrace();
        }


    }

}
```