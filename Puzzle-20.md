## 字符串中有什么？

```go
package main

import (
    "fmt"
)

func main() {
    msg := "π = 3.14159265358..."
    fmt.Printf("%T ", msg[0])
    for _, c := range msg {
        fmt.Printf("%T\n", c)
        break
    }
}
```

## 猜输出

此代码将打印： uint8 int32


Go 字符串是 UTF-8 编码的。当您使用 [] 或 len 访问字符串时，Go 将访问底层的 []byte。 Slice 是 Go 中的一种类型，别名为 uint8。

当你在 Go 中迭代一个字符串时，你会得到一个名为 rune 的 Unicode 字符，它是 int32 的别名。这是因为 UTF-8 中的字符最多可以有 4 个字节。

如果您使用 %v 动词打印符文，您将看到数值。使用 %c 动词来显示字符。如果你看到 ？或其他字符而不是您尝试打印的符文，这意味着当前字体不支持它。没有一种字体可以显示 Unicode 规范中的所有字体。

有几种方法可以编写符文文字。让我们看看下面的部分。

```go
package main

import (
    "fmt"
)

func main() {
    r1 := '™'
    fmt.Println(r1)        // 8482
    fmt.Printf("%c\n", r1) // ™
    
    r2 := '\x61'           // \x - 2 digits
    fmt.Printf("%c\n", r2) // a
    
    r3 := '\u2122'         // \u - 4 digits (8482 in hex)
    fmt.Printf("%c\n", r3) // ™
    
    r4 := '\U00002122'     // \U - 8 digits
    fmt.Printf("%c\n", r4) // ™
}
```

## 进一步阅读

- Go 中的字符串、字节、符文和字符
    http://blog.golang.org/strings
- Unicode 和你
    http://betterexplained.com/articles/unicode/
- 维基百科上的 Unicode
    http://en.wikipedia.org/wiki/Unicode
- 维基百科上的 UTF-8
    http://en.wikipedia.org/wiki/UTF-8
- Go 规范中的符文文字
    http://golang.org/ref/spec#Rune_literals
- Unicode 字符表
    http://unicode-table.com/