## 在克拉科夫时

```go
package main

import (
    "fmt"
)

func main() {
    city := "Kraków"
    fmt.Println(len(city))
}
```

## 猜输出

此代码将打印：7


如果你数一数克拉科夫的字符数，就会出现六个。那为什么是7？原因是……历史。

起初，计算机是在英语国家开发的：英国和美国。当早期的开发人员想在只能理解位的计算机中编码文本时，他们提出了以下方案。使用一个字节（8 位）来表示一个字符。例如，a 是 97 (01100001)，b 是 98，依此类推。一个字节足以容纳英文字母表，包含二十六个小写字母、二十六个大写字母和十个数字。甚至还有一些空间留给其他特殊字符（例如，9 表示制表符）。这称为 ASCII 编码。

过了一段时间，其他国家开始使用计算机，他们想用自己的母语写作。 ASCII 不够好；单个字节不足以容纳表示不同语言字母所需的所有数字。这导致了几种不同的编码方案；最常见的一种是UTF-8。 Rob Pike，Go 的设计者之一，也是 UTF-8 的设计者之一。

Go 字符串是 UTF-8 编码的。这意味着一个字符（称为符文）可以是 1 到 4 个字节长。当您在 Go 中询问字符串的长度时，您将获得以字节为单位的大小。在这个例子中，符文 ó 占用了 2 个字节；因此，字符串的总长度为 7。

如果您想知道字符串中的符文数量，则需要使用 unicode/utf8 包。

```go
package main

import (
    "fmt"
    "unicode/utf8"
)

func main() {
    city := "Kraków"
    fmt.Println(utf8.RuneCountInString(city))
}
```

## 进一步阅读

- Go 中的字符串、字节、符文和字符 http://blog.golang.org/strings
- Unicode 和你 http://betterexplained.com/articles/unicode/
- 维基百科上的 Unicode http://en.wikipedia.org/wiki/Unicode
- 维基百科上的 ASCII http://en.wikipedia.org/wiki/ASCII
- 维基百科上的 UTF-8 http://en.wikipedia.org/wiki/UTF-8