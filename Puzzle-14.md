## 多重人格

```go
package main

import (
    "fmt"
)

func main() {
    a, b := 1, 2
    b, c := 3, 4
    fmt.Println(a, b, c)
}
```

## 猜输出

此代码将打印：1 3 4


Go 的短变量声明 (:=) 可用于多值上下文，其中左侧有多个变量。

如果左边的所有变量都是新的，那么你很好：

```a, b := 1, 2```
如果你没有新的变量，你会得到一个编译错误：

```a, b := 1, 2```
a, b := 2, 3 // 错误：:= 左侧没有新变量
当有混合时，就像在这个预告片中一样，只要类型匹配，你仍然很好。 在这种情况下，当你做

```b, c := 3, 4```
b 是一个现有的变量，并且 3 的类型是 int 匹配 b 的当前类型。 好的在这里。 c 是一个新变量，使这个语句可以。

## 进一步阅读

- Go Tour 中的短变量声明
    http://tour.golang.org/basics/10
- Go 规范中的常量声明
    http://golang.org/ref/spec#Constant_declarations