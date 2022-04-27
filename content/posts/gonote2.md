+++
title = "Go笔记2"
date = "2022-04-27T19:16:09+08:00"
author = "[Wizzz]"
authorTwitter = "" #do not include @
cover = ""
tags = ["golang", ""]
keywords = ["", ""]
description = "go笔记2：运算符，条件语句和循环语句"
showFullContent = false
readingTime = false
hideComments = false
+++

#	go语言学习笔记2

第二份

## go语言运算符

go里内置的运算符有：

+ 算术运算符
+ 逻辑运算符
+ 关系运算符
+ 位运算符
+ 赋值运算符
+ 其他运算符

### 算术运算符

设A为10，B为20

| 运算符 | 描述 | 实例       |
| ------ | ---- | ---------- |
| +      | 相加 | A+B输出30  |
| -      | 相减 | A-B输出-10 |
| *      | 相乘 | A*B输出200 |
| /      | 相除 | B/A输出2   |
| %      | 求余 | B%A输出0   |
| ++     | 自增 | A++输出11  |
| --     | 自减 | A--输出9   |

### 关系运算符

| 运算符 | 描述                              |
| ------ | --------------------------------- |
| ==     | 两值相等返回true，不相等返回false |
| !=     | 两值不等返回true，相等返回false   |
| >      | 左边值更大返回true，否则false     |
| <      | 右边值更大返回true，否则false     |
| >=     | 右边值更小返回false，否则true     |
| <=     | 左边值更小返回false，否则true     |

### 逻辑运算符

| 运算符 | 描述 |
| ------ | ---- |
| &&     | 与   |
| \|\|   | 或   |
| ！     | 非   |

### 位运算符

go支持&、|、^、<<、>>这几种位运算符

### 赋值运算符

支持所有算术运算符和位运算符对应的赋值运算符

### 其他运算符

| 运算符 | 描述     |
| ------ | -------- |
| &      | 返回地址 |
| *      | 指针变量 |

### 运算符优先级

二元运算符方向均是从左至右

| 优先级 | 运算符           |
| ------ | ---------------- |
| 5      | * / % >> << & ^ |
| 4      | + - \| ^          |
| 3      | == != < <= > >=  |
| 2      | &&               |
| 1      | \|\|             |

当然，可以通过括号临时提升某个表达式的优先级
*注：go的自增自减只能作为表达式使用而不能作为赋值语句*

## 条件语句

### if语句

```go
if b:="right";a<20 {
       /* 如果条件为 true 则执行以下语句 */
       fmt.Println("a 小于 20\n",b )
   }
```

go的if强大的一个地方就是能在条件判断语句前声明生命周期只在该条件语句内的变量，使用;进行分割

条件不需要用()包含，但必须存在{}，左括号必须在if或else的同一行,在有返回值的函数中，最终的return不能在条件语句中

### if else

```go
if age == 25 {
    fmt.Println("true")
} else if age < 25 {
    fmt.Println("too small")
} else {
    fmt.Println("too big")
}
```

### if嵌套语句

略

### switch case语句

go里的switch不用加break，默认情况下该逻辑语句的实现自带break，匹配成功后就不会执行其他case，但如果要执行后面的语句，可以使用 `fallthrough`

其中匹配的类型并不局限于常量或整数，但必须是相同的类型，或结果位相同类型的表达式，可以同时测试多个值，用逗号分开即可
`case val1,val2,val3`

#### type switch

switch case还可被用于type-switch来判断某个interface中实际储存的变量类型

```go
switch x.(type){
    case type:
       statement(s);    
    case type:
       statement(s); 
    /* 你可以定义任意个数的case */
    default: /* 可选 */
       statement(s);
}
```

#### fallthrough

强制执行下一条case而不去管该case是否为true

```go
switch {
    case true:
            fmt.Println("1、case 条件语句为 true")
            fallthrough
    case false:
            fmt.Println("2、case 条件语句为 false")
```

上面两条都会被执行

### select语句

go中独有的一个控制结构，类似于用于通信的switch语句
每个case都必须是一个通信操作，要么发送要么接收
select随机执行一个可运行的case，如果没有case可运行就会阻塞直到有case可运行为止，一个默认的子句应该总是可运行的

select的语法如下

```go
select {
    case communication clause  :
       statement(s);    
    case communication clause  :
       statement(s);
    /* 你可以定义任意数量的 case */
    default : /* 可选 */
       statement(s);
}
```

语法特征是

+ 每个case都必须是一个通信
+ 所有channel表达式都会被求值
+ 所有被发送的表达式都会被求值
+ 如果任意某个通信可以进行就执行，其他被忽略
+ 如果有多个case可以运行，它会随机公平地选出一个执行，其他不会执行，否则：
  + 如果有default语句就执行该语句
  + 没有的话就将阻塞至某个通信可以运行为止，go不会重新对channel或值进行求值

实例：

```go
  var c1, c2, c3 chan int
  var i1, i2 int
  select {
    case i1 = <-c1:
      fmt.Printf("received ", i1, " from c1\n")
    case c2 <- i2:
      fmt.Printf("sent ", i2, " to c2\n")
    case i3, ok := (<-c3):  // same as: i3, ok := <-c3
      if ok {
        fmt.Printf("received ", i3, " from c3\n")
      } else {
        fmt.Printf("c3 is closed\n")
      }
    default:
      fmt.Printf("no communication\n")
  }  
```

## 循环语句

### for循环

有三种形式，只有一种使用分号

`for init; condition; post { }`

`for condition { }`

`for { }` *无限循环*

`init`：赋值表达式，给控制变量赋初值
`condition`：关系表达式或逻辑表达式，循环控制条件
`post`：一般为赋值表达式，给控制变量增量或减量

执行过程：先执行 `init`，然后用 `condition`判定是否满足给定条件，值为真则执行循环体内语句，然后执行 `post`并再次对 `condition`进行判定，直到不满足条件则跳出循环

for的 `range`格式可对slice，map，数组，字符串进行迭代循环，格式如下：

```go
for key, value := range oldMap {
    newMap[key] = value
}
```

而对于无限循环，要中止就按 `ctrl+c`

#### for—each range循环

这种循环可以对字符串，数组，切片等进行迭代输出元素

```go
strings := []string{"google", "runoob"}
        for i, s := range strings {
                fmt.Println(i, s)
        }


        numbers := [6]int{1, 2, 3, 5}
        for i,x:= range numbers {
                fmt.Printf("第 %d 位 x 的值 = %d\n", i,x)
        }  
```

### 循环嵌套

略

### 循环控制语句

go支持一下几种循环控制语句：

`break`

`continue`

`goto` *这个小心用*

