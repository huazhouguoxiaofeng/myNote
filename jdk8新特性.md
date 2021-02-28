### 集合与lambda表达式

```java
List<Integer> collect = integers.stream().collect(Collectors.toList());List<Integer> collect = integers.stream().collect(Collectors.toCollection(LinkedList::new));
List<Integer> collect1 = integers.stream().collect(Collectors.toCollection(CopyOnWriteArrayList::new));
Set<Integer> collect2 = integers.stream().collect(Collectors.toCollection(TreeSet::new));
```

 ### 史上最全lambda示例

```java
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.junit.Test;

import java.math.BigInteger;
import java.util.*;
import java.util.function.Function;
import java.util.stream.Collectors;
import java.util.stream.IntStream;
import java.util.stream.Stream;

public class StreamStudy extends User {

    /**
     * 数组变成流：重载的方法，可以有始末
     * 注意这个方法的开环与闭环：stream(T[] array, int startInclusive, int endExclusive)
     */
    private static Stream<Integer> arrStream = Arrays.stream(intArray, 1, 3);
    private static Stream<String> strStream = strList.stream();


    /**
     * 只产生一个echo哦
     * 或者产生好多个东东哦
     */
    private static Stream<String> ofEchoStream = Stream.of("echo");
    private static Stream<String> strStream02 = Stream.of("gently", "down", "the", "stream");

    private static Stream<String> emptyStream = Stream.empty();

    /**
     * 1）随机产生无限个echo，所以一定要limit截住，否则 outOfMemoryException
     * 2）generate()方法一定是无限流
     * 3)输出：echo,echo,echo,echo
     */
    @Test
    public void testGenerateStream(){
        Stream<String> generateStream = Stream.generate(() -> "echo").limit(4);
        System.out.println(generateStream.collect(Collectors.joining(",")));
    }

    /**
     * 随机产生数字
     */
    private static Stream<Double> randomStream = Stream.generate(Math::random);

    /**
     * 起始值为0，每次加1
     */
    private static Stream<BigInteger> intStream02 = Stream.iterate(BigInteger.ZERO, n -> n.add(BigInteger.ONE));

    /**
     * 合并流
     */
    @Test
    public void testCombineStream(){
        Stream<String> combineStream = Stream.concat(strStream02,ofEchoStream);
        System.out.println(combineStream.collect(Collectors.toList()));
    }

    /**
     * 并行流
     * isParallel()：判断是否是并行流
     */
    private static Stream<String> strStream03 = strList.parallelStream();
    private static Stream<String> strStream04 = strList.stream().parallel();

    /**
     * list 转 hashMap 类型
     * 注意map的泛型，以及toMap里面的参数值
     * list 转其他map类型待续
     */
    @Test
    public void toMap(){
        Map<String, String> collect = oldUserList.stream().collect(Collectors.toMap(User::getId, User::getName));
        // 注意看 Function.identity() 源码：就是类似于 i -> i；如果流中 StreamData::getId 有一样的话，则 throw an IllegalStateException
        Map<String, User> collect02 = oldUserList.stream().collect(Collectors.toMap(User::getId, Function.identity()));
        // 当有多个相同key值的时候，要保留哪个value值呢，就用下面那个方法（这里的策略是用新值）
        Map<String, User> collect1 = oldUserList.stream().collect(Collectors.toMap(User::getId, Function.identity(), (existingValue, newValue) -> newValue));
        collect1.forEach((k,v) -> System.out.println(k + "," + v));
        // 如果要 value 值是个集合，看 groupingBy() 方法
    }

    /**
     * 流变数组
     */
    @Test
    public void streamToArray(){
        String[] strArr = strStream02.toArray(String[]::new);
    }

    /**
     * 1）map 方法里面的那个参数一定是一个lambda表达式  a.toUpperCase() ，如果里面  a.toUpperCase() ，则报错
     * 2）map 方法返回的一样是那个stream流，所以可以继续调用 方法，达到链式反应方法
     * 3）注意尽量用方法引用：map(String::toUpperCase)；而不是：map(a -> a.toUpperCase())
     */
    @Test
    public void mapFlatMapMethod() {
        String str = strList.stream().map(String::toUpperCase).collect(Collectors.joining(","));
        List<Integer> list = intList02.stream().map(cost -> cost + 50).map(cost -> cost * 2).collect(Collectors.toList());
        List<Stream<Integer>> collect1 = User.flatMapList.stream().map(Collection::stream).collect(Collectors.toList());
    /*
    输出：[1, 11, 2, 22, 3, 33, 4, 44, 5, 55]
    flatMap(i -> i.stream()) 可以变为  Collection::stream
    注意：这个 StreamData.flatMapList  里面的类型是个集合数组啊，所以才可以用stream方法嘛
     */
        List<Integer> collect = User.flatMapList.stream().flatMap(Collection::stream).collect(Collectors.toList());
        // 输出：[2, 22, 4, 44, 6, 66, 8, 88, 10, 110]
        List<Integer> collect02 = User.flatMapList.stream().flatMap(i -> i.stream().map(j -> j * 2)).collect(Collectors.toList());
    }

    /**
     * 集合遍历
     * 箭头左边表示参数
     * 箭头右边一定要是表达式，当里面有多个等式时，一定要用大括号哦
     */
    @Test
    public void iterator() {
        strList.forEach(a -> a += "eeee");
        strList.forEach(a -> {
            a += "ddd";
            System.out.println(a += "ddd");
            System.out.println(a + "ccc");
        });
    }


    /**
     * 对元素进行处理之后，再进行统计；Collectors的方法不能对元素进行处理，要不就要for循环进行遍历
     * Stream<E> 流对象 有 mapToInt、mapToLong、mapToDouble 方法，分别返回 IntStream、LongStream、DoubleStream 对象
     * IntStream、LongStream、DoubleStream 对象 调用 summaryStatistics() 方法返回 IntSummaryStatistics 对象
     * IntSummaryStatistics 一共有：getMax()、getMin()、getAverage()、getCount()、getSum() 五种方法
     */
    @Test
    public void primitiveTypeStreams() {
        // 映射，分别加 1，用 parallelStream() 乱序输出，你懂的
        intList.parallelStream().map(i -> i + 1).forEach(System.out::println);
        // 返回 IntSummaryStatistics 对象
        IntSummaryStatistics intSummaryStatistics = intList.stream().mapToInt(i -> i + 1).summaryStatistics();
        // 求各个对象里面年龄的总和、平均等
        int sum1 = oldUserList.stream().mapToInt(User::getAge).sum();
        long sum2 = oldUserList.stream().collect(Collectors.summarizingInt(User::getAge)).getSum();
        Double collect1 = oldUserList.stream().collect(Collectors.averagingInt(User::getAge)); // 求平均值
    }

    /**
     * 去重
     * peek()：就把流中的分别遍历打印吧，感觉用处不大
     */
    @Test
    public void distinctPeek() {
        List<Integer> collect = intList.stream().distinct().peek(System.out::print).collect(Collectors.toList());
        // 有序排列：[4, 5, 6, 1, 2, 3, 7, 8, 9, 10]
        List<Integer> list = intList.stream().distinct().collect(Collectors.toList());
        // 无序排列：[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
        Set<Integer> set = intList.stream().collect(Collectors.toSet());
    }

    /**
     * 双冒号：对参数不能有修改
     */
    @Test
    public void doubleColon() {
        // 变大写字母
        String str = strList.stream().map(String::toUpperCase).collect(Collectors.joining());
        //求和
        int sum = intList.stream().mapToInt(Integer::intValue).sum();

    }

    /**
     * 过滤
     */
    @Test
    public void filterFindFirstAnyAllMatch() {
        // 过滤出长度大于2的字符串：找到第一个
        Optional<String> first = strList.stream().filter(i -> i.length() > 2).findFirst();        // 这个非常的非常的重要，求两个集合中的交集；那样的话怎么求差集，明显取反!（i -> videoOrders2.contains(i)）；求并集的话，两个集合相加再 distint,或者一个集合再加上差集
        /*
        1）过滤出偶数列表
        2）findAny(): If you are OK with any match, not just the first one, use the findAny method.
        This is effective when you parallelize the stream, since the stream can report any match that it finds
        instead of being constrained to the first one.
         */
        Optional<Integer> any = intList.stream().parallel().filter(i -> i % 2 == 0).findAny();
        // match：注意可以用并行流
        boolean flag = strStream.parallel().allMatch(i -> i.length() > 3);
        boolean flag02 = strList.stream().parallel().anyMatch(i -> i.length() > 3);
        boolean flag03 = strList.parallelStream().noneMatch(i -> i.length() > 3);
    }

    /**
     * 一般般的: 求交集、并集、差集
     */
    @Test
    public void test04(){
        List<Integer> aaa = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);
        List<Integer> bbb = Arrays.asList(2, 4, 6, 8, 10, 12, 14);
        /*
        求交集
         */
        List<Integer> list = aaa.stream().filter(bbb::contains).collect(Collectors.toList());
        List<Integer> list02 = aaa.stream().filter(a -> bbb.contains(a)).collect(Collectors.toList());
        /*
        求aaa的差集
         */
        List<Integer> list03 = aaa.stream().filter(a -> !bbb.contains(a)).collect(Collectors.toList());
        /*
        求bbb的差集
         */
        List<Integer> list04 = bbb.stream().filter(b -> !aaa.contains(b)).collect(Collectors.toList());
        /*
        求并集
         */
        // 这个不是很严谨，放在这里仅供参考
        List<Integer> list05 = Stream.concat(aaa.stream(), bbb.stream()).distinct().collect(Collectors.toList());
        // aaa的差集加上bbb
        List<Integer> list06 = Stream.concat(aaa.stream().filter(a -> !bbb.contains(a)), bbb.stream()).collect(Collectors.toList());
        System.out.println(list06);
    }

    /**
     * 有点强大的：求交集、并集、差集
     */
    @Test
    public void test05(){
        // 交集，是做修改，实际项目中要把旧id的值set进去，还要for循环，所以这个好像没有意义
        List<User> updateList = newUserList.stream().filter(
                newa -> oldUserList.stream().map(User::getName).collect(Collectors.toList()).contains(newa.getName())
        ).collect(Collectors.toList());
        List<User> updateList02 = newUserList.stream().filter(
                newa -> oldUserList.stream().map(User::getName).collect(Collectors.toList()).contains(newa.getName()) &&
                        oldUserList.stream().map(User::getPwd).collect(Collectors.toList()).contains(newa.getPwd())

        ).collect(Collectors.toList());
        // 差集，新增（删除的，你懂啦）
        List<User> insertList = newUserList.stream().filter(
                newa -> !oldUserList.stream().map(User::getName).collect(Collectors.toList()).contains(newa.getName())
        ).collect(Collectors.toList());
        // 并集 就是差集加上另一个东东的全集
    }

    /**
     * 对里面的值处理再相加
     */
    @Test
    public void mapReduce() {
        // 为每个订单加上12%的税；因为只有一个参数，所以cost也可以不用括号；sum是自定义的一个变量
        double bill = intList02.stream().map(cost -> cost * 1.12).reduce((sum, cost) -> sum + cost).get();
        System.out.println("Total : " + bill);
    }

    @Test
    public void test01() {
        //转成其它数据结构比如set
        Set<Integer> integersSet = intList.stream().collect(Collectors.toSet());
        //复合操作
        List<Integer> list = intList.stream().filter(i -> i % 2 == 0).map(i -> i * i).distinct().collect(Collectors.toList());
    }

    /**
     * 排序并取出前n个元素
     * sorted(): It sorts the elements of stream using natural ordering. The element class must implement Comparable interface.
     * sorted(Comparator<? super T> comparator): Here we create an instance of Comparator using lambda expression. We can sort the stream elements in ascending and descending order.
     * To reverse the natural ordering Comparator provides reverseOrder() method. We use it as follows：list.stream().sorted(Comparator.reverseOrder())
     */
    @Test
    public void sortLimitSkip() {
        // 只获取前面多少位啊
        Stream<Double> limit = randomStream.limit(6);
        // skip()：跳过前面多少位数字
        Stream<Integer> skip = intList.stream().limit(5).skip(2);
        // 取出前五个数据 [4, 5, 6, 1, 2]
        List<Integer> limitInteger = intList.stream().limit(5).collect(Collectors.toList());
        //排序并且提取出前5个元素 [1,2,3,4,5]
        List<Integer> sortIntegers = intList.stream().sorted().limit(5).collect(Collectors.toList());
        List<Integer> collect1 = intList.stream().sorted(Comparator.reverseOrder()).collect(Collectors.toList());
        /*
    　　根据长度排行，正序或者倒序：
    　　1）要用 Comparator.comparing(String::length) ，不要用 Comparator.comparing(obj -> obj.length())
    　　2）核心，里面放一个 Comparator 对象
     　 */
        List<String> collect = strList.stream().sorted(Comparator.comparing(String::length)).collect(Collectors.toList());
        List<String> collect03 = strList.stream().sorted(Comparator.comparing(String::length,Comparator.reverseOrder())).collect(Collectors.toList());
        List<String> collect04 = strList.stream().sorted(Comparator.comparing(String::length).reversed()).collect(Collectors.toList());
        // 根据用户名字字母顺序排名，反序（终极版本）
        List<User> collect2 = oldUserList.stream().sorted(Comparator.comparing(User::getAge).thenComparing(User::getName).thenComparing(User::getPwd)).collect(Collectors.toList());
        // 得到年龄最大的 StreamData：核心也是放一个比较器
        Optional<User> max = oldUserList.stream().max(Comparator.comparing(User::getAge));

    }

    /**
     * 分组：groupingBy
     */
    @Test
    public void groupingPartitioningBy() {
        Map<String, List<User>> collect = oldUserList.stream().collect(Collectors.groupingBy(User::getId));
        // 根据奇偶性分组：{false=[5, 1, 3, 7, 9], true=[4, 6, 2, 8, 8, 10]}
        Map<Boolean, List<Integer>> listMap = intList.stream().collect(Collectors.groupingBy(i -> i % 2 == 0));
        Map<Boolean, Long> collect4 = intList.stream().collect(Collectors.groupingBy(i -> i % 2 == 0, Collectors.counting()));
        /* When the classifier function is a predicate function (that is, a function returning a boolean value), the stream elements are partitioned into two lists: those where the function returns true and the complement. In this case, it is more efficient to use partitioningBy instead of groupingBy. */
        Map<Boolean, List<Integer>> collect1 = intList.stream().collect(Collectors.partitioningBy(i -> i % 2 == 0));
        collect.forEach((k,v) -> System.out.println(k + "," + v));
    }
}



@Data
@AllArgsConstructor
@NoArgsConstructor
class User {

    public String id;
    public String name;
    public String pwd;
    public Integer age;
    public Double height;

    public static List<String> strList = Arrays.asList("aaaaa", "c", "bbb", "dd", "cc", "eeeeee", "fff", "zzz");
    public static List<Integer> intList = Arrays.asList(4, 5, 6,1, 2, 3,7, 8,8,9,10);
    public static List<Integer> intList02 = Arrays.asList(100, 200, 300, 400, 500);
    public static Integer[] intArray = {4, 5, 6,1, 2, 3,7, 8,8,9,10};

    public static List<User> oldUserList = new ArrayList<User>(){
        {
            this.add(new User("111", "guo", "111a", 11, 11.1));
            this.add(new User("222", "xiao", "222b", 22, 22.2));
            this.add(new User("333", "feng", "333c",23, 23.5));
            this.add(new User("444", "sadfaf", "444d", 22, 22.3));
            this.add(new User("555", "hasdghdsg", "444d", 22, 33.4));
        }
    };

    public static List<User> newUserList = new ArrayList<User>(){
        {
            this.add(new User("", "guo", "111", 11, 11.1));
            this.add(new User("", "xiao", "222", 22, 226.7));
            this.add(new User("", "feng", "333c",23, 23.5));
            this.add(new User("", "sadfafNew", "444d", 25, 22.72));
            this.add(new User("", "hasdghdsgNew", "444d", 24, 33.84));
        }
    };

    public static List<List<Integer>> flatMapList = new ArrayList<List<Integer>>(){
        {
            this.add(Arrays.asList(1,11));
            this.add(Arrays.asList(2,22));
            this.add(Arrays.asList(3,33));
            this.add(Arrays.asList(4,44));
            this.add(Arrays.asList(5,55));
        }
    };

}
```

