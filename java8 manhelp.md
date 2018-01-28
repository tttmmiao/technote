## java8 集合之间的转换
### list转map
```java
list.stream().collect(Collectors.toMap(Object::getXx,a->a,(k1,k2)->k1));
```
### 分组 list转Map<Key,ValueList>
将list中的对象按照某个属性进行分组。
```java
list.stream().collect(Collectors.groupingBy(Object::getXx));
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