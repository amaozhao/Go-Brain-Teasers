## 有趣的旗帜

```go
package main

import (
    "fmt"
)

type Flag int

func main() {
    var i interface{} = 3
    f := i.(Flag)
    fmt.Println(f)
}
```

## 猜输出

这段代码会恐慌。


尽管 Flag 基本上是一个 int，但 Go 编译器将其视为一种不同的类型，并且类型断言 (i.(Flag)) 会恐慌。这是你可以使用逗号，ok 范例的另一种情况。

```go
package main

import (
    "fmt"
)

type Flag int

func main() {
    var i interface{} = 3
    f, ok := i.(Flag)
    if !ok {
        fmt.Println("not a Flag")
        return
    }
    fmt.Println(f)
}
```

你还可以将第 7 行更改为键入 Flag = int。现在 Flag 是一个类型别名，代码将起作用。但是，现在你可以在需要 Flag 时使用 int 并且类型系统不会保护你。

## 进一步阅读

- 类型别名
    http://golang.org/doc/go1.9#language
- “代码库重构（在 Go 的帮助下）”（解释类型别名的基本原理）
    http://talks.golang.org/2016/refactor.article
- Go 规范中的类型声明
    http://golang.org/ref/spec#Type_declarations
- Go 规范中的类型断言
    http://golang.org/ref/spec#Type_assertions
- 在 Effective Go 中键入 Switch
    http://golang.org/doc/effective_go.html#type_switch
- Effective Go 中的接口转换和类型断言
    http://golang.org/doc/effective_go.html#interface_conversions