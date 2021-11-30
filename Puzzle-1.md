## 数字 π

```go
package main

import(
    "fmt"
)

func main() {
    var π = 22 / 7.0
    mt.Println(π)
}
```

## 猜输出

此代码将打印：3.1415929203539825


这里有两个令人惊讶的事情：一是 π 是一个有效标识符，二是 355 / 113.0 实际编译。

让我们从 π 开始。关于标识符的 Go 语言规范说

标识符命名程序实体，例如变量和类型。标识符是一个或多个字母和数字的序列。标识符中的第一个字符必须是字母。

字母可以是 Unicode 字母，包括 π。这写起来很有趣，但实际上它会让你的同事的生活更艰难。我可以使用我正在使用的 Vim 编辑器轻松输入 π；然而，大多数编辑器和 IDE 将需要更多的努力。

我发现 Unicode 标识符唯一有用的地方是将数学公式转换为代码时。除此之外，坚持使用普通的旧 ASCII。

现在到 355 / 113.0。 Go 类型系统不允许在整数 (355) 和浮点数 (113.0) 之间进行除法（或任何其他数学运算）。但是你在 = 的右边是常量，而不是变量。常量的类型是在使用时定义的；在本例中，编译器会将 355 转换为浮点数以完成操作。

如果先给变量赋值355和113.0，再尝试同样的代码，就会失败。以下不会编译。

```go
package main

import (
    "fmt"
)

func main() {
    a, b := 22, 7.0
    var π = a / b
    fmt.Println(π)
}
```

## 进一步阅读

- 常数说明 http://golang.org/ref/spec#Constants
- 常数 http://blog.golang.org/constants
- Ardan Labs 的“Go 中的数值常量介绍” http://ardanlabs.com/blog/2014/04/introduction-to-numeric-constants-in-go.html
- 维基百科上 π 的近似值 http://en.wikipedia.org/wiki/Approximations_of_%CF%80
- 标识符的 Go 语言规范 http://golang.org/ref/spec#Identifiers