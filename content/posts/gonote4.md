+++
title = "Gonote4"
date = "2022-05-09T19:32:45+08:00"
author = "[Wizzz]"
authorTwitter = "" #do not include @
cover = ""
tags = ["golang"]
keywords = ["", ""]
description = "go笔记4：数组，指针，结构体与切片"
showFullContent = false
readingTime = false
hideComments = false
+++

## 数组

具有相同唯一类型的一组已编号且长度固定的数据项类型，可以是任意的原始类型或自定义类型

可以通过索引来读取

### 声明数组

需要指定元素类型及元素个数，一维数组格式如下：

```go
var variable_name [SIZE] variable_type
```

实例：

```go
var balance [10] float32
```

### 初始化

```go
var balance = [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
```

或

```go
var balance = [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
```

数组长度不确定就用`...`代替

如果设置了长度，还可以通过指定下标初始化元素

```go
balance := [5]float32{1:2.0,3:7.0}
```

如果忽略了`[]`中的数字不设置数组大小，go会根据元素个数来设置数组大小

初始化数组长度后，元素可以不初始化，但未进行数组大小初始化的数组初始化结果元素大小就为多少

### 访问数组元素

跟其他语言一样

### 多维数组

如下：

```go
var variable_name [SIZE1][SIZE2]...[SIZEN] variable_type
```

创建各个维度元素不一致的多维数组

```go
package main

import "fmt"

func main() {
    // 创建空的二维数组
    animals := [][]string{}

    // 创建三一维数组，各数组长度不同
    row1 := []string{"fish", "shark", "eel"}
    row2 := []string{"bird"}
    row3 := []string{"lizard", "salamander"}

    // 使用 append() 函数将一维数组添加到二维数组中
    animals = append(animals, row1)
    animals = append(animals, row2)
    animals = append(animals, row3)

    // 循环输出
    for i := range animals {
        fmt.Printf("Row: %v\n", i)
        fmt.Println(animals[i])
    }
}
```

*注：初始化时右括号不能单独一层*

### 向函数传递数组

需要在函数定义时声明形参为数组

```go
void myFunction(param [10]int){
```

或

```go
void myFunction(param []int){
```

另外，若数组元素一多，由于其值传递的特性，对内存是个很大的开销，这里就要使用数组指针，但要注意传数组不能借此修改外部数组，但传指针就可能修改了

go数组作为参数传递的也是副本，函数内修改不改变原来数组

数组为值，长度是其类型的一部分，在函数参数中是值传递，修改对调用者不可见

而后面会讲slice（切片）类似于指针传递，函数内修改对调用可见，可以修改外部数据

## 指针

指针声明格式：

```go
var var_name *var-type
```

### 使用指针

+ 定义指针变量
+ 为指针变量赋值
+ 访问指针变量中指向地址的值

在指针类型前加上`*`就可以获取指针所指向的内容

### 空指针

当指针定义后没有分配到任何变量时值为`nil`，即空指针

### 指针数组

声明：

```go
var ptr [MAX]*int;
```

*注：遍历指针数组时若要用range循环时注意一下*

### 指向指针的指针

格式：

```go
var ptr **[type]
```

### 指针作为函数参数

允许向函数传递指针，只需要在函数定义参数设置好就行

## 结构体

可以在不同项定义不同的数据类型

结构体是由一系列具有相同类型或不同类型的数据构成的数据集合

定义：

```go
type struct_variable_type struct {
   member definition
   member definition
   ...
   member definition
}
```

一旦定义了结构体类型就能用于变量的声明

```go
variable_name := structure_variable_type {value1, value2...valuen}
```

或

```go
variable_name := structure_variable_type { key1: value1, key2: value2..., keyn: valuen}
```

### 访问结构体成员

使用`.`操作符

### 结构体作为参数

也是可以的

### 结构体指针

当然也可以了，访问结构体成员是使用`.`操作符

在结构体指针中，在其对应的内存地址取成员值时，会变成变成引用传递，这就很爽了，因为如果要写一个修改结构体内值的函数，那么需要将修改后的值返回出来然后重新赋值，但只要用结构体指针就可以直接对其进行修改

*结构体属性中首字母大写相当于public，小写相当于private，是相对于包来说的*

## 切片（slice）

go切片是对数组的抽象

数组长度不可变，在特定场景就不太适用，go提供了灵活而功能强悍的内置类型切片（动态数组），与数组相比其长度不固定，可以追加元素，追加时可能使其容量增大

切片实际上是获取了数组的某一部分，len切片<=cap切片<=len数组

由三部分组成：指向底层数组的指针，len，cap

声明（即指定一个未指定大小的数组）

```go
var identifier []type
```

可以使用make()函数创建切片

```go
var slice1 []type = make([]type, len)
```

或

```go
slice1 := make([]type, len)
```

也可以指定初始容量

```go
make([]T, length, capacity)
```

### 切片初始化

