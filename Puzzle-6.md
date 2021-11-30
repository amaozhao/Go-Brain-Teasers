## 我们到了吗？

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    timeout := 3
    fmt.Printf("before ")
    time.Sleep(timeout * time.Millisecond)
    fmt.Println("after")
}
```

## 猜输出

这段代码不会编译。


当你写 timeout := 3 时，Go 编译器会做一个类型推断。在这种情况下，它会推断超时是一个整数。然后在第 11 行，将 timeout（int 类型）与 time.Millisecond（time.Duration 类型）相乘，这是不允许的。

您有多种选择来解决这个问题。

将超时类型更改为 time.Duration（第 9 行）：

```var timeout time.Duration = 3```
将 timeout 设为 const，然后它的类型将在使用上下文中解析（第 9 行）：

```const timeout = 3```
使用类型转换将 timeout 转换为 time.Duration（第 11 行）：

```time.Sleep(time.Duration(timeout) * time.Millisecond)```

## 进一步阅读

- 维基百科上的类型推断
    http://en.wikipedia.org/wiki/Type_inference
- Go Tour 中的类型推断
    http://tour.golang.org/basics/14
- go/types：golang/example 中的 Go 类型检查器
    https://github.com/golang/example/tree/master/gotypes
- 时间。持续时间
    http://golang.org/pkg/time/#Duration
- Go 规范中的类型转换
    http://golang.org/ref/spec#Conversions