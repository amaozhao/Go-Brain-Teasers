## 日志中有什么？

```go
```

## 猜输出

此代码将打印：2009-11-10 00:00:00 +0000 UTC


%v 动词将打印所有结构字段。

```go
package main

import (
    "fmt"
)

// Point is a 2D point
type Point struct {
    X int
    Y int
}

func main() {
    p := Point{1, 2}
    fmt.Printf("%v\n", p) // {1 2}
}
```

您会期望 Teaser 代码打印 {Hello 2009-11-10 00:00:00 +0000 UTC}。没有的原因是 Log 结构的定义方式。在第 11 行中有一个没有名称的字段，只有一个类型。这称为嵌入，它意味着 Log 类型具有 time.Time 具有的所有方法和字段。

time.Time 定义了一个 String() 字符串方法，这意味着它实现了 [ fmt.Stringer 接口。由于 Log 嵌入了 time.Time，它还有一个 String() 字符串方法。如果传递给 fmt fmt.Printf 的参数实现了 fmt.Stringer，fmt.Printf 将使用它而不是默认输出。

如果将第 11 行中的定义更改为 Time time.Time，您将看到 {Hello 2009-11-10 00:00:00 +0000 UTC} 的预期输出。

您还可以在 Go 中嵌入接口。参见io.ReadWriter的定义，例如：

```go
type ReadWriter interface {
    Reader
    Writer
}
```

## 进一步阅读

- Gopher Academy 博客上的 Flags 乐趣
    http://blog.gopheracademy.com/advent-2019/flags/
- 嵌入 Effective Go
    http://golang.org/doc/effective_go.html#embedding
- Ardan Labs 博客上的 Go 中的方法、接口和嵌入类型
    http://ardanlabs.com/blog/2014/05/methods-interfaces-and-embedded-types.html