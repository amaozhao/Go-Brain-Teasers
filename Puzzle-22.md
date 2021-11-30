## 数我一百万

```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    var count int
    var wg sync.WaitGroup

    for i := 0; i < 1_000_000; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            count++
        }()
    }
    wg.Wait()
    fmt.Println(count)
}
```

## 猜输出

此代码将打印：891239


不同的输出

您的输出可能会有所不同，但会低于 1,000,000。
你在这里拥有的是一个竞争条件。增加整数的操作不是原子的，这意味着代码可能会在操作中被中断。最重要的是，还有内存屏障。为了加快速度，现代计算机尝试从 CPU 缓存中获取值，然后再进入主内存。

您可以通过运行 go tool compile -S count.go 来查看 count.go 的汇编输出。执行此操作后，您将在第 16 行看到为 count++ 生成的代码：

```go
0x0045 00069 (count.go:16)  PCDATA  $0, $1
0x0045 00069 (count.go:16)  PCDATA  $1, $4
0x0045 00069 (count.go:16)  MOVQ    "".&count+56(SP), AX
0x004a 00074 (count.go:16)  PCDATA  $0, $0
0x004a 00074 (count.go:16)  INCQ    (AX)
```

MOVQ 将获取计数器的当前值到 AX 寄存器。这可能来自内存或缓存。

Go 工具链可以帮助您检测此类竞争条件。 go run 和 go test 都有一个 -race 标志来检测竞争条件。你可以试试：

```go
$ go run -race count.go
==================
WARNING: DATA RACE
Read at 0x00c00011a010 by goroutine 8:
        main.main.func1()
                        ./code/count.go:16 +0x6c

Previous write at 0x00c00011a010 by goroutine 7:
        main.main.func1()
                        ./code/count.go:16 +0x82

Goroutine 8 (running) created at:
        main.main()
                        ./code/count.go:14 +0xe4

Goroutine 7 (finished) created at:
        main.main()
                        ./code/count.go:14 +0xe4
```

为了解决这个问题，你可以使用一个 sync.Mutex 来确保一次只有一个 goroutine 更新计数。

```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    var count int
    var wg sync.WaitGroup
    var m sync.Mutex
    for i := 0; i < 1_000_000; i++ {
        wg.Add(1)
        go func() {
            m.Lock()
            defer m.Unlock()
            defer wg.Done()
            count++
        }()
    }
    wg.Wait()
    fmt.Println(count)
}
```


另一种选择是使用 sync/atomic 包。 sync/atomic，提供比使用互斥锁更快但更难使用的原子操作。

```go
package main

import (
    "fmt"
    "sync"
    "sync/atomic"
)

func main() {
    var count int64 // Atomic works with int64, not int
    var wg sync.WaitGroup
    for i := 0; i < 1_000_000; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            atomic.AddInt64(&count, 1)
        }()
    }
    wg.Wait()
    fmt.Println(count)
}
```

## 进一步阅读

- 维基百科上的竞争条件
    http://en.wikipedia.org/wiki/Race_condition
- 原子与非原子操作
    http://preshing.com/20130618/atomic-vs-non-atomic-operations/
- 介绍围棋比赛检测器
    http://blog.golang.org/race-detector
- 记忆障碍/栅栏
    http://mechanical-sympathy.blogspot.com/2011/07/memory-barriersfences.html
- 维基百科上的内存屏障
    http://en.wikipedia.org/wiki/Memory_barrier
- Go 中的调度：第一部分 – 操作系统调度器
    http://ardanlabs.com/blog/2018/08/scheduling-in-go-part1.html
- 人类规模的计算机延迟
    http://twitter.com/srigi/status/917998817051541504