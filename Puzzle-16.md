## Channel中有什么？

```go
package main

import (
    "fmt"
)

func main() {
    ch := make(chan int, 2)
    ch <- 1
    ch <- 2
    <-ch
    close(ch)
    a := <-ch
    b := <-ch
    fmt.Println(a, b)
}
```

## 猜输出

此代码将打印：2 0


ch 是一个容量为 2 的缓冲通道（您可以使用 cap(ch) 进行检查）。您向 ch 发送两个值，1 和 2。如果您尝试发送另一个值，则会陷入僵局。

然后您从 ch 接收一个值，关闭它，并从它接收另外两个值。问题是，当你从一个封闭的通道接收时会发生什么？

答案是如果缓冲区中有值，你就会得到它们；否则，您将获得通道类型的零值。这就是为什么 a 获得缓冲区中的值 2，而 b 获得 0，这是 int 的零值。

您如何知道从通道获得的值是实际存在的值还是自通道关闭以来的零值？使用通常的逗号，好的范例。

```go
package main

import (
    "fmt"
)

func main() {
    ch := make(chan int, 1)
    ch <- 1
    a, ok := <-ch
    fmt.Println(a, ok) // 1 true
    close(ch)
    b, ok := <-ch
    fmt.Println(b, ok) // 0 false
}
```

## 进一步阅读

- Go Tour 中的缓冲通道
    http://tour.golang.org/concurrency/3
- 有效围棋中的通道
    http://golang.org/doc/effective_go.html#channels
- 为什么 Go 中没有通道？
    http://medium.com/justforfunc/why-are-there-nil-channels-in-go-9877cc0b2308