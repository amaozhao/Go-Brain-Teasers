## 两手空空

```go
package main

import (
    "fmt"
)

func main() {
    var m map[string]int
    fmt.Println(m["errors"])
}
```

## 猜输出

此代码将打印：0


未初始化映射的零值为 nil。 Go 的 map 类型上的一些操作是 nil 安全的，这意味着它们可以在 nil map 上工作而不会出现恐慌。

len 和访问值（例如，m["errors"]）都适用于 nil 映射。 len 返回 0，通过键访问值将执行以下操作：

- 如果键在地图中，则返回与其关联的值。
- 如果键不在映射中，则返回值类型的零值。

在此示例中，在映射中找不到键“errors”，您将返回 int 的零值，即 0。

这种行为很方便。如果您想计算项目（例如，文档中的词频），您可以执行 m[word]++ 而不用担心单词是否在地图 m 中。

这就引出了一个问题，你怎么知道你从地图中得到的值是因为它在那里还是因为它是值类型的零值？答案是使用逗号，ok 范式。

当您输入 val, ok := m[key] 时，如果我们从地图中获取值，则 ok 为真，如果键不在地图中，则为假。

Go 中还有其他零安全类型。例如，您可以询问 nil 切片的长度，从空通道接收值（尽管您将永远被阻塞）等等。

## 进一步阅读

- Go Maps 在行动 http://blog.golang.org/maps
- Effective Go 中的地图 http://golang.org/doc/effective_go.html#maps
- Go 规范中的零值 http://golang.org/ref/spec#The_zero_value