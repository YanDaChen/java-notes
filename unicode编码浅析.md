---
title: unicode编码浅析
date: 2020-03-19 20:00:43
tags:[原理]
---

### 一、unicode概述

​	unicode其为根据唯一码点（编号）确定字符，不论编码形式为utf8，16，32，其unicode码点都一致，只是每个编码的规则和字节内容存在差异。

### 二、utf-8浅析

​	utf8为可变字节，其结构如下：
| 字节数 | 码点 | 编码结构（2进制） |
|:-|:-:|:-|
| 1字节 | U-00000000 - U-0000007F | 0xxxxxxx  |
| 2字节 | U-00000080 - U-000007FF | 110xxxxx 10xxxxxx  |
| 3字节 | U-00000800 - U-0000FFFF | 1110xxxx 10xxxxxx 10xxxxxx  |
| 4字节 | U-00010000 - U-001FFFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx  |
| 5字节 | U-00200000 - U-03FFFFFF | 111110xx 10xxxxxx 10xxxxxx 10xxxxxx 10xxxxxx  |
| 6字节 | U-04000000 - U-7FFFFFFF | 1111110x 10xxxxxx 10xxxxxx 10xxxxxx 10xxxxxx 10xxxxxx  |

通过数头字节有几个1确定有几个字节，然后以如下规则替换xxxxx：

```javascript
//以js为荔子：
"衍".codePointAt(0).toString(16);//码点为884d
parseInt("884d",16).toString(2);//二进制为1000100001001101
// 对照上述码点为3字节区间内，将二进制从右往左填充，不足补0
// 1110xxxx 10xxxxxx 10xxxxxx（结构）
//     1000   100001   001101（二进制补足）
// 11101000 10100001 10001101（结果）
//    e8      a1        8d   （十六进制）
// 所以“衍”字的utf-8字节为e8 a1 8d。
```

同理，有字节转汉字就是上述过程反转而已。

### 三、utf-16浅析

​	使用二或四个字节为每个字符编码，有顺序问题。

| 字节数 | 码点 | 编码结构（2进制） |
|:-|:-:|:-|
| 2字节 | U-00000000 - U-0000FFFF | 直接原本表示  |
| 4字节 | U-0000FFFF - U-0010FFFF | 110110xx xxxxxxxx  110111xx xxxxxxxx |

+ 字节如果以D800~DBFF 开头，说明为4字节的前16位。
+ 字节如果以DC00~DFFF 开头，说明为4字节的后16位。
+ 其他开头的为单16位。

4字节情况的处理方式如下：

```javascript
//还是以js为荔子：
"\ud83c\udde8"=="🇨";//true
"🇨".codePointAt(0).toString(16);//1f1e8
"\u{1f1e8}"=="🇨";//true
//码点为1f1e8位于4字节区域，进行如下计算：
// 1.码点减去0x10000
0x1f1e8-0x10000;//61928，转成2进制为1111000111101000
// 2.对照上述结构进行填充
// 110110xx xxxxxxxx  110111xx xxxxxxxx（结构）
//            111100        01 11101000（二进制补足）
// 11011000 00111100  11011101 11101000（结果）
//     d8      3c         dd       e8  （十六进制）
//所以🇨这个字符的utf-16字节为d8 3c dd e8和上述结果一致。
```

同理，反转逻辑相反。

tips：所以js的string是以utf-16的形式存储的。

### 四、else

国内百度搜索utf8在线转换很多都是转给你码点，并不是真正的utf8字节码。

国外正确转换：<https://mothereff.in/utf-8> 

附带一个unicode大全网站：<https://www.qqxiuzi.cn/zh/unicode-zifu.php> 

以上。