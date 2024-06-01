更多详情： [https://www.runoob.com/java/java-string.html](https://www.runoob.com/java/java-string.html)
<a name="DXx3c"></a>
## 常用方法

- 判断一个字符串是否包含指定字符或字符串 string.**contains**(String s)
```java
public boolean contains(CharSequence chars)
// 参数
// chars -- 要判断的字符或字符串。
// 返回值
// 如果包含指定的字符或字符串返回 true，否则返回 false。
```
实例：以下实例判断 Runoob 中是否包含字符或字符系列：
```java
public class Main {
    public static void main(String[] args) {
        String myStr = "Runoob";
        System.out.println(myStr.contains("Run"));
        System.out.println(myStr.contains("o"));
        System.out.println(myStr.contains("s"));
    }
}
// 输出:
// true
// true
// false
```

- 连接字符串  string.**concat**(string s);
```java
// String 类提供了连接两个字符串的方法：
string1.concat(string2);
// 返回 string2 连接 string1 的新字符串。也可以对字符串常量使用 concat() 方法，如：
"我的名字是".concat("Runoob");
// 更常用的是使用'+'操作符来连接字符串，如：
"Hello," + " runoob" + "!"
// 结果如下:
"Hello, runoob!"
```
```java
public class StringDemo {
    public static void main(String args[]) {     
        String string1 = "菜鸟教程网址：";     
        System.out.println("1、" + string1 + "www.runoob.com");  
    }
}
//输出
// 1、菜鸟教程网址：www.runoob.com
```

- 创建格式化字符串 ： **String.format**
   - 我们知道输出格式化数字可以使用 printf() 和 format() 方法。

String 类使用静态方法 format() 返回一个String 对象而不是 PrintStream 对象。<br />String 类的静态方法 format() 能用来创建可复用的格式化字符串，而不仅仅是用于一次打印输出。如下所示：
```java
System.out.printf("浮点型变量的值为 " +
                  "%f, 整型变量的值为 " +
                  " %d, 字符串变量的值为 " +
                  "is %s", floatVar, intVar, stringVar);
```
你也可以这样写
```java
String fs;
fs = String.format("浮点型变量的值为 " +
                   "%f, 整型变量的值为 " +
                   " %d, 字符串变量的值为 " +
                   " %s", floatVar, intVar, stringVar);
```

- 检测字符串是否匹配给定的正则表达式 matches()

语法<br />public boolean matches(String regex)<br />参数

- **regex** -- 匹配字符串的正则表达式。

返回值<br />在字符串匹配给定的正则表达式时，返回 true。
```java
public class Test {
    public static void main(String args[]) {
        String Str = new String("www.runoob.com");

        System.out.print("返回值 :" );
        System.out.println(Str.matches("(.*)runoob(.*)"));
        
        System.out.print("返回值 :" );
        System.out.println(Str.matches("(.*)google(.*)"));

        System.out.print("返回值 :" );
        System.out.println(Str.matches("www(.*)"));
    }
}
// 返回值 :true
// 返回值 :false
// 返回值 :true
```
