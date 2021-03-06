# Problems

## mybatis报错
**错误1:Mapped Statements collection does not contain value**

* 原因：发现sqlSession.mappedStatements为空，通过
  sqlSession.mappedStatements.get(id)时报错。
  -> sqlSession.loadedResources中没有加载到xml文件
  -> IDEA是不会编译src的java目录的xml文件，所以在Mybatis的配置文件中找不到xml文件！（也有可能是Maven构建项目的问题，网上教程很多项目是普通的Java web项目，所以可以放到src下面也能读取到） -- [参考解决方案](http://blog.csdn.net/u010648555/article/details/70880425)
  
  解决方案：在pom文件中，添加
  
  ```xml
  <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                </includes>
            </resource>
        </resources>
    </build>
  ```
  
* 排查过程：检查了很多次appcontext-dao中关于数据库的配置，没问题。最终定位出原因是xml文件没有被加载。
* 可尽快定位问题的方式：直接调试源码。

**批量操作常见异常及解决方法**

[批量操作常见异常](http://www.cnblogs.com/yangh965/p/6544949.html)

## JDK7 sort排序出现Comparison method violates its general contract! - compare方法需要返回1，-1，0

错误描述

```java
java.lang.IllegalArgumentException: Comparison method violates its general contract!  
at java.util.TimSort.mergeHi(TimSort.java:868)  
at java.util.TimSort.mergeAt(TimSort.java:485)  
at java.util.TimSort.mergeCollapse(TimSort.java:408)  
at java.util.TimSort.sort(TimSort.java:214)  
at java.util.TimSort.sort(TimSort.java:173)  
at java.util.Arrays.sort(Arrays.java:659)  
at java.util.Collections.sort(Collections.java:217)  
```
原因：JDK7中compare方法需要返回1，-1，0。

JDK7中的Collections.Sort方法实现中，如果两个值是相等的，那么compare方法需要返回0，否则 可能 会在排序时抛错，而JDK6是没有这个限制的。

在 JDK7 版本以上，Comparator 要满足自反性，传递性，对称性，不然 Arrays.sort，

Collections.sort 会报 IllegalArgumentException 异常。

说明：

1） 自反性：x，y 的比较结果和 y，x 的比较结果相反。

2） 传递性：x>y,y>z,则 x>z。

3） 对称性：x=y,则 x,z 比较结果和 y，z 比较结果相同。
反例：下例中没有处理相等的情况，实际使用中可能会出现异常：

因为违反了自反性
new Comparator<Student>() {
    @Override
    public int compare(Student o1, Student o2) {
        return o1.getId() > o2.getId() ? 1 : -1;
    }
} 

```java
//错误的方式
Collections.sort(list, new Comparator<Integer>() {
	@Override
	public int compare(Integer o1, Integer o2) {
		return o1 > o2 ? 1 : -1;// 错误的方式
	}
});

```

```java
//正确的方式
Collections.sort(list, new Comparator<Integer>() {
	@Override
	public int compare(Integer o1, Integer o2) {
		// return o1 > o2 ? 1 : -1;
		return o1.compareTo(o2);// 正确的方式
	}
});
```

参考[stackoverflow-why does my compare method throw exception](https://stackoverflow.com/questions/6626437/why-does-my-compare-method-throw-exception-comparison-method-violates-its-gen)

[图解JDK7的Comparison method violates its general contract异常](http://blog.2baxb.me/archives/993)

## 线程同步 wait-notify时抛IllegalMonitorException

* Integer flag = 1;

  ```java
  synchronized(flag){
     if(){
        flag.wait();
     }
     flag = flag + 1;
     flag.notity(); //throw IllegalMonitorException
  }
  ```
  
  问题：将Integer换成AtomicInteger不会抛错。
  原因：在同步块中对flag进行了赋值操作，使flag引用的对象改变，再调用notify的时候抛出异常（因为没有获得该flag引用的控制权，即没有synchronized）. 不能用Integer,Integer是不可变对象
  
## hashMap中的key没有重写equals方法引起引用频繁fullGC

```java
 for(int i = 0;i < 10000;i++){
	 map.put(key,value)
 }
```

key没有重写equals方法，每次比较两个对象的地址是否一样，map的长度不断增加。

## 异步补偿模块
* 通过反射newInstance. 如AService依赖BService,没有考虑到依赖，BService为null
  
  解决思路：如何获取AService
  解决过程：通过上下文getBean
  method.invoke(obj,args)
  obj传入bean时，出现
  object is not an instance of declaring class错误 --- 原因：bean是一个jdkDynamicAopProxy, method应为接口对象，而不是实例对象