### map

```java
package com.gxf.test.mytest;

import org.junit.After;
import org.junit.Before;
import org.junit.Test;

import java.util.HashMap;
import java.util.Map;

public class TestMap {

    private static Map<String, String> map = new HashMap<String, String>(){
        {
            this.put("11", "111");
            this.put("22", "222");
            this.put("33", "333");
        }
    };

    @Before
    public void testBefore(){
        
    }

    @Test
    public void test01(){
        // 如果 key 不存在或相关联的value值为 null, 则设置新的 key/value 值。
        map.putIfAbsent("11", "aaa");
        map.computeIfAbsent("11", k -> k + "22"); // 11 键已经存在了，所以对应的value值不被计算
        map.computeIfAbsent("55", k -> k + "22"); // 55 键还没有存在，所以计算，“55”+“22”
        // 如果key跟value都存在的话，则计算咯
        map.computeIfPresent("33", (k, v) -> k + v);
        // 只要 key 存在，不管对应值是否为  null，则用传入的 value 替代原来的值。即使传入的 value 是 null 也会用来替代原来的值，而不是删除
        map.replace("11", "bbb");
        // key 值存在且对应的value值，与第二个值相等时，用第三个值来替换新的value值
        map.replace("11", "bbb", "eee");
        // 对其中的key value值进行计算
        map.replaceAll((k,v) -> k + "-" + v);
        // 第一个值对应的value和第二个值相等的话，则移除
        map.remove("55","55-5522");
        /*
        computeIfAbsent 与 computeIfPresent  的结合体。也就是既不管 key 存不存在，也不管 key 对应的值是否为 null,
        compute 死活都要设置与 key 相关联的值，或者计算出的值为 null 时删除相应的 key, 返回值为最终的 map.get(key)。
         */
        String ret;
        Map<String, String> map01 = new HashMap<>() ;
        ret = map01.compute("a", (key, value) -> "a" + value); //ret="anull", map={"a":"anull"}
        ret = map01.compute("a", (key, value) -> "a" + value); //ret="aanull", map={"a":"aanull"}
        ret = map01.compute("a", (key, value) -> null); //ret=null, map={}
        System.out.println(map01);
    }

    @After
    public void testAfter(){

    }

}
```

### LocalDate、LocalDateTime 

```java
package com.gxf.test.mytest;

import org.junit.After;
import org.junit.Before;
import org.junit.Test;

import java.time.*;
import java.time.format.DateTimeFormatter;
import java.time.temporal.ChronoUnit;
import java.time.temporal.TemporalAdjusters;

public class TestLocalDateTime {

    // 2019-09-16 尼玛，真的好屌
    private static LocalDate now =LocalDate.now();
    // 2019-09-16T19:40:10.932 这个一般般
    private static LocalDateTime localDateTime = LocalDateTime.now();
    // 年月日 2012-09-16
    private static LocalDate of = LocalDate.of(2012, 9, 16);
    // 19:50:53.516
    private static LocalTime now2 = LocalTime.now();
    // 年月，例如信用卡到期：2020-09
    YearMonth now3 = YearMonth.now();
    // 重要的format和parse
    DateTimeFormatter fmt3 = DateTimeFormatter.ofPattern("yyyy-MM-dd");
    DateTimeFormatter fmt6 = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");

    @Before
    public void testBefore(){

    }

    /**
     * 重要的format和parse 转换
     * DateTimeFormatter.BASIC_ISO_DATE
     * DateTimeFormatter.ISO_DATE
     */
    @Test
    public void test01(){
        // 2020-09-19
        String format = now.format(fmt3);
        LocalDate parse = LocalDate.parse("2019-09-12", fmt3);
        // 20190916
        String format1 = now.format(DateTimeFormatter.BASIC_ISO_DATE);
        // 2019-09-16
        String format2 = now.format(DateTimeFormatter.ISO_DATE);
    }

    @Test
    public void test02(){

        // 当前年份：2019
        int year = now.getYear();
        // month 对象
        Month month = now.getMonth();
        // 当前月份：9
        int value = month.getValue();
        // 当前月份：9
        int monthValue = now.getMonthValue();
        // 该日期是当前周的第几天：1
        int value1 = now.getDayOfWeek().getValue();
        // 该日期是当前月的第几天：16
        int dayOfMonth = now.getDayOfMonth();
        // 该日期是当前年的第几天：259
        int dayOfYear = now.getDayOfYear();
        // 返回当年有多少天：365
        int i = now.lengthOfYear();
        // 返回这个月有多少天：30
        int i1 = now.lengthOfMonth();

        // 修改该日期的年份，这样的话，date.getYear() 就是 2020 了
        LocalDate date = now.withYear(2020);
        // 修改该日期的月份
        LocalDate date1 = now.withMonth(6);

        // 判断是否是闰年：fasle
        boolean leapYear = now.isLeapYear();
        // 当前日期是否比它早
        boolean before = now.isBefore(of);
        // 当前日期是否比它晚
        boolean after = now.isAfter(of);
        // 与当前日期是否一样
        boolean equal = now.isEqual(of);
        boolean equals = now.equals(of);
        // 只判断日月是否具相等
        MonthDay of1 = MonthDay.of(monthValue, dayOfMonth);
        MonthDay of2 = MonthDay.of(of.getMonthValue(), of.getDayOfMonth());
        System.out.println("of1.equals(of2)**" + of1.equals(of2) );

    }

    @Test
    public void test03(){
        // 添加、减少哦
        LocalDate date2 = now.plusDays(1);
        LocalDate date3 = now.plusWeeks(1);
        LocalDate date4 = now.plusMonths(1);
        LocalDate date5 = now.plusYears(1);
        LocalDate date6 = now.minusDays(1);
        LocalDate date7 = now.minusWeeks(1);
        LocalDate date8 = now.minusMonths(1);
        LocalDate date9 = now.minusYears(1);
        // 一周后
        LocalDate plus = now.plus(1, ChronoUnit.WEEKS);
        // 一个月后
        LocalDate plus1 = now.plus(1, ChronoUnit.MONTHS);
        System.out.println(now.plus(1, ChronoUnit.MONTHS).equals(now.plusMonths(1)) + "***********");

        // 相差多少时间， LocalDateTime 用 Duration
        String diao1 = "2019-09-12 23:23:23";
        String diao2 = "2019-09-16 23:24:23";
        // 为什么是后面减去前面，好奇怪，随便啦。不足四个小时就是四个小时，管尾巴是多少
        Duration between = Duration.between(LocalDateTime.parse(diao2,fmt6), LocalDateTime.parse(diao1,fmt6));
        long l = between.toDays();
        long l1 = between.toHours();
        System.out.println(l);
        System.out.println(l1);
        /**
         * LocalDate 用 Period: 后来在实践中发现，这个只能计算相差不到一个月的，相当于废了，所以要用另外一个 toEpochDay()  方法
         * Period between1 = Period.between(now, LocalDate.parse(diao3, DateTimeFormatter.ISO_DATE));
         * System.out.println(between1.getDays());
         */
        String diao3 = "2019-09-12";
        String diao4 = "2016-06-12";
        long days = LocalDate.parse(diao3, DateTimeFormatter.ISO_DATE).toEpochDay() - LocalDate.parse(diao3, DateTimeFormatter.ISO_DATE).toEpochDay();
        System.out.println(days);
    }

    @Test
    public void test04(){
        // 创建一个新的日期，它的值为当月的第一天：2019-10-01
        LocalDate with = now.with(TemporalAdjusters.firstDayOfMonth());
        // 创建一个新的日期，它的值为当月的最后一天
        LocalDate with5 = now.with(TemporalAdjusters.lastDayOfMonth());
        // 创建一个新的日期，它的值为下月的第一天：2019-11-01
        LocalDate with1 = now.with(TemporalAdjusters.firstDayOfNextMonth());
        // 创建一个新的日期，它的值为当年的第一天
        LocalDate with2 = now.with(TemporalAdjusters.firstDayOfYear());
        // 创建一个新的日期，它的值为当年最后一天
        LocalDate with6 = now.with(TemporalAdjusters.lastDayOfYear());
        // 创建一个新的日期，它的值为明年的第一天
        LocalDate with3 = now.with(TemporalAdjusters.firstDayOfNextYear());
        // 创建一个新的日期，例如：当月的第一个星期一为7号
        LocalDate with4 = now.with(TemporalAdjusters.firstInMonth(DayOfWeek.MONDAY));
        // 创建一个新的日期，例如：当月的第一个星期一为28号
        LocalDate with7 = now.with(TemporalAdjusters.lastInMonth(DayOfWeek.MONDAY));
        // 创建一个新的日期，例如：当月的第二个星期一为14号
        LocalDate with8 = now.with(TemporalAdjusters.dayOfWeekInMonth(2, DayOfWeek.MONDAY));
        // 获取上月的最后一个星期一
        LocalDate with9 = now.with(TemporalAdjusters.dayOfWeekInMonth(0, DayOfWeek.MONDAY));
        // 获取这个月的倒数第一个星期一：28号
        LocalDate with10 = now.with(TemporalAdjusters.dayOfWeekInMonth(-1, DayOfWeek.MONDAY));
        // 获取下个星期五的日期
        LocalDate with11 = now.with(TemporalAdjusters.next(DayOfWeek.FRIDAY));
        System.out.println(with11);
    }

    @After
    public void testAfter(){

    }

}
```

