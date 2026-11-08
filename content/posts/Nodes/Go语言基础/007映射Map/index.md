---
title: "007 映射Map"
date: 2026-11-08T10:20:52+08:00
draft: false
tags: ["Go语言", "基础", "入门"]
categories: ["Nodes"]
series: ["Go语言基础"]
series_order: 7
---

<!-- 封面：将 feature.png 或 feature.jpg 放在本篇 index.md 同目录下。发布前将 draft 改为 false。 -->

## Map

> 类似Python中的字典，不过略有不同。

1. 强类型限制，python中的类型可以混着写`{"age": 18, 1: "hello"}`. 但在Go语言中，`map` 必须在声明时严格固定 Key 和 Value 的类型，语法是 `map[Key类型]Value类型`

2.  Go语言中，访问不存在的`key` 不会报错，而是显示”零值“

3.  Map遍历是完全随机的，这是官方故意为之

4.  只声明不初始化，写入会报错。

5.  key键必须是基本数据类型，value可以是任意类型

```go
package main

import "fmt"

func main(){

    // 只声明
    var map0 map[string]string
    fmt.Println(map0 == nil)

    // 初始化的几种方法
    var map1 map[int]string = map[int]string{}
    var map2 map[int]string = make(map[int]string,5)
    
    // 声明并赋值
    map3 := map[int]string{
        1:"你好",
        2:"世界",    // 最后一行也加逗号
    } 
    fmt.Println(map1)
    fmt.Println(map2)
    fmt.Println(map3)
    
    //设置值
    map1[3] = "你的眼睛在说我愿意"
    fmt.Println(map1[3]) //取值
    
    //删除
    delete(map1,3)
    fmt.Println(map1) 
    
    //取一个不存在的值
    str := map1[100]
    fmt.Println(str)  // 打印零值
    // 判断是否存在
    str,ok := map1[100]   //ok 为true表示存在值，false表示不存在
    fmt.Println(str,ok)
    
    
}
```
