---
title: "003 Go变量声明"
date: 2026-11-06T17:14:22+08:00
draft: false
tags: ["Go语言", "基础", "入门"]
categories: ["Nodes"]
series: ["Go语言基础"]
series_order: 3
---

## 

## 变量

### 变量定义

```go
package main
func main(){
    // 先声明，再赋值
    var name string
    name = "鲨鱼"

    //声明并赋值
    var name string = "鲨鱼"
    
    //省略类型
    var name2 = "Shark1"
    fmt.Println(name2)
    
    //短声明符号
    name3 := Shark1"
}
```



### 定义多个变量

```go
package main

import "fmt"

func main() {
	// 同时定义多个变量
	var name1, name2, name3 string

	// 同时定义多个变量并赋值
	var a1, a2 string = "你好", "世界"

	// 短声明定义多个变量并赋值
	b1, b2 := "Hello", "World"

    //局部变量必须使用，否则会报错。
	fmt.Println(name1, name2, name3)
	fmt.Println(a1, a2)
	fmt.Println(b1, b2)

}

```

### 全局变量与局部变量

1. 定义在函数体内部的变量（包括main函数），定义后就必须使用。 

2. 定义在函数外部的变量就是全局变量，可以只定义不使用。

```go
package main
import "fmt"

// 全局变量，可以只定义，不使用
var userName = "Shark1"

func main(){
    // 局部变量
    var name = "shark"
    // 局部变量必须要使用
    fmt.println(name)
}
```

## 常量

定义常量所用到的关键字是`const`，常量在定义的时候就要赋值，并且定义后，就无法再修改了。

```go
package main
import "fmt"

func main(){
    const name = "shark"
}
```

## 访问外部变量

在Go语言中，想要实现跨包访问必须遵守以下几点

1. 命名规范: 变量/函数/方法/属性等，在命名时候，首字母必须大写。

2. Mod文件: 确保根目录下有`go.mod`文件。创建命令：`go mod init 项目名`

3. 独立包: 以“独立包”的形式去 `import` ，就必须为它创建一个单独的子目录。

> **包名由所在目录决定，导入路径也是按目录定位** Go 中的 `import` 导入的是**目录（文件夹路径）**，而不是具体的文件名。



创建一个version文件 

```go
package version

// 可以被外部访问的变量
var Version string = "1.1.0"

//不可以被外部访问的变量
var version string = "1.2.0"
```

main文件

```go
package main

import (
    "fmt"
    "Gostudy/version"
)

func main(){
    // 可以访问
    fmt.Println(version.Version)
    
    // 不可以访问
    fmt.Println(version.version)
}
```
