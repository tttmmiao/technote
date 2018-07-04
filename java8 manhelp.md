[TOC]

## java8 集合之间的转换
### list转map
```java
list.stream().collect(Collectors.toMap(Object::getXx,a->a,(k1,k2)->k1));
```
**group by a list and display the total count of it**

```java
list.stream().collect(Collectors.groupingBy(Function.identity(),Collectors.counting()));
```

**group by the field of the objct**

```java
Map<XXX,List<YYY>> map = list.stream().collect(Collectors.groupingBy(Object::getXxx,Collectors.mapping(Object::getYYY,Collectors.toList())));
```

### 分组 list转Map<Key,ValueList>
将list中的对象按照某个属性进行分组。
```java
list.stream().collect(Collectors.groupingBy(Object::getXx));
```
将list中的对象按照某个属性进行分组，并对分组集合进行排序后，取出第一个值

```java
 Map<Integer,Integer> res = list.stream().sorted(Comparator.comparing(Object::getXXX))
                    .collect(Collectors.groupingBy(Object::getYYY,
                            Collectors.collectingAndThen(Collectors.toList(),values->values(0).getZZZ())));
```

### 过滤 filter
```java
filterList = list.stream().filter(x->null!=x).collect(Collectors.toList);
```

### 求和
将集合中的数据按照某个属性求和
```java
BigDecimal result = list.stream().map(Object::getXx).reduce(BigDecimal.ZERO,BigDecimal::add);
```

## 获取时间
[参考网址](https://www.liaoxuefeng.com/article/00141939241051502ada88137694b62bfe844cd79e12c32000)

Java8 新增了 *LocalDate* 和 *LocalTime*接口。

*java.util.Date* 和 *SimpleDateFormatter* 都不是线程安全的，且需要配合Calendar写很多代码。*LocalDate* 和 *LocalTime* 与最基本的 *String* 类一样，是final类型，线程安全且不可修改。

*java.util.Date* 包含日期
*java.util.Time* 包含时间
*java.util.DateTime* 包含日期和时间

### localDate的使用
```java
//获取当前日期
LocalDate today = LocalDate.now();
//根据年，月，日获取日期
LocalDate someday = LocalDate.of(2018,8,4);
// 根据字符串取：
LocalDate endOfFeb = LocalDate.parse("2014-02-28");
LocalDate.parse("2014-02-29"); // 无效日期无法通过：DateTimeParseException: Invalid date
```
### 常见日期转换
```java
// 取本月第1天：
LocalDate firstDayOfThisMonth = today.with(TemporalAdjusters.firstDayOfMonth()); 
// 取本月第2天：
LocalDate secondDayOfThisMonth = today.withDayOfMonth(2); 
// 取本月最后一天，再也不用计算是28，29，30还是31：
LocalDate lastDayOfThisMonth = today.with(TemporalAdjusters.lastDayOfMonth()); 
// 取下一天：
LocalDate firstDayOf2015 = lastDayOfThisMonth.plusDays(1); 
// 取2015年1月第一个周一
LocalDate firstMondayOf2015 = LocalDate.parse("2015-01-01").with(TemporalAdjusters.firstInMonth(DayOfWeek.MONDAY)); 
```
### localTime的使用
```java
LocalTime now = LocalTime.now(); // 11:09:09.240
//清除毫秒数
LocalTime now = LocalTime.now().withNano(0)); // 11:09:09
//构造时间
LocalTime zero = LocalTime.of(0, 0, 0); // 00:00:00
LocalTime mid = LocalTime.parse("12:00:00"); // 12:00:00
```
### 获取零点
```java
Date now = new Date();
LocalDateTime todayStart = LocalDateTime.of(LocalDate.now(), LocalTime.MIN);
Date start = Date.from(todayStart.atZone(ZoneId.systemDefault()).toInstant());
```