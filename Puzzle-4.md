## 我一无所有

```go
package main

import (
    "fmt"
)

func main() {
    n := nil
    fmt.Println(n)
}
```

## 猜输出

这段代码不会编译。


nil 不是类型而是保留字。 初始化为 nil 的变量必须有一个类型。 例如，如果你将赋值更改为 var n *int = nil，则代码将编译，因为现在 n 具有类型。

以下是一些使用 nil 的地方：

- map、slice 和 chan 的零值为零。
- 你不能使用 == 比较切片或映射； 你只能将它们与 nil 进行比较。
- 发送到 nil 频道或从 nil 频道接收将永远阻塞。 你可以使用它来避免繁忙的等待。

## 进一步阅读

- 戴夫·切尼 (Dave Cheney) 的 Practical Go 中的“Understanding nil”
    http://dave.cheney.net/practical-go/presentations/gophercon-israel.html#nil
- 为什么 Go 中没有通道？
    http://medium.com/justforfunc/why-are-there-nil-channels-in-go-9877cc0b2308
- Dave Cheney 的 Channel Axioms
    http://dave.cheney.net/2014/03/19/channel-axioms