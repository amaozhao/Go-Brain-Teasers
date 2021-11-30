## 及时行乐

```go
package main

import (
    "encoding/json"
    "fmt"
    "log"
    "time"
)

func main() {
    t1 := time.Now()
    data, err := json.Marshal(t1)
    if err != nil {
        log.Fatal(err)
    }

    var t2 time.Time
    if err := json.Unmarshal(data, &t2); err != nil {
        log.Fatal(err)
    }
    fmt.Println(t1 == t2)
}
```

## 猜输出

此代码将打印：false


您可能希望此代码失败，因为 JSON 格式中没有时间类型，或者您可能希望比较成功。

Go 的 encoding/json 允许您为 JSON 不支持的类型定义自定义 JSON 序列化。您可以通过实现 json.Marshaler 和 json.Unmarshaler 接口来做到这一点。 Go 的 time.Time 通过将自身编组到 RFC 3339 格式的字符串并返回来实现这些接口。

JSON 的类型非常有限。例如，它只有浮点数，如果在解组时使用空接口，可能会导致令人惊讶的结果。

```go
package main

import (
    "encoding/json"
    "fmt"
    "log"
)

func main() {
    n1 := 1
    data, err := json.Marshal(n1)
    if err != nil {
        log.Fatal(err)
    }

    var n2 interface{}
    if err := json.Unmarshal(data, &n2); err != nil {
        log.Fatal(err)
    }
    fmt.Printf("n1 is %T, n2 is %T  \n ", n1, n2)
}
```

前面会打印 n1 是 int，n2 是 float64。

一旦你通过了 JSON 序列化，你会期望时间是相等的。时间包文档说明如下（我的重点）：

操作系统提供了一个“挂钟”，它受时钟同步变化的影响，以及一个“单调时钟”，它不是。一般规则是挂钟是用来报时的，而单调钟是用来测量时间的。而不是拆分 API，在这个包中 time.Now 返回的 Time 包含挂钟读数和单调时钟读数......

单调时钟用于测量持续时间。它们的存在是为了避免诸如您的计算机在测量期间切换到夏令时等问题。单调时钟本身的价值并不意味着什么；只有两个单调时钟读数之间的差异才有用。

当你使用 == 比较 time.Time 时，Go 会比较 time.Time 结构体字段，包括单调读数。但是，当 Go 将 time.Time 序列化为 JSON 时，它在输出中不包含单调时钟。当你回读时间到t2时，它不包含单调读数，比较失败。

问题的解决方法写在time.time的文档中：

一般来说，比 t == u 更喜欢 t.Equal(u)，因为 t.Equal 使用最准确的比较可用并正确处理只有一个参数具有单调时钟读数的情况。

## 进一步阅读

- JSON 格式
    http://json.org
- 程序员相信时间的谎言
    http://infiniteundo.com/post/25326999628/falsehoods-programmers-believe-about-time
- 时间包文档
    http://golang.org/pkg/time/#Time
- 以下参考资料和索引中的 RFC 3339
    http://ietf.org/rfc/rfc3339.txt
- 维基百科上的 ISO 8601
    http://en.wikipedia.org/wiki/ISO_8601