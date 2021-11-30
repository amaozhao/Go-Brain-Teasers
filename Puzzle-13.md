## 自由范围整数

```go
package main

import (
    "fmt"
)

func fibs(n int) chan int {
    ch := make(chan int)
    
    go func() {
        a, b := 1, 1
        for i := 0; i < n; i++ {
            ch <- a
            a, b = b, a+b
        }
    }()
    return ch
}

func main() {
    for i := range fibs(5) {
        fmt.Printf("%d ", i)
    }
    fmt.Println()
}
```

## 猜输出

这段代码会死锁。


要从通道中获取值，您可以使用接收运算符 (←ch) 或在通道上使用范围执行 for 循环，从而使用其中的所有内容。

范围如何知道通道中何时没有更多值？它等待通道关闭。前面脚本的问题是goroutine没有关闭通道。

```go
package main

import (
    "fmt"
)

func fibs(n int) chan int {
    ch := make(chan int)
    
    go func() {
        defer close(ch) // <1>
        a, b := 1, 1
        for i := 0; i < n; i++ {
            ch <- a
            a, b = b, a+b
        }
    }()
    return ch
}

func main() {
    for i := range fibs(5) {
        fmt.Printf("%d ", i)
    }
    fmt.Println()
}
```

迭代通道时的范围是这样的：

```go
package main

import (
    "fmt"
)

func fibs(n int) chan int {
    ch := make(chan int)
    go func() {
        defer close(ch)
        a, b := 1, 1
        for i := 0; i < n; i++ {
            ch <- a
            a, b = b, a+b
        }
    }()
    return ch
}

func main() {
    ch := fibs(5)
    for {
        i, ok := <-ch
        if !ok {
            break
        }
        fmt.Printf("%d ", i)
    }
    fmt.Println()
}
```

使用此方法创建迭代器时，需要注意 goroutine 泄漏。如果你创建 ch := fibs(3) 然后只使用其中的一两个值，fibs 中的 goroutine 将在发送到通道时被阻塞。由于存在对通道的引用，垃圾收集器不会回收它。正如 Dave Cheney 所说，“永远不要在不知道将如何完成的情况下启动 goroutine。”

通常的做法是传递一个 done 通道或一个 context.Context。这会使代码复杂一点，但您可以控制停止 goroutines 并避免 goroutine 泄漏。

```go
package main

import (
    "context"
    "fmt"
)

func fibs(ctx context.Context, n int) chan int {
    ch := make(chan int)
    go func() {
        defer close(ch)
        a, b := 1, 1
        for i := 0; i < n; i++ {
            select {
                case ch <- a:
                a, b = b, a+b
                case <-ctx.Done():
                fmt.Println("cancelled")
            }
        }
    }()
    return ch
}

func main() {
    ctx, cancel := context.WithCancel(context.Background())
    ch := fibs(ctx, 5)
    for i := 0; i < 3; i++ {
        val := <-ch
        fmt.Printf("%d ", val)
    }
    fmt.Println()
    cancel()
}
```

## 进一步阅读

- Go 并发模式：上下文
    http://blog.golang.org/context
- Go 并发模式：管道和取消
    http://blog.golang.org/pipelines
- 包上下文
    http://golang.org/pkg/context/
- Dave Cheney 在 Practical Go 中的“并发”章节
    http://dave.cheney.net/practical-go/presentations/gophercon-israel.html#concurrency