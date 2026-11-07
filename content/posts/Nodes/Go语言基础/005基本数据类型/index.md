---
title: "005 基本数据类型"
date: 2026-11-07T17:28:12+08:00
draft: false
tags: ["Go语言", "基础", "入门"]
categories: ["Nodes"]
series: ["Go语言基础"]
series_order: 5
---

<!-- 封面：将 feature.png 或 feature.jpg 放在本篇 index.md 同目录下。发布前将 draft 改为 false。 -->

## Go 语言基本数据类型

> 在Go语言中，基本数据类型共有5类：整数/浮点数/复数/布尔值/字符串

### 整型

整数分为**有符号数**和**无符号数**

* 有符号数`int`：有正负号的整数，负数以补码的形式存储. 

* 无符号数`uint`：只有正整数

> 有两个特殊的数据类型 
> 
> byte (单字节字符)  = uint8 , 二者完全等价.  但是所表示的含义不同, uint8表示8位的二进制数 , 而byte则表示Ascii码.
> 
> rune (多字节字符) = int32 , 二者完全等价.  用来表示一个 **Unicode 字符（码点）**，可以涵盖中文字符（如 `'中'`）、日文、Emoji（如 `'🚀'`）等任意字符。

```go
package main 

import "fmt"

func main(){
    // 有符号数 int
    // 当声明具体类型时,默认类型是int,int类型的大小取决于当前的系统.
    var num1 = 12; 
    fmt.Printf("%T\n",num1) // 打印数据类型
    
    // int 后面的数字指二进制的位数
    var num2 int8 = 12
    var num3 int16 = 1
    var num4 int32 = 1
    var num5 int64 = 1
    
    //无符号数 uint 
    var unum1 uint8 = 1
    var unum2 uint16 = 1
    var unum3 uint32 = 1
    var unum1 uint64 = 1
    
}
```

#### 字符型

1. 在Go语言中, 字符的本质就是一个整数, 直接输出时, 对应的是该字符的码值

2. 字符类型是可以进行运算的 因为完全等价与uint8 和 int32

3. 格式化输出 %c 时，会输出该数字对应的 unicode 字符

4. 字符型使用单引号, 字符串使用双引号.



```go
package main

import "fmt"

func main(){
    // 使用unicode码 打印中国
    fmt.Printf("%c %c \n",20013,22269) 
}
```





### 浮点型

> 若没有显示声明,默认为float64

Go语言中有两种浮点类型

* float32: 最大范围约为3.4e38，可以使用常量定义：`math.MaxFloat32`

* float64: 最大范围约为 1.8e308，可以使用一个常量定义：`math.MaxFloat64`



### 字符串类型

> 字符的赋值是单引号，字符串的赋值是双引号



#### 转义字符

| 符号  | 作用         |
| --- | ---------- |
| \t  | 制表符        |
| \n  | 换行符        |
| \   | 转义字符,例如 \" |
| \r  | 回到行首       |

#### 多行字符串

> 使用反引号````包裹 , 会原样输出字符串.  也不会执行转义.

```go
package main

import "fmt"

func main(){
    fmt.Println(`
    你好
    世界
    Hello
    World \n

    `)
}
```

### 布尔类型

1. Go语言中, 不允许将整型转换成布尔型

2. 布尔类型 默认值为 false

3. 布尔类型无法参与数值运算,也无法与其他类型进行转换



### 零值问题

> Go语言中，基本数据类型之声明不赋值，默认为零值。
> 
> 例如： int 默认为0 / bool默认为false / 字符串默认为 “ ” / float默认为0



```go
package main

import "fmt"

func main() {
  var a1 int
  var a2 float32
  var a3 string
  var a4 bool

  fmt.Printf("%#v\n", a1)
  fmt.Printf("%#v\n", a2)
  fmt.Printf("%#v\n", a3)
  fmt.Printf("%#v\n", a4)
}
```
