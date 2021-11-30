## 生食

```go
package main

import (
    "fmt"
)

func main() {
    s := `a\tb`
    fmt.Println(s)
}
```

猜输出

此代码将打印：a\tb

> Go规范[1]说
>
> 有两种形式：原始字符串文字和解释的字符串文字。

原始字符串用反引号括起来，解释过的字符串用引号括起来。在原始字符串中，\ 没有特殊含义，因此 '\t' 是两个字符，反斜杠和字母 t。如果它是一个解释字符串，\t 将被解释为制表符。

除了通常的 \t（制表符）、\n（换行符）和朋友之外，您还可以在解释字符串中使用 Unicode 代码点。 fmt.Println("\u2122") 将打印™。

原始字符串最常见的用途之一是创建多行字符串。

```
// An HTTP request
request := `GET / HTTP/1.1
Host: www.353solutions.com
Connection: Close

`
```

## 进一步阅读

- 去规范文字
    http://golang.org/ref/spec#String_literals
- Go 中的字符串、字节、符文和字符
    http://blog.golang.org/strings
- 字符串规范
    http://golang.org/ref/spec#String_literals
- 维基百科上的 ASCII 控制字符
    http://en.wikipedia.org/wiki/Control_character#In_ASCII