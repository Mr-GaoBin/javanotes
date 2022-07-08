# hutool



## DateUtil(时间工具类)

* 方法摘要

| 方法                                          | 描述                                               |
| --------------------------------------------- | -------------------------------------------------- |
| cn.hutool.core.date.DateUtil.date()           | 当前时间，转换为{@link DateTime}对象               |
| cn.hutool.core.date.DateUtil.dateSecond()     | 当前时间，转换为{@link DateTime}对象，忽略毫秒部分 |
| cn.hutool.core.date.DateUtil.current()        | 当前时间的时间戳                                   |
| cn.hutool.core.date.DateUtil.currentSeconds() | 当前时间的时间戳（秒）                             |
| cn.hutool.core.date.DateUtil.now()            | 当前时间，格式 yyyy-MM-dd HH:mm:ss                 |
| cn.hutool.core.date.DateUtil.today()          | 当前日期，格式 yyyy-MM-dd                          |



*  字符串转日期

>DateUtil.parse方法会自动识别一些常用格式，包括：
>
>yyyy-MM-dd HH:mm:ss
>
>yyyy-MM-dd
>
>HH:mm:ss
>
>yyyy-MM-dd HH:mm
>
>yyyy-MM-dd HH:mm:ss.SSS
>
>这四种情况均可以转换，并且输出结果均为： 2019-09-17 00:00:00
>
>Date date1 = DateUtil.parse("2019-09-17");
>
>Date date2 = DateUtil.parse("2019-09-17", "yyyy-MM-dd");
>
>Date date3 = DateUtil.parse("2019/09/17", "yyyy/MM/dd");
>
>Date date4 = DateUtil.parse("2019:09:17", "yyyy:MM:dd");



* 日期时间差

>Date date1 = DateUtil.parse("2019-09-20 17:35:35");
>
>Date date2 = DateUtil.parse("2019-09-17 14:35:35");
>
>//这里若date1和date2换位置，输出结果不变：betweenDay = 3
>
>long betweenDay = DateUtil.between(date1, date2, DateUnit.DAY);



* 日期时间偏移

>String now = DateUtil.now();
>
>//date = 2019-09-17 17:35:35
>
>Date date = DateUtil.parse(now);
>
>//结果：newDate = 2019-09-19 17:35:35
>
>Date newDate = DateUtil.offset(date, DateField.DAY_OF_MONTH, 2);
>
>//常用偏移，结果：newDate2 = 2019-09-20 17:35:35
>
>DateTime newDate2 = DateUtil.offsetDay(date, 3);
>
>//常用偏移，结果：newDate3 = 2019-09-17 14:35:35
>
>DateTime newDate3 = DateUtil.offsetHour(date, -3);
>
>针对当前时间，提供了简化的偏移方法（例如昨天、上周、上个月等）：
>
>昨天
>
>DateUtil.yesterday()
>
>明天
>
>DateUtil.tomorrow()
>
>上周
>
>DateUtil.lastWeek()
>
>下周
>
>DateUtil.nextWeek()
>
>上个月
>
>DateUtil.lastMonth()
>
>下个月
>
>DateUtil.nextMonth()



* other

>年龄：ageOfNow = 28
>
>int ageOfNow = DateUtil.ageOfNow("1991-01-13");
>
>是否闰年：leapYear = false
>
>boolean leapYear = DateUtil.isLeapYear(2019);



```java

        Calendar calendar = Calendar.getInstance();
        //获取明天此刻的时间
        calendar.add(Calendar.DATE, 1);
        //获取当前时间
        calendar.get(Calendar.DATE);
        System.out.println(calendar.getTime());



        //获取当前时间
        LocalDateTime today = LocalDateTime.now();
        //两种方法获取明天此刻时间
        LocalDateTime tomorrow = today.plusDays(1);
        LocalDateTime tomorrow2 = today.minusDays(-1);
        System.out.println(today);
        System.out.println(tomorrow);
        System.out.println(tomorrow2);


        //当前时间:date = 2019-09-17 16:59:23
         Date date = DateUtil.date();
         //当前时间:date2 = 2019-09-17 16
         Date date2 = DateUtil.date(Calendar.getInstance());
         //当前时间:date3 = 2019-09-17 16
         Date date3 = DateUtil.date(System.currentTimeMillis());
         //当前时间字符串:now = 2019-09-17 16
         String now = DateUtil.now();
         //明天日期字符串
         DateTime today1 = DateUtil.tomorrow();
```

