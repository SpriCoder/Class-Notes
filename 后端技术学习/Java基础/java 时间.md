Java 时间
---

# 1. Date时间类
1. 来源：java.util.Date
2. 表示一个特定的瞬间，精确到毫秒

## 1.1. 构造方法
1. `Date()`:分配一个Date对象，并初始化此对象为当前的日期和时间精确到毫秒
2. `Date(Long date)`:分配Date对象并初始化此对象，以表示从标准基准时间，即1970年1月1日00:00:00 GMT以指定毫秒数

## 1.2. 其他方法

返回值|函数名|函数意义
--|--|--
Boolean|after(Date when)|测试该日期是否在指定日期以后
Boolean|before(Date when)|测试该日期是否在指定日期之前
Boolean|equals(Date when)|比较两个日期的相等性，重写的方法
Long|getTime()|返回自1970年1月1日00:00:00 GMT以来的
String|toString()|把此Date对象转换为各种形式的String

# 2. DateFormat类和SimpleDateFormat类
1. 构造:`SimpleDateFormat a = new SimpleDateFormat("YYYY-MM-dd")`
2. 按照构造中的格式转化生成相应Date对象：`Date date = a.parse("2019-6-21")`
3. 按照long类型的变量生成对应的Date对象：`Date date = new Date(long)`
4. 具体的格式化字符介绍
    1. 使用的语句：`SimpleDateFormat a = new SimpleDateFormat("yyyy")`
    2. 转化成为字符串：`a.format(Date)`

标识符|内涵
--|--
G|年代标志符
y|年
M|月
d|日
h|时，12进制，1-12
H|时，24进制，0-23
m|分
s|秒
S|毫秒
E|星期
D|一年中的第几天
F|一月中第几个星期几
w|一年中第几个星期
W|一月中的第几个星期
a|上午/下午 标记符
k|时，一天中(1-24)
K|时，在上午或下午(0-11)
z|时区

```java
public static String getLongDate(long time){
    Date date = new Date(time);
    SimpleDateFormat sd = new SimpleDateFormat("yyyyMMddHHmmss");
    return sd.format(date);
}
```

## 2.1. 线程不安全
```java
private StringBuffer format(Date date, StringBuffer toAppendTo,FieldDelegate delegate) {
        // Convert input date to time field list
        calendar.setTime(date);
        boolean useDateFormatSymbols = useDateFormatSymbols();
        for (int i = 0; i < compiledPattern.length; ) {
            int tag = compiledPattern[i] >>> 8;
            int count = compiledPattern[i++] & 0xff;
            if (count == 255) {
                count = compiledPattern[i++] << 16;
                count |= compiledPattern[i++];
            }
            switch (tag) {
                case TAG_QUOTE_ASCII_CHAR:
                    toAppendTo.append((char)count);
                    break;
                case TAG_QUOTE_CHARS:
                    toAppendTo.append(compiledPattern, i, count);
                    i += count;
                    break;
                default:
                    subFormat(tag, count, delegate, toAppendTo, useDateFormatSymbols);
                    break;
            }
        }
        return toAppendTo;
}
```
1. Calender是共享变量，并且没有做线程安全控制，当多个线程同时使用相同的static的SimpleDateFormat方法的时候，多个线程同时调用setTime导致Time的覆盖
2. 注意处理format是线程不安全的，parse的模块也是线程不安全的。
3. parse方法实际调用alb.establish(calendar).getTime()方法来解析，alb.establish(calendar)方法里主要完成了
   1. 重置日期对象cal的属性值
   2. 使用calb中中属性设置cal
   3. 返回设置好的cal对象
4. 多线程并发如何保证线程安全:
   1. 避免线程之间共享一个SimpleDateFormat对象，每个线程使用时都创建一次SimpleDateFormat对象 => 创建和销毁对象的开销大
   2. 对使用format和parse方法的地方进行加锁 => 线程阻塞性能差
   3. 使用ThreadLocal保证每个线程最多只创建一次SimpleDateFormat对象 => 较好的方法

# 3. Calendar日历类
1. 他是一个抽象类，定义了一些特定的日历字段之间的转换用的方法
2. 注意月份的表示中：一月是0

# 4. LocalDate
1. 为什么建议使用LocalDate而不是Date?
   1. 因为Date中的很多方法都已经被弃用，并且是线程不安全的
2. 只能获取年月日

## 4.1. 创建对象
```java
//获取当前年月日
LocalDate localDate = LocalDate.now();
//构造指定的年月日
LocalDate localDate1 = LocalDate.of(2019, 9, 10);
```

## 4.2. 获取属性
```java
//获取年
int year = localDate.getYear();
int year1 = localDate.get(ChronoField.YEAR);
//获取月
Month month = localDate.getMonth();
int month1 = localDate.get(ChronoField.MONTH_OF_YEAR);
//获取天
int day = localDate.getDayOfMonth();
int day1 = localDate.get(ChronoField.DAY_OF_MONTH);
//获取星期几
DayOfWeek dayOfWeek = localDate.getDayOfWeek();
int dayOfWeek1 = localDate.get(ChronoField.DAY_OF_WEEK);
```

