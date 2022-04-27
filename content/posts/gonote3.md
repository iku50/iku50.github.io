+++
title = "Go笔记3"
date = "2022-04-27T19:26:25+08:00"
author = "[Wizzz]"
authorTwitter = "" #do not include @
cover = ""
tags = ["golang"]
keywords = ["", ""]
description = "go笔记3：函数与变量作用域"
showFullContent = false
readingTime = false
hideComments = false
+++

# go语言学习笔记3

这篇是函数相关与变量作用域相关

## go语言函数

go语言最少有个main函数，go语言标准库里有不少内置函数

定义

```go
func function_name( [parameter list] ) [return_types]{
    /*函数体*/
}
```

+ `func`:函数由此开始声明
+ `function_name`:函数名称，列表和返回值类型构成了函数签名
+ `parameter list`:参数列表，参数就像占位符，函数调用时可以把值传进去
+ `return_types`:返回类型，当函数不需要返回值那就不是必须的
+ 函数体：函数定义的代码集合

### 可变参数

任意多个参数在前面加上`...`就行

本质上是一个数组，可以像使用数组一样使用它

### 返回多值

go的函数可以返回多个值

```go
func swap(x, y string) (string, string) {
   return y, x
}
```

不想用某个值就在接收时用`_`省略掉就好了

```go
file, _ := os.Open("/usr/tmp")
```

 ### 值传递

指在调用函数时将实际参数复制一份到函数中，这样若对参数修改，不影响到实际参数

默认情况下go就用的是值传递

但有时我们会像在函数内部对实际参数进行修改，所以有了引用传递

### 引用传递

指在调用函数时将实际参数的地址传入函数，那么函数中对参数进行的修改将影响到实际参数

引用传递指针参数进函数内

```go
func swap(x *int, y *int) {
   var temp int
   temp = *x    /* 保持 x 地址上的值 */
   *x = *y      /* 将 y 值赋给 x */
   *y = temp    /* 将 temp 值赋给 y */
}
```

调用时：

```go
swap(&a, &b)
```

但实际上go所有的传递都是值传递，像slice或map等看似是引用传递但实际上它们本身就是指针，函数将该指针复制了一份，但它们指向的位置不变所以能够借指针改变值

### 函数作为实参

go中可以很灵活地创建函数并作为另一个函数的实参

如下：

```go
getSquareRoot := func(x float64) float64 {
   return math.Sqrt(x)
}
```

该函数返回了一个函数作为变量，使用也是完全没问题的

```go
fmt.Println(getSquareRoot(9))
```

函数作参还可以轻易实现回调函数

```go
package main
import "fmt"

// 声明一个函数类型
type cb func(int) int

func main() {
    testCallBack(1, callBack)
    testCallBack(2, func(x int) int {
        fmt.Printf("我是回调，x：%d\n", x)
        return x
    })
}

func testCallBack(x int, f cb) {
    f(x)
}

func callBack(x int) int {
    fmt.Printf("我是回调，x：%d\n", x)
    return x
}
```

### 闭包

即匿名函数，是一个“内联”语句或表达式，优越性在于可以直接使用函数内的变量而不必申明

以下实例中创建了函数`getSequence()`并返回另一个函数，该函数目的是在闭包中递增`i`变量

```go
package main

import "fmt"

func getSequence() func() int {
   i:=0
   return func() int {
      i+=1
     return i  
   }
}

func main(){
   /* nextNumber 为一个函数，函数 i 为 0 */
   nextNumber := getSequence()  

   /* 调用 nextNumber 函数，i 变量自增 1 并返回 */
   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
   
   /* 创建新的函数 nextNumber1，并查看结果 */
   nextNumber1 := getSequence()  
   fmt.Println(nextNumber1())
   fmt.Println(nextNumber1())
}
```

闭包函数中的返回值不用谢具体形参名称

### 方法

方法就是一个包含了接收者的函数，接收者可以是命名类型或结构体类型的一个值或一个指针，所有给定类型的方法属于该类型的方法集，语法格式为

```go
func (variable_name variable_data_type) function_name() [return_type]{
   /* 函数体*/
}
```

实例如下：

```go
package main

import (
   "fmt"  
)

/* 定义结构体 */
type Circle struct {
  radius float64
}

func main() {
  var c1 Circle
  c1.radius = 10.00
  fmt.Println("圆的面积 = ", c1.getArea())
}

//该 method 属于 Circle 类型对象中的方法
func (c Circle) getArea() float64 {
  //c.radius 即为 Circle 类型对象中的属性
  return 3.14 * c.radius * c.radius
}
```

go没有面向对象，像常见的java或c++都是编译器隐式的给函数加一个this指针，但在go里这个this指针需要明确指出

还有就是，当我们没有强制使用指针调用时，go编译器会自动帮我们去指针或解引用来满足接收方法的需求，我们不需要严格遵守这些

如果在方法里想要改变结构体成员值，需要对方法使用指针

```go
func (c *Circle) changeRadius(radius float64)  {
   c.radius = radius
}
```

引用类型改变值则需要转指针

```go
func change(c *Circle, radius float64)  {
   c.radius = radius
}
```

## 变量作用域

作用域为已声明标识符的常量，类型，变量，函数或包在源码中的作用范围

可以在三个地方声明

+ 函数内定义的叫局部变量
+ 函数外定义的叫全局变量
+ 函数定义中的变量叫形式参数

### 局部变量

作用域只在函数体内，参数和返回值变量也是局部变量

### 全局变量

函数体外声明的变量，可以在整个包甚至外部包（被导出后使用）

全局变量与局部变量名称可以相同，但局部变量会被优先考虑

### 形式参数

会作为函数的局部变量使用

### 默认值

| 数据类型  | 全局变量 |
| :-------: | :------: |
|   `int`   |    0     |
| `float32` |    0     |
|  pointer  |   nil    |

可以用花括号控制变量作用域，花括号中变量是单独作用域，同名变量会覆盖外层（强龙不压地头蛇）