可以如下直接初始化

```go
s :=[] int {1,2,3 } 
```

或者引用其他数组来初始化切片

```go
s := arr[startIndex:endIndex] 
```

表明将arr中从下标`startIndex`到`endIndex-1`下的元素创建为一个新的切片

省略`startIndex`表示从第一个元素开始，省略`endIndex`表示一直到最后一个元素

新切片的大小为`endIndex-startIndex`，容量为:底层数组容量`-startIndex`

### len()和cap()函数

切片可索引，可以由len()方法获取长度

切片也提供了计算容量的方法cap()测量切片最长可以达到多少

### 空/nil 切片

一个切片在未初始化之前默认为nil，长度为0，容量为0

切片还有nil切片和空切片，nil切片意味着指向底层数组的指针为nil，而空切片对应的指针是个实际的地址

nil切片表示不存在的切片，空切片表示一个空集合，它们用处不一

### 切片截取

可以设置下限及上限来设置截取切片`[lower-bound:upper-bound]`,实例如下：

```go
package main

import "fmt"

func main() {
   /* 创建切片 */
   numbers := []int{0,1,2,3,4,5,6,7,8}  
   printSlice(numbers)

   /* 打印原始切片 */
   fmt.Println("numbers ==", numbers)

   /* 打印子切片从索引1(包含) 到索引4(不包含)*/
   fmt.Println("numbers[1:4] ==", numbers[1:4])

   /* 默认下限为 0*/
   fmt.Println("numbers[:3] ==", numbers[:3])

   /* 默认上限为 len(s)*/
   fmt.Println("numbers[4:] ==", numbers[4:])

   numbers1 := make([]int,0,5)
   printSlice(numbers1)

   /* 打印子切片从索引  0(包含) 到索引 2(不包含) */
   number2 := numbers[:2]
   printSlice(number2)

   /* 打印子切片从索引 2(包含) 到索引 5(不包含) */
   number3 := numbers[2:5]
   printSlice(number3)

}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```

### append()函数和copy()函数

如果想增加切片容量，就必须创建一个更大的切片并把原分片的内容都拷贝过来

下面的代码描述了从拷贝切片的copy方法和追加新元素的append方法

```go
package main

import "fmt"

func main() {
   var numbers []int
   printSlice(numbers)

   /* 允许追加空切片 */
   numbers = append(numbers, 0)
   printSlice(numbers)

   /* 向切片添加一个元素 */
   numbers = append(numbers, 1)
   printSlice(numbers)

   /* 同时添加多个元素 */
   numbers = append(numbers, 2,3,4)
   printSlice(numbers)

   /* 创建切片 numbers1 是之前切片的两倍容量*/
   numbers1 := make([]int, len(numbers), (cap(numbers))*2)

   /* 拷贝 numbers 的内容到 numbers1 */
   copy(numbers1,numbers)
   printSlice(numbers1)  
}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```

*使用 `append(list,[params])`时，会先判断 list的 cap长度是否大于等于`len(list)+len([params])`如果大于则 cap不变，否则 cap等于`max{cap(list),cap[params]}`*

而这种判断的根源，其实是新切片与原切片共用一个底层数组，所以修改时底层数组的值就会改变，原切片或数组的值就被改变了而其实上面就是在判断底层数组能否容纳，不能就创建新的底层数组

*使用 `copy`函数要注意要初始化目标切片的 size，否则无法进行复制*

*由于slice的看似引用传递实则值传递特性，就算能通过其结构中带的指针修改切片数据内容，但使用append进行扩容时，实际改变的是被复制的那个切片长度，对于函数外的切片则没有变化，针对这种情况，只有传切片指针才能进行扩容*

### 函数间传递切片

因为切片占用内存小，成本低，所以切片特别适用于在数组间传递

## 范围（range）

用range关键字在for循环中迭代数组，切片，通道（channel），集合（map）的元素，在数组和切片中返回元素的索引和对应的值，在集合中返回key-value对

实例：

```go
package main
import "fmt"
func main() {
    //这是我们使用range去求一个slice的和。使用数组跟这个很类似
    nums := []int{2, 3, 4}
    sum := 0
    for _, num := range nums {
        sum += num
    }
    fmt.Println("sum:", sum)
    //在数组上使用range将传入index和值两个变量。上面那个例子我们不需要使用该元素的序号，所以我们使用空白符"_"省略了。有时侯我们确实需要知道它的索引。
    for i, num := range nums {
        if num == 3 {
            fmt.Println("index:", i)
        }
    }
    //range也可以用在map的键值对上。
    kvs := map[string]string{"a": "apple", "b": "banana"}
    for k, v := range kvs {
        fmt.Printf("%s -> %s\n", k, v)
    }
    //range也可以用来枚举Unicode字符串。第一个参数是字符的索引，第二个是字符（Unicode的值）本身。
    for i, c := range "go" {
        fmt.Println(i, c)
    }
}
```