## 4.3. 时间计算
```java
LocalDate localDate = LocalDate.now();
LocalDate localDate1 = localDate.with(firstDayOfYear());//当前日期的第一天日期
```

## 4.4. 格式化和解析时间
```java
LocalDate localDate = LocalDate.of(2019, 9, 10);
String s1 = localDate.format(DateTimeFormatter.BASIC_ISO_DATE);
String s2 = localDate.format(DateTimeFormatter.ISO_LOCAL_DATE);

//自定义格式化
DateTimeFormatter dateTimeFormatter =   DateTimeFormatter.ofPattern("dd/MM/yyyy");
String s3 = localDate.format(dateTimeFormatter);

//解析时间
LocalDate localDate1 = LocalDate.parse("20190910", DateTimeFormatter.BASIC_ISO_DATE);
LocalDate localDate2 = LocalDate.parse("2019-09-10", DateTimeFormatter.ISO_LOCAL_DATE);

```

# 5. LocalTime
1. 只会获取几点几分几秒

## 5.1. 创建
```java
//获取当前时间
LocalTime localTime1 = LocalTime.now();
//获取指定时间
LocalTime localTime = LocalTime.of(13, 51, 10);
```

## 5.2. 属性
```java
//获取小时
int hour = localTime.getHour();
int hour1 = localTime.get(ChronoField.HOUR_OF_DAY);
//获取分
int minute = localTime.getMinute();
int minute1 = localTime.get(ChronoField.MINUTE_OF_HOUR);
//获取秒
int second = localTime.getSecond();
int second1 = localTime.get(ChronoField.SECOND_OF_MINUTE);
```

# 6. LocalDateTime
1. 获取年月日时分秒，等于LocalDate + LocalTime

## 6.1. 创建类
```java
LocalDateTime localDateTime = LocalDateTime.now();
LocalDateTime localDateTime1 = LocalDateTime.of(2019, Month.SEPTEMBER, 10, 14, 46, 56);
LocalDateTime localDateTime2 = LocalDateTime.of(localDate, localTime);
LocalDateTime localDateTime3 = localDate.atTime(localTime);
LocalDateTime localDateTime4 = localTime.atDate(localDate);
//字符串转时间
String dateTimeStr = "2018-07-28 14:11:15";
DateTimeFormatter df = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
LocalDateTime dateTime = LocalDateTime.parse(dateTimeStr, df);
//时间戳转换
LocalDateTime time =LocalDateTime.ofEpochSecond(timestamp/1000,0, ZoneOffset.ofHours(8));
```

## 6.2. 属性
```java
//获取LocalDate
LocalDate localDate2 = localDateTime.toLocalDate();
//获取LocalTime
LocalTime localTime2 = localDateTime.toLocalTime();

//时间转字符串格式化
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyyMMddHHmmssSSS");
String dateTime = LocalDateTime.now(ZoneOffset.of("+8")).format(formatter);
//转换为时间戳
time.toInstant(ZoneOffset.of("+8")).toEpochMilli()
```

## 6.3. 值修改
```java
//增加一年
localDateTime = localDateTime.plusYears(1);
localDateTime = localDateTime.plus(1, ChronoUnit.YEARS);
//减少一个月
localDateTime = localDateTime.minusMonths(1);
localDateTime = localDateTime.minus(1, ChronoUnit.MONTHS);
//修改年为2019
localDateTime = localDateTime.withYear(2020);
//修改为2022
localDateTime = localDateTime.with(ChronoField.YEAR, 2022);
```

## 6.4. 时间计算
```java
LocalDateTime now = LocalDateTime.now();
System.out.println("计算两个时间的差：");
LocalDateTime end = LocalDateTime.now();
Duration duration = Duration.between(now,end);
long days = duration.toDays(); //相差的天数
long hours = duration.toHours();//相差的小时数
long minutes = duration.toMinutes();//相差的分钟数
long millis = duration.toMillis();//相差毫秒数
long nanos = duration.toNanos();//相差的纳秒数
System.out.println(now);
System.out.println(end);
System.out.println("发送短信耗时【 "+days+"天："+hours+" 小时："+minutes+" 分钟："+millis+" 毫秒："+nanos+" 纳秒】");
```

## 6.5. LocalDateTime和时间戳
```java
//LocalDateTime转换为时间戳
Long timestamp = LocalDateTime.now().toInstant(ZoneOffset.of("+8")).toEpochMilli();
//时间戳转换为LocalDateTime
LocalDateTime time2 =LocalDateTime.ofEpochSecond(timestamp/1000,0,ZoneOffset.ofHours(8));
```

# 7. Instant类

## 7.1. 创建类
```java
Instant instant = Instant.now();
```

## 7.2. 属性
```java
long currentSecond = instant.getEpochSecond();
long currentMilli = instant.toEpochMilli();
```
