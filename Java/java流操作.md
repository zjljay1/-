什么是 Stream？<br />Stream（流）是一个来自数据源的元素队列并支持聚合操作

元素是特定类型的对象，形成一个队列。 Java中的Stream并不会存储元素，而是按需计算。<br />数据源 流的来源。 可以是集合，数组，I/O channel， 产生器generator 等。<br />聚合操作 类似SQL语句一样的操作， 比如filter, map, reduce, find, match, sorted等。

stream：返回此集合的流。这是Collection接口下的方法，继承了Collection的List和Set都能用。<br />collect：使用Collector对此流的元素进行可变缩减操作，说人话就是，流的收集器。<br />min：求出流中的最小值。<br />max：求出流中的最大值。<br />limit：截取流中的元素并返回。<br />count：统计流中的元素个数。<br />filter：对流进行过滤。<br />sort：对流中的元素进行排序。<br />distinct：返回该流不同元素组成的流，说白了就是去重。<br />map：返回由给定函数应用于此流的元素的结果组成的流。<br />concat：合并两个流，组合成新流，哈哈，是不是很眼熟。<br />reduce：Java流中的聚合函数，这算是一个核心了，像数据库里的count 、sum 、avg 、max 、min 等函数就是一种聚合操作。<br /> 有一个列表，从中筛选出值大于30的元素
```java
List<Integer> list = new ArrayList<>();
list.add(1);
list.add(20);
list.add(40);
list.add(100);        
// 按照以前的方法   
List<Integer> list1 = new LinkedList<>();     
for (Integer value : list) {       
    if (value > 30) {          
        list1.add(value);     
    }     
}     
System.out.println(list1);     
// 使用java流
// 详细讲解一下，后面我就偷懒了
// list.stream()就是先将集合处理一下，返回我们需要的流
// filter的方法就是这里帮我们筛选值大于30的方法
// 这里用的是lambda表达式，所以要先了解一下lambda表达式，
// filter(Predicate<? super T> predicate)方法的参数是Predicate类型，Predicate是一个函数式接口
// 当把数据筛选出来后，此时还需要把它变成List的集合，毕竟现在还只是流，不是我们要的
// 这时候就需要collect方法了，这就是我说的流收集器，其实不止可以转换成List，还有Collectors.toSet()和Collectors.toMap()
List<Integer> list2 = list.stream().filter(value -> {
    return value > 30;      
}).collect(Collectors.toList());   
System.out.println(list2);

```
这里有两个实体类，一个用户类，一个任务类，下面将列举多种情况，看看用Java流是怎么处理的（并对比一下，如果没有Java流，按照传统的做法，是该怎么做）。
```java
@Data
public class User {
    /**
     * 用户id
     */
    private int id;
    
    /**
     * 用户年龄
     */
    private int age;

    /**
     * 用户名称
     */
    private String name;

    /**
     * 用户电话
     */
    private String phoneNumber;
}

```
```java
@Data
public class Task {
    /**
     * 任务id
     */
    private int id;

    /**
     * 接任务用户的用户id
     */
    private int userId;

    /**
     * 任务描述
     */
    private String taskContent;

    /**
     * 任务奖金
     */
    private int bonus;
}

```

1. 场景一：系统需要获取任务信息包括接任务的用户名称做一个数据展示，但不需要年龄和电话号码，需

