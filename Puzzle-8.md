## 睡眠排序

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func main() {
    var wg sync.WaitGroup
    for _, n := range []int{3, 1, 2} {
        wg.Add(1)
        go func() {
            defer wg.Done()
            time.Sleep(time.Duration(n) * time.Millisecond)
            fmt.Printf("%d ", n)
        }()
    }
    wg.Wait()
    fmt.Println()
}
```

## 猜输出

此代码将打印：2 2 2


你可能期望 1 2 3。每个 goroutine 休眠 n 毫秒，然后打印 n。 wg.Wait() 会等到所有的 goroutine 完成，然后程序会打印一个换行符。

但是，每个 goroutine 使用的 n 与第 11 行中定义的 n 相同。这称为闭包。

这就是为什么按照设计，goroutines（和延迟函数）被编写为带参数的函数调用。

你有两种选择来解决此错误。第一个，也是我的偏好，是将 n 作为参数传递给 goroutine。

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func main() {
    var wg sync.WaitGroup
    for _, n := range []int{3, 1, 2} {
        wg.Add(1)
        go func(i int) {
            defer wg.Done()
            time.Sleep(time.Duration(n) * time.Millisecond)
            fmt.Printf("%d ", i)
        }(n)
    }
    wg.Wait()
    fmt.Println()
}
```

第二种解决方案是利用这样一个事实，每次在 Go 中编写 { 时，都会打开一个新的变量作用域。

```go
package main

import (
    "fmt"
    "sync"
    "time"
)
func main() {
    var wg sync.WaitGroup
    for _, n := range []int{3, 1, 2} {
        n := n // <1>
        wg.Add(1)
        go func() {
            defer wg.Done()
            time.Sleep(time.Duration(n) * time.Millisecond)
            fmt.Printf("%d ", n)
        }()
    }
    wg.Wait()
    fmt.Println()
}
```

n 现在是一个新变量，它只存在于 for 循环范围内，更重要的是，存在于 goroutine 闭包中。它“遮蔽”了第 11 行中的外部 n。

## 进一步阅读

- Go Tour 中的函数闭包
    http://tour.golang.org/moretypes/25
- 维基百科关闭
    http://en.wikipedia.org/wiki/Closure_(computer_programming