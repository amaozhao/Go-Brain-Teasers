## 谁是下一个？

```go
package main

import (
    "fmt"
)

var (
    id = nextID()
)

func nextID() int {
    id++
    return id
}

func main() {
    fmt.Println(id)
}
```

## 猜输出

这段代码不会编译。


构建代码显示问题：

```go
$ go build next_id.go
# command-line-arguments
./next_id.go:8:2: initialization loop:
        /home/miki/Projects/go-brain-teasers/code/next_id.go:8:2: id refers to
        /home/miki/Projects/go-brain-teasers/code/
        next_id.go:11:6: nextID refers to
        /home/miki/Projects/go-brain-teasers/code/next_id.go:8:2: id
```

代码中有一个初始化循环。 id 的值取决于 nextID，nextID 使用 id 的值，其中……

Go 编译器尝试找到这些循环，但在某些情况下它无法检测到它们（称为隐藏依赖项）。

确保你的包初始化简单易懂。 尽量避免包级变量，并尽可能将初始化推迟到 main。

## 进一步阅读

- Go 规范中的包初始化
    http://golang.org/ref/spec#Package_initialization
- Dave Cheney 的“在实践中删除包作用域变量”
    http://dave.cheney.net/2017/06/11/go-without-package-scoped-variables