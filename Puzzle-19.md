## 犯错或不犯错

```go
package main

import (
    "fmt"
)

type OSError int

func (e *OSError) Error() string {
    return fmt.Sprintf("error #%d", *e)
}

func FileExists(path string) (bool, error) {
    var err *OSError
    return false, err // TODO
}

func main() {
    if _, err := FileExists("/no/such/file"); err != nil {
        fmt.Printf("error: %s\n", err)
    } else {
        fmt.Println("OK")
    }
}
```

## ## 猜输出

此代码将打印：错误：\<nil\>


第 19 行中的 if 语句表示 err != nil，但 err 打印为 \<nil\>。 此外，第 14 行中的 err 未初始化，这意味着它具有指针的零值，即 nil。

如果您查看 Go 源代码库中的 src/runtime/runtime2.go，您将看到以下定义：

```go
type iface struct {
    tab  *itab
    data unsafe.Pointer
}
```

- itab 描述了接口。
- data 是指向实现接口的值的指针。

只有当 itab 和 data 都为 nil 时，接口才被认为是 nil，这里不是这种情况。

解决方案是在返回错误时始终使用错误类型的变量。 在这种情况下，将第 14 行更改为 var err 错误。

## 进一步阅读

- go-internals Book 中的接口
    http://github.com/teh-cmc/go-internals/blob/master/chapter2_interfaces/README.md
- Go 数据结构：接口
    http://research.swtch.com/interfaces