### Optional

```java
/**
 * public static <T> Optional<T> of(T value)
 * public static <T> Optional<T> ofNullable(T value)
 * public T get()
 * public boolean isPresent()
 * public void ifPresent(Consumer<? super T> consumer)
 * public <X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier) throws X
 * public void ifPresent(Consumer<? super T> consumer)
 * public Optional<T> filter(Predicate<? super T> predicate)
 * public<U> Optional<U> flatMap(Function<? super T, Optional<U>> mapper)
 * public <X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier) throws X
 * 
 * if(xxxOrder != null){
 *  if(xxxOrder.getXxxShippingInfo() != null){
 *   if(xxxOrder.getXxxShippingInfo().getXxxShipmentDetails() != null){
 *    if(xxxOrder.getXxxShippingInfo().getXxxShipmentDetails().getXxxTrackingInfo() != null){
 *     ...
 *    }
 *   }
 *  }
 * }
 *
 *
 * private String[] getFulfillments(XxxOrder xxxOrder) {
 *     return Optional.ofNullable(xxxOrder)
 *             .map((o) -> o.getXxxShippingInfo())
 *             .map((si) -> si.getXxxShipmentDetails())
 *             .map((sd) -> sd.getXxxTrackingInfo())
 *             .map((t) -> new String[]{t.getTrackingNumber(), t.getTrackingLink()})
 *             .orElse(null);
 * }
 * 
 */

public class Hehe {

    User user;
    User user2;
    User user3;
    User user4;

    @Before
    public void testBefore(){
        user2 = new User(222,"你好");
        user3 = new User(333,"你好333");
        user4 = new User(444,null);
    }

    /**
     * public Optional<T> filter(Predicate<? super T> predicate)
     * 参数是 Predicate<? super T>，她只有一个入参，出参为布尔
     * i.getName().equals("你好aaa") 如果当前条件成立，则返回当天对象optional，否则返回空opional
     */
    @Test
    public void test06(){
        final User userCCC = Optional.of(user2).filter(i -> i.getName().equals("你好aaa")).orElse(user4);
        System.out.println(userCCC);
    }

    /**
     * public<U> Optional<U> flatMap(Function<? super T, Optional<U>> mapper)
     * 留意：这里的出参类型为：Optional<U>，但是map方法的为 ? extends U
     */
    @Test
    public void test05(){
        // 明显输出：尼玛字符串
        final User userA = Optional.ofNullable(user2).flatMap(i -> Optional.of(new User(444, i.getName()))).orElse(user4);
        System.out.println(userA);
    }

    /**
     * ? super T  ：  明显为入参，入参为前面过来的Optional.ofNullable(user2)
     * ? extends U ： 出参，出参为 Optional<U>，泛型为 <U>，也就是出参类型为乜，<U>就为乜
     * public<U> Optional<U> map(Function<? super T, ? extends U> mapper)
     */
    @Test
    public void test04(){
        // 明显输出：你好
        final String nima = Optional.ofNullable(user2).map(User::getName).orElse("尼玛字符串");
        // 明显输出：尼玛字符串
        final String nima02 = Optional.ofNullable(user4).map(User::getName).orElse("尼玛字符串");
        System.out.println(nima02);
    }

    /**
     * orElseThrow
     * 提供者，没有入参，所以是 () -> {}，出参的类型是 <? extends X> ，继承了 X , 那么 X 是什么，看前面，X 是 X extends Throwable
     */
    @Test
    public void test03(){
        User userAAA = Optional.ofNullable(user).orElseThrow(() -> new NullPointerException("尼玛为空"));
        System.out.println(userAAA);
    }

    /**
     * orElseGet
     * 执行一定的行为之后再返回
     */
    @Test
    public void test02(){
        User userAAA = Optional.ofNullable(user2).orElseGet(this::createUser4);
        System.out.println(userAAA);
    }

    /**
     * orElse
     * 返回值
     */
    @Test
    public void test01(){
        // Optional.ofNullable(user)：返回一个空的optional；orElse(user2)：因为optional为空，所以返回user2
        final User userAAA = Optional.ofNullable(user).orElse(user2);
        // Optional.ofNullable(user2)：user2不为空，返回一个不为空的user2的optional；orElse(user)：optional不为空，所以返回user2
        final User userBBB = Optional.ofNullable(user2).orElse(user);
        final User userCCC = Optional.ofNullable(user3).orElse(user2);
    }

    private User createUser4(){
        System.out.println("调用user4");
        User userDiao = new User();
        userDiao.setId(444);
        return userDiao;
    }

}



class User{
    private Integer id;
    private String name;
	......
}
```

