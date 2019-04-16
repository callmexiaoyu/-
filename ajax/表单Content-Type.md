# ajax数据类型

[TOC]

***
## ajax提交的数据类型以及对应的格式

|  |                  |      |
| ----------------------------------- | ---------------- | ---- |
| Content-Type                        | 数据类型         |      |
| text/plain;charset=UTF-8            | 字符串或二进制流 |      |
| application/x-www-form-urlencoded   | 键值对字符串     |      |
| application/json                    | json字符串       |      |
| multipart/form-data                 | 二进制流         |      |

`请求头的数据类型Content-Type与其传送的数据类型不匹配时，转为multipart/form-data二进制流`

