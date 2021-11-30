## 一个时髦的数字？

```go
package main

import (
    "fmt"
)

func main() {
    fmt.Println(0x1p-2)
}
```

## 猜输出

在移至下一页之前尝试猜测输出是什么。

此代码将打印：0.25


Go 有几种数字类型。主要的两个是

整数
这些是整数。 Go 有 int8、int16、int32、int64 和 int。还有所有未签名的，例如uint8等。

漂浮
这些是实数。 Go 有 float32 和 float64。

还有其他类型，例如 complex 和 math/big 中定义的各种类型。

当您编写数字字面量时，例如 3.14，Go 编译器需要将其解析为特定类型（在本例中为 float64）。 Go 规范[2] 定义了如何写数字。让我们看一些例子。

```go
package main

import (
    "fmt"
)

func main() {
    // Integer
    printNum(10)    // 10 of type int
    printNum(010)   // 8 of type int
    printNum(0x10)  // 16 of type int
    printNum(0b10)  // 2 of type int
    printNum(1_000) // 1000 of type int <1>
    
    // Float
    printNum(3.14)  // 3.14 of type float64
    printNum(.2)    // 0.2 of type float64
    printNum(1e3)   // 1000 of type float64
    printNum(0x1p-2) // 0.25 of type float64
    
    // Complex
    printNum(1i)     // (0+1i) of type complex128
    printNum(3 + 7i) // (3+7i) of type complex128
    printNum(1 + 0i) // (1+0i) of type complex128
}

func printNum(n interface{}) {
    fmt.Printf("%v of type %T\n", n, n)
}
```

_ 作为千位分隔符。它使大数字对我们人类更具可读性。

1e3 被称为科学记数法。

0x1p-2 在 Go 规范中被称为十六进制浮点文字，并遵循 IEEE 754 2008 规范。要计算该值，请执行以下操作：

将 p 之前的值计算为十六进制数。在这个例子中，它是 0x1 = 1。

将 p 之后的值计算为 2 的该值的幂。在这个例子中，它是 2-2 = 0.25。

最后，将两个数字相乘，在本例中为 1 * 0.25 = 0.25。

## 进一步阅读

- Go 规范中的词法元素
    http://golang.org/ref/spec#Lexical_elements
- 维基百科上的科学记数法
    http://en.wikipedia.org/wiki/Scientific_notation
- 维基百科上的 IEEE 754
    http://en.wikipedia.org/wiki/IEEE_754

整数文字
http://golang.org/ref/spec#Integer_literals

浮点文字
http://golang.org/ref/spec#Floating-point_literals

想象中的文字
http://golang.org/ref/spec#Imaginary_literals