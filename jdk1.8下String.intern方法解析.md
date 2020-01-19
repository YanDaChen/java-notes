#### 1. 总起

jdk1.8没有永久代有元空间，jdk1.7字符串常量池（StringTable）由永久代移入堆内。

主原则：

1. 字符串常量池只持有堆内引用。
2. new 一定会新建一个新的对象。
3. intern方法会返回常量池已存在字符串的引用，不存在则在常量池记录<font color="red">当前字符串的引用</font>，并返回当前字符串的引用。
4. 字面量（如String str = "aaa"的aaa）会产生一个新的String对象，并且常量池引用它，如果此字面量之前不存在的话。如果存在直接返回引用。
5. 编译器对<font color="red">字面量</font>的+有进行优化，如String str = "a"+"b";会直接编译成String str = "ab"（不会因为这条语句在堆内产生"a""b"两个对象）。

#### 2.案例

```java
        String s0 = new StringBuilder().append("he").append("llo").toString();
        System.out.println(s0.intern() == s0);
//true，原因见3。intern调用时候常量池引用不存在，则指向自己。
```

```java
        String s0 = new StringBuilder().append("he").append("llo").toString();
        System.out.println("hello" == s0);//false
        System.out.println(s0.intern() == s0);//false
//原因先4后3，字面量直接会建立常量池引用。和s0不等。
```

```java
        String s1 = new StringBuilder().append("ja").append("va").toString();
        System.out.println(s1.intern() == s1);//false
//原因是java已经被声明过了。sun.misc.Version会被加载，里面包含java字面量
```

```java
        //1
        String str1 = new String("1234");
        String str2 = new String("1234");
        System.out.println("new String()==：" + (str1 == str2));//false，见2
        //2
        String str3 = "1234";
        String str4 = "1234";
        System.out.println("常量字符串==：" + (str3 == str4));//true，见4
        //3
        String str5 = "1234";
        String str6 = "12" + "34";
        System.out.println("常量表达式==：" + (str5 == str6));//true，见5
        //4
        String str7 = "1234";
        String str8 = "12" + 34;
        System.out.println("字符串和数字相加的表达式==：" + (str7 == str8));//true，见5
        //5
        String str9 = "12true";
        String str10 = "12" + true;
        System.out.println("字符串和Boolen相加表达式==：" + (str9 == str10));//true，见5
        //6
        final String val = "34";
        String str11 = "1234";
        String str12 = "12" + val;
        System.out.println("字符串和常量相加的表达式==：" + (str11 == str12));//true，见5
        //7
        String str13 = "1234";
        String str14 = "12" + getVal();
        System.out.println("字符串和函数得来的常量相加表达式==：" + (str13 == str14));//fasle，见5
```

