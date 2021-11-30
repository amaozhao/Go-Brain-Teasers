## 双城记

```go
package main

import (
    "fmt"
)

func main() {
    city1, city2 := "Kraków", "Kraków"
    fmt.Println(city1 == city2)
}
```

## 猜输出

不要尝试将 PDF 中的代码复制并粘贴到你的编辑器中，它可能会也可能不会起作用。使用源存储库中的文件来运行代码。

此代码将打印：false


你的视力没问题。这两个字符串看起来一样。但是，如果打印每个变量的长度，city1 的长度将为 7，city2 的长度将为 8。

在拼图 3 中， 在克拉科夫的时候，我们谈到了 Go 的字符串是 UTF-8 编码的字节切片。 UTF-8 有很多特性，有些字符（符文）不是用来显示而是用来控制的，比如文本方向，有时也叫做bidi（bidirectional 的缩写）。

city1 在位置 4 (ó) 处有一个 1 字节的符文，而 city2 在位置 4 处有一个 o 符文和一个控制字符，上面写着“给前一个字符添加一个元音变音”。这些编码方式称为 NFC 和 NFD。

当你比较字符串时，它们在字节级别进行比较。这就是为什么你会看到 false 作为输出。要解决此问题，你需要使用名为 golang.org/x/text/unicode/norm 的外部库。

```go
package main

import (
    "fmt"
    "golang.org/x/text/unicode/norm"
)

func main() {
    city1, city2 := "Kraków", "Kraków"
    city1, city2 = norm.NFC.String(city1), norm.NFC.String(city2)
    fmt.Println(city1 == city2)
}
```

确保将你正在处理的所有文本规范化为单一形式（例如，NFC）。

golang.org/x 包不在标准库中，而是由 Go 核心开发人员（和朋友）维护。 golang.org/x 下有很多有用的库。我建议花一些时间去了解他们。

例如， golang.org/x/text/transform 有一个 NewReader 函数，它返回一个 io.Reader ，它将把它读取的所有内容转换为正常形式。

## 进一步阅读

- 维基百科上的比迪
    http://en.wikipedia.org/wiki/Bidirectional_text
- 维基百科上的 Unicode 等效
    http://en.wikipedia.org/wiki/Unicode_equivalence
- Go 中的字符串、字节、符文和字符
    http://blog.golang.org/strings
- 维基百科上的 UTF-8
    http://en.wikipedia.org/wiki/UTF-8