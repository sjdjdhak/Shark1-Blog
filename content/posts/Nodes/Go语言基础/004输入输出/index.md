---
title: "004 输入输出"
date: 2026-11-07T15:05:35+08:00
draft: false
tags: ["Go语言", "基础", "入门"]
categories: ["Nodes"]
series: ["Go语言基础"]
series_order: 4
---

<!-- 封面：将 feature.png 或 feature.jpg 放在本篇 index.md 同目录下。发布前将 draft 改为 false。 -->

## 输出Printf / Println

```go
package main

import "fmt"

func main(){
    // 带换行的输出
    fmt.Println("Hello World")

    // 格式化输出,支持多种占位符
    fmt.Printf("%s\n","Hello World")     // 字符串占位符
    fmt.Printf("%d\n", 123)     // 整数占位符
    fmt.Printf("%c\n",'a')    // 打印字符
    fmt.Printf("%f\n", 3.1415926)    // 浮点数占位符
    fmt.Printf("%.2f\n", 3.1415926)    // 浮点数保留2位小数
    fmt.Printf("%T %T\n","Hello World",123)     // 打印数据类型           
    fmt.Printf("%v\n","Hello World")    // 任意类型占位符
    fmt.Printf("%#v\n","")     // 带格式的输出，常用于空字符串

    // 格式化后赋值给变量
    name := fmt.Sprintf("%s","Shark1")
    fmt.Println(name)

}
```

## 输入Scan

```go
package main


import "fmt"

func(){
    fmt.Println("请输入您的名字：")
    var name string
    fmt.Scan(&name)    //这里传入的是指针
    fmt.Println(name)
}
```
