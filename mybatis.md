# mybatis

[TOC]

## 基础

### #与$的区别

> ＃{}解析为一个JDBC预编译（prepared statement）的参数标记符

 ＃{}被解析为一个参数占位符？.

示例：select * from user where name = #{name} 被解析为select * from user where name = ？

> ${}仅仅是一个纯粹的string替换，动态sql解析阶段将会进行变量替换

示例：select * from user where name = ${name} 当传递的参数为”tianmiao“时，sql会被替换成select * from user where name =“tianmiao” .

> 表名作为变量时，必须使用${}

原因：表名是字符串，使用＃时会被替换为？，在对？进行变量替换后，表名会带上单引号，导致sql语法错误。

例如：select * from #{table_name} where name = #{name}

预编译后的sql是select * from ？ where name = ？

传入参数table_name = "student" , name = "tianmiao"，占位符进行变量替换后，sql变成

select * from ‘student’ where name = ‘tianmiao’ ，sql语句出错。

### parameterType

* 基本数据类型：int, string ,date. 作为传参时，只能传一个。#{参数名}获取传入值。
* 复杂数据类型：包括JAVA实体类和Map。通过#{属性名}或者#{map的keyname}获取传入值。

### 传入参数的方式

mybatis提供了一个使用注解来注入多个参数的方式，在接口的参数上添加@Param注解（@Param实质是将多个参数绑定到一个map作为输入参数）。

或者，采用map的方式封装多个参数。

parameterType是传入参数的类型。

1. 传入的是对象类型，

```java
int insert(User user);   //xml中使用parameterType = "User"
```

2. 传入的是多个参数（用@Param注解），或者直接组装成一个map数据结构时，使用

   ```xml
   parameterType = "map"
   ```

   直接通过#{keyname}引用到键对应的值。

3. 传入是集合类型

   mybatis会自动将List或Array对象包装到一个Map对象中，List类型的对象会以“list”为键名，Array对象会以“array”为键名。在sql映射文件中使用foreach来遍历List或Array元素。

### foreach

用于构建in条件中，可以在sql语句中迭代一个集合。

foreach的属性主要有item, index, collection, open , close, separator

*item*: 集合中每一个元素进行迭代时的别名；

*index*：指定一个名字，用于表示在迭代过程中，每次迭代到的位置；

*open*: 该语句以什么开始；

*separator*: 每次进行迭代之间以什么符号作为分隔符；

*close*: 以什么结束；

*collection*： 

​	当传入是单参数且参数类型为List时，collection="list";

​	当传入是单参数且参数类型为Array时，collection="array";

​	当传入是多参数，把参数封装成Map的类型，collection=传入的参数的key.

