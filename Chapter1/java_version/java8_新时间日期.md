# 新时间日期API

## 旧SimpleDateFormat的线程安全问题

- 多个线程同时格式化,会抛异常

```java
SimpleDateFormat sdf = new SimpleDateFormat("yyyyMMdd");
Callable<Date> task = new Callable<Date>() {
    @Override
    public Date call() throws Exception {
        return sdf.parse("20161121");
    }
    
};
ExecutorService pool = Executors.newFixedThreadPool(10);
List<Future<Date>> results = new ArrayList<>();
for (int i = 0; i < 10; i++) {
    results.add(pool.submit(task));
}
for (Future<Date> future : results) {
    System.out.println(future.get());
}
pool.shutdown();
```

- 解决线程安全问题

```java
Callable<Date> task = new Callable<Date>() {
    @Override
    public Date call() throws Exception {
        return DateFormatThreadLocal.convert("20161121");
    }
    
};
ExecutorService pool = Executors.newFixedThreadPool(10);
List<Future<Date>> results = new ArrayList<>();
for (int i = 0; i < 10; i++) {
    results.add(pool.submit(task));
}
for (Future<Date> future : results) {
    System.out.println(future.get());
}
pool.shutdown();

//其中DateFormatThreadLocal
public class DateFormatThreadLocal {

    private static final ThreadLocal<DateFormat> df = new ThreadLocal<DateFormat>() {
        protected DateFormat initialValue() {
            return new SimpleDateFormat("yyyyMMdd");
        }

    };

    public static final Date convert(String source) throws ParseException {
        return df.get().parse(source);
    }

}
```

- LocalDate则不存在线程安全问题

```java
DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyyMMdd");
Callable<LocalDate> task = new Callable<LocalDate>() {
    @Override
    public LocalDate call() throws Exception {
        LocalDate ld = LocalDate.parse("20161121", dtf);
        return ld;
    }
};
ExecutorService pool = Executors.newFixedThreadPool(10);
List<Future<LocalDate>> results = new ArrayList<>();
for (int i = 0; i < 10; i++) {
    results.add(pool.submit(task));
}
for (Future<LocalDate> future : results) {
    System.out.println(future.get());
}
pool.shutdown();
```

## LocalDate、LocalTime、LocalDateTime
LocalDate、LocalTime、LocalDateTime 类的实例是不可变的对象，分别表示使用 ISO-8601日历系统的日期、时间、日期和时间。它们提供了简单的日期或时间，并不包含当前的时间信息。也不包含与时区相关的信息

|方法 |描述 |示例|
|:---|:---|:---|
|now() |静态方法，根据当前时间创建对象| LocalDate localDate = LocalDate.now()<br>LocalTime localTime = LocalTime.now()<br>LocalDateTime localDateTime = LocalDateTime.now()|
|of()  |静态方法，根据指定日期/时间创建对象|LocalDate localDate = LocalDate.of(2016, 10, 26)<br>LocalTime localTime = LocalTime.of(02, 22, 56)<br>LocalDateTime localDateTime = LocalDateTime.of(2016, 10, 26, 12, 10, 55)|
|plusDays<br>plusWeeks<br>plusMonths<br>plusYears|向当前 LocalDate 对象添加几天、几周、几个月、几年||
|minusDays<br>minusWeeks<br>minusMonths<br>minusYears|从当前 LocalDate 对象减去几天、几周、几个月、几年||
|plus<br>minus |添加或减少一个 Duration 或 Period||
|withDayOfMonth<br>withDayOfYear<br>withMonth<br>withYear|将月份天数、年份天数、月份、年份修改为指定的值并返回新的LocalDate 对象||
|getDayOfMonth |获得月份天数(1-31)||
|getDayOfYear |获得年份天数(1-366)||
|getDayOfWeek |获得星期几(返回一个 DayOfWeek枚举值)||
|getMonth |获得月份, 返回一个 Month 枚举值||
|getMonthValue |获得月份(1-12)||
|getYear |获得年份||
|until |获得两个日期之间的 Period 对象，或者指定 ChronoUnits 的数字||
|isBefore<br>isAfter |比较两个 LocalDate||
|isLeapYear |判断是否是闰年||

## Instant 时间戳
用于“时间戳”的运算。它是以Unix元年(传统的设定为UTC时区1970年1月1日午夜时分)开始所经历的描述进行运算

```java
@Test
public void test2() {
    Instant ins = Instant.now();  //默认使用 UTC 时区
    System.out.println(ins);

    OffsetDateTime odt = ins.atOffset(ZoneOffset.ofHours(8));//指定时区
    System.out.println(odt);

    System.out.println(ins.toEpochMilli());//转成时间戳

    Instant ins2 = Instant.ofEpochSecond(5);//设置Instant/秒
    System.out.println(ins2);
}
```

## Duration 和 Period
* Duration:用于计算两个“时间”间隔
* Period:用于计算两个“日期”间隔

