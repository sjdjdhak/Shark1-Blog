---
title: "006 数组与切片"
date: 2026-11-07T19:48:49+08:00
draft: false
tags: ["Go语言", "基础", "入门"]
categories: ["Nodes"]
series: ["Go语言基础"]
series_order: 6
---

<!-- 封面：将 feature.png 或 feature.jpg 放在本篇 index.md 同目录下。发布前将 draft 改为 false。 -->

## 数组

> 数组是一种常见的数据类型，几乎所有的编程语言都会用到。

1. 数组中的元素必须为统一类型。

2. 声明数组时，必须指定其长度或者大小。

3. 声明语法：`[3]数据类型`

4. 在Go 语言中，不支持负数索引

```go
package main

import "fmt"

func main(){
    // 数组的声明方式
    var array [3]int = [3]int{1,2,3}
    fmt.Println(array)

    //索引访问 索引从0开始
    fmt.Println(array[0])

    // 获取数组长度 len , len-1就是数组最有一个元素的索引
    fmt.Println(len(array))

    // 更改数组中的元素
    array[0] = 666
    fmt.Println(array[0])
}
```

## 列表（切片slice）

> 列表可以动态扩容，相较于数组来说更加灵活。

```go
package main

import "fmt"

func main(){
    // 定义一个字符串切片
    var list []string
    // 在切片中添加一个元素
    list = append(list,"Hello")
    list = append(list,"World")
    // 打印切片
    fmt.Println(list)
    // 获取切片的长度
    fmt.Println("切片的长度是："len(list))

    // 修改切片中的值
    list[1] = "Window"
    fmt.Println(list)
}
```

### 切片实例化

> 除基本数据类型，其他数据类型如果只定义不赋值，那么实际的值就是`nil`
> 
> 如果要对切片进行操作，就要进行实例化。

#### make函数的用法

```go
make([]type,langth,capacity) // 参数说明 切片类型，切片长度，切片容量
```

```go
package main 

import main

func main(){
    // 这里打印为true
    var list1 []string 
    fmt.Println(list1 == nil)

    // 初始化方法1
    var list2 []string = []string{}
    fmt.Println(list2 == nil)

    // 方法2
    var list3 []string = make([]string,5)
    // 简写
    list3 := make([]string,5)
}
```

#### 为什么叫切片？

“切片（Slice）”这个名字，字面意思就是“从底层数组上切下来的一片”。在 Go 语言中，**没有任何一个切片能脱离数组独立存在**——每一个切片的背后，都一定藏着一个真正的数组（称为“底层数组”，Backing Array）。

之所以叫“切片”，是因为它最原始、最核心的创建方式，就是用下标区间 `[start:end]` 从一个现成的数组上“切”出一截来：

```go
package main 

import "fmt"

func main(){
    // 1. 先准备一整条“面包”（长度为 5 的固定数组）
    arr := [5]int{10, 20, 30, 40, 50}

    // 2. 从下标 1 切到下标 4（左闭右开，不含下标 4）
    // 得到一个切片 s，内容是 [20, 30, 40]
    s := arr[1:4]
}
```

#### 切片的排序

```go
package main

import (
    "fmt"
    "sort"
)

func main() {
    //初始化一个切片
    slice := []int{3, 2, 1, 4, 5}

    sort.Ints(slice) // 升序
    fmt.Println(slice)

    // 正确写法：先用 sort.IntSlice 包装，再传给 sort.Reverse，最后用 sort.Sort 执行
    // Reverse 只是更改了排序规则，最后要用Sort执行
    sort.Sort(sort.Reverse(sort.IntSlice(slice))) //降序
    // slices.Reverse(slice) 这个包可以直接完成
    fmt.Println(slice)

}
```