要返回的大概是这种数据。
```java
public class TaskVo {
    /**
     * 任务id
     */
    private int id;

    /**
     * 用户名称
     */
    private String userName;

    /**
     * 任务描述
     */
    private String taskContent;

    /**
     * 任务奖金
     */
    private int bonus;
}

```
业务实现
```java
public class test {
    public static void main(String[] args) {
        // 先来构建数据
        List<User> users = Arrays.asList(new User(1, "小明", 17, "13879873219")
                , new User(2, "小红", 20, "13879873218")
                , new User(3, "小陈", 39, "13879873217"));

        List<Task> tasks = Arrays.asList(new Task(1, 1, "扶老奶奶过马路", 100)
                , new Task(2, 2, "帮助程序员A找到系统bug", 100)
                , new Task(3, 3, "给作者点个赞再走吧", 10000));
        // 这里将数据转换成我们需要的一个Map，以用户id为key，用户的name为value
        Map<Integer, String> userMap = users.stream().collect(Collectors.toMap(User::getId, User::getName));
        // 使用Java流将数据处理成我们需要的样子
        List<TaskVo> list = tasks.stream().map(task -> {
            // map方法的用处我已经在前面写过了，这里是具体的用法。
            // 下面是构建我需要的一个TaskVo对象并返回
            // 当然网上有很多对象转换的方式和工具，我这里对象的属性少，这样写无所谓，正式开发可能遇到那种比较多的,所以别学我，也别问我为什么不用工具，作者不想回答你并向你扔了一只狗
            TaskVo vo = new TaskVo();
            vo.setBonus(task.getBonus());
            vo.setId(task.getId());
            vo.setTaskContent(task.getTaskContent());
            // 注意这里的用户名是从userMap里取的哟
            vo.setUserName(userMap.get(task.getId()));
            return vo;
        }).collect(Collectors.toList());
        // 打印出来瞧瞧
        list.stream().forEach(System.out::println);
    }
}

```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/28163149/1671503050742-a8abc0fc-a9d0-4656-9485-302e9be6a26b.png#averageHue=%23323130&clientId=ubd221cbd-9195-4&from=paste&id=udaf49fad&originHeight=136&originWidth=753&originalType=url&ratio=1&rotation=0&showTitle=false&size=25768&status=done&style=none&taskId=u7cbba7ac-efc1-4b18-a239-bde8c7021be&title=)<br />场景二：公司需要统计一下接了奖金1000以上（包含1000）的任务的人，能接1000以上的人都是有能力的人，所以需要重点关注，看能不能发展成公司的核心用户，并按照奖金把高的排在前面，低的排后面。
```java
public class test {
    public static void main(String[] args) {
        // 构建数据
        List<User> users = Arrays.asList(new User(1, "小明", 17, "13879873219")
                , new User(2, "小红", 20, "13879873218")
                , new User(3, "小陈", 39, "13879873217")
                , new User(4, "小李", 25, "13879873217"));

        List<Task> tasks = Arrays.asList(new Task(1, 1, "给同事带杯奶茶", 1)
                , new Task(2, 2, "帮助程序员A找到系统bug", 100)
                , new Task(3, 3, "给作者点个赞再走吧", 1000)
                , new Task(4, 4, "如果喜欢收藏一下再走吧", 10000));
		// 这里是干嘛的就不说了吧
        Map<Integer, String> userMap = users.stream().collect(Collectors.toMap(User::getId, User::getName));
        // 筛选出我们需要的人才
        List<String> list = tasks.stream().filter(task -> {
            // 筛选出金额大于等于1000的
            return task.getBonus() >= 1000;
        }).sorted((t1, t2) -> {
            // 排序
            return t2.getBonus() - t1.getBonus();
        }).map(task -> {
            // 取出用户名
            return userMap.get(task.getUserId());
        }).collect(Collectors.toList());
        // 做一下打印，有能力的人都是接的什么任务我就不多说了吧，你懂的
        list.stream().forEach(System.out::println);
    }
}

```

1. 场景三：公司需要预计一下用户完成任务后本月需要结算给用户的任务奖金，这样的话，财务好早做准备。
```java
public class test {
    public static void main(String[] args) {
        List<Task> tasks = Arrays.asList(new Task(1, 1, "给同事带杯奶茶", 1)
                , new Task(2, 2, "帮助程序员A找到系统bug", 100)
                , new Task(3, 3, "给作者点个赞再走吧", 1000)
                , new Task(4, 4, "如果喜欢收藏一下再走吧", 10000));
        // 计算所有任务金额，这里就是reduce方法的作用，也就是聚合函数,就实现了类似数据库的sum函数了
        Integer money = tasks.stream().map(Task::getBonus).reduce((obj1, obj2) -> {
            // 这里的话，就可以看成reduce方法会把流中的每一个元素取出，并进行加法操作
            return obj1 + obj2;
        }).get();
        System.out.println(money);
    }
}

```
<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/28163149/1671507973792-52b2135b-8431-4601-84f9-45dc9232efb6.png#averageHue=%232d2c2c&clientId=ubd221cbd-9195-4&from=paste&id=ucd497144&originHeight=91&originWidth=382&originalType=url&ratio=1&rotation=0&showTitle=false&size=5081&status=done&style=none&taskId=ue3f7f649-8431-4a0f-be78-2f35a06df4c&title=)<br />当然reduce还有一个重载方法，有两个参数（其实还有一个重装方法，有三个参数，不过我几乎没用过，实际开发也很少用到，所以有兴趣的可以自己去了解一下）。第一个参数是作为初始值来使用的，第二个参数就和前面一个参数一样的。<br />举个实际点的例子，公司除了要预计本月看得见的任务奖金，还要预留一部分钱，以防止某个牛人在月底突然接了任务然后以极快的速度把它给完成了，到时候不给人家结算也不好是吧。
```java
public class test {
    public static void main(String[] args) {
        List<Task> tasks = Arrays.asList(new Task(1, 1, "给同事带杯奶茶", 1)
                , new Task(2, 2, "帮助程序员A找到系统bug", 100)
                , new Task(3, 3, "给作者点个赞再走吧", 1000)
                , new Task(4, 4, "如果喜欢收藏一下再走吧", 10000));
        // 这里获取值的有点细微的区别，仔细看看哦
        Integer money = tasks.stream().map(Task::getBonus).reduce(1000, (obj1, obj2) -> {
            return obj1 + obj2;
        });
        System.out.println(money);
    }
}

```
<br /> ![image.png](https://cdn.nlark.com/yuque/0/2022/png/28163149/1671508016789-c7a51c4b-aaaf-4de6-99d3-6de8b917d7a7.png#averageHue=%232d2d2c&clientId=ubd221cbd-9195-4&from=paste&id=u0bedf50e&originHeight=89&originWidth=357&originalType=url&ratio=1&rotation=0&showTitle=false&size=5344&status=done&style=none&taskId=u80aafb64-c62b-4390-8f33-a7a9016d8d3&title=)<br />reduce方法是很核心的方法，只要理解了它的用法，那么max、min、sum等等聚合函数的实现都可以通过reduce方法完成。

 