```java
@Test
public void test3() {
    Instant ins1 = Instant.now();

    System.out.println("--------------------");
    try {
        Thread.sleep(1000);
    } catch (InterruptedException e) {
    }

    Instant ins2 = Instant.now();
    Duration between = Duration.between(ins1, ins2);
    System.out.println("所耗费时间为：" + between);
    System.out.println("所耗费时间为：" + between.getSeconds());
    System.out.println("所耗费时间为：" + between.toMillis());

    System.out.println("----------------------------------");

    LocalDate ld1 = LocalDate.now();
    LocalDate ld2 = LocalDate.of(2011, 1, 1);

    Period pe = Period.between(ld2, ld1);
    System.out.println(pe.getYears());
    System.out.println(pe.getMonths());
    System.out.println(pe.getDays());

    Period until = ld2.until(ld1);//同between
    System.out.println(until.getYears());
}
```

## 日期的操作
* TemporalAdjuster : 时间校正器。有时我们可能需要获取例如：将日期调整到“下个周日”等操作。
* TemporalAdjusters : 该类通过静态方法提供了大量的常用 TemporalAdjuster 的实现

```java
@Test
public void test4() {
    LocalDateTime ldt = LocalDateTime.now();
    System.out.println(ldt);

    LocalDateTime ldt2 = ldt.withDayOfMonth(10);
    System.out.println(ldt2);

    LocalDateTime ldt3 = ldt.with(TemporalAdjusters.next(DayOfWeek.SUNDAY));
    System.out.println(ldt3);

    //自定义：下一个工作日
    LocalDateTime ldt5 = ldt.with((l) -> {
        LocalDateTime ldt4 = (LocalDateTime) l;
        DayOfWeek dow = ldt4.getDayOfWeek();

        if (dow.equals(DayOfWeek.MONDAY)) {
            return ldt4.plusDays(7);
        } else if (dow.equals(DayOfWeek.TUESDAY)) {
            return ldt4.plusDays(6);
        } else if (dow.equals(DayOfWeek.SUNDAY)) {
            return ldt4.plusDays(5);
        } else if (dow.equals(DayOfWeek.THURSDAY)) {
            return ldt4.plusDays(4);
        } else if (dow.equals(DayOfWeek.FRIDAY)) {
            return ldt4.plusDays(3);
        } else if (dow.equals(DayOfWeek.SATURDAY)) {
            return ldt4.plusDays(2);
        } else {
            return ldt4.plusDays(1);
        }
    });

    System.out.println(ldt5);

}
```

## 解析和格式化
java.time.format.DateTimeFormatter 类：该类提供了三种格式化方法：
* 预定义的标准格式
* 语言环境相关的格式
* 自定义的格式

```java
@Test
public void test5() {
//		DateTimeFormatter dtf = DateTimeFormatter.ISO_LOCAL_DATE;
    DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy年MM月dd日 HH:mm:ss E");

    LocalDateTime ldt = LocalDateTime.now();
    String strDate = ldt.format(dtf);
    System.out.println(strDate);

    LocalDateTime newLdt = ldt.parse(strDate, dtf);
    System.out.println(newLdt);
}
```

## 时区的处理
Java8 中加入了对时区的支持，带时区的时间为分别为：
> ZonedDate  
> ZonedTime  
> ZonedDateTime  

其中每个时区都对应着 ID，地区ID都为 “{区域}/{城市}”的格式例如 ：Asia/Shanghai 等  
* ZoneId：该类中包含了所有的时区信息  
* getAvailableZoneIds() : 可以获取所有时区时区信息
* of(id) : 用指定的时区信息获取 ZoneId 对象

```java
@Test
public void test7() {
    LocalDateTime ldt = LocalDateTime.now(ZoneId.of("Asia/Shanghai"));
    System.out.println(ldt);

    ZonedDateTime zdt = ZonedDateTime.now(ZoneId.of("US/Pacific"));
    System.out.println(zdt);
}

@Test
public void test6() {
    Set<String> set = ZoneId.getAvailableZoneIds();
    set.forEach(System.out::println);
}
```

## 与传统日期处理的转换

|类 |To 遗留类 |From 遗留类|
|:---|:---|:---|
|java.time.Instant <br> java.util.Date |Date.from(instant) |date.toInstant()|
|java.time.Instant <br> java.sql.Timestamp |Timestamp.from(instant) |timestamp.toInstant()|
|java.time.ZonedDateTime <br> java.util.GregorianCalendar|GregorianCalendar.from(zonedDateTime) |cal.toZonedDateTime()|
|java.time.LocalDate <br> java.sql.Time |Date.valueOf(localDate) |date.toLocalDate()|
|java.time.LocalTime <br> java.sql.Time |Date.valueOf(localDate) |date.toLocalTime()|
|java.time.LocalDateTime <br> java.sql.Timestamp |Timestamp.valueOf(localDateTime) |timestamp.toLocalDateTime()|
|java.time.ZoneId <br> java.util.TimeZone |Timezone.getTimeZone(id) |timeZone.toZoneId()|
|java.time.format.DateTimeFormatter <br> java.text.DateFormat |formatter.toFormat() |无|