## 双重打击

```go
package main

import (
    "fmt"
)

func init() {
    fmt.Printf("A ")
}

func init() {
    fmt.Print("B ")
}

func main() {
    fmt.Println()
}
```

## 猜输出

此代码将打印： A B


通常，Go 编译器不会让你在同一个包中定义两个同名的函数。但是，init 是特殊的。以下是文档中的内容：

即使在单个源文件中，每个包也可以定义多个这样的函数。在 package 块中，init 标识符只能用于声明 init 函数，而标识符本身并没有声明。因此，不能从程序的任何地方引用 init 函数。

init 是在导入模块初始化和包级变量初始化之后包初始化的最后阶段。

由于你可以使用函数调用来初始化包级变量，因此请为特殊情况保存 init。此外，尽量避免包级变量。我很少编写 init 函数，而更喜欢在 main 中进行初始化，在那里我可以更好地控制初始化、错误处理和日志记录的顺序。

包级变量或 init 的问题之一是你无法返回错误值。如果你在 init 中有错误，最好的做法是恐慌，不要在错误状态下继续执行。为了支持这一点，一些 Go 包提供了它们的新函数的 Must 版本。例如，regexp 包有 MustCompile：

MustCompile 与 Compile 类似，但如果无法解析表达式，则会出现恐慌。它简化了保存已编译正则表达式的全局变量的安全初始化。

尝试考虑你的类型以及是否应该为它们提供 Must 函数。

## 进一步阅读

- Effective Go 中的 init 函数
    http://golang.org/doc/effective_go.html#init
- Go 规范中的包初始化
    http://golang.org/ref/spec#Package_initialization
- Dave Cheney 的“在实践中删除包作用域变量”
    http://dave.cheney.net/2017/06/11/go-without-package-scoped-variables
- Ran Tavory 编写可部署代码（第 2 部分）
    http://medium.com/@rantav/writing-deployable-code-part-two-217bc884c917