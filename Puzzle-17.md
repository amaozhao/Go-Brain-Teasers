## 一个 **Int** eresting 字符串

```go
package main

import (
    "fmt"
)

func main() {
    i := 169
    s := string(i)
    fmt.Println(s)
}
```

## 猜输出

此代码将打印：©


string 类型支持从 int 类型转换。 它会将这个整数视为符文。 符文 169 是版权标志 (©)。 犯这个错误的人通常来自 Python 等语言，其中 str(169) → "169"。

如果要将数字转换为字符串（或将字符串转换为数字），请使用 strconv 包。

```go
package main

import (
    "fmt"
    "strconv"
)

func main() {
    i := 169
    s := strconv.Itoa(i)
    fmt.Println(s)
}
```

字符串还支持从字节切片进行类型转换。

```go
package main

import (
    "fmt"
)

func main() {
    data := []byte{'1', '2', '9'}
    s := ​string(data)
    fmt.Println(s) // 129
}
```

当您从字节切片转换为字符串时，Go 将复制字节切片，从而进行内存分配。 在映射中，您不能使用 [] 字节作为键，因此存在编译器优化。

```go
package main

import (
    "fmt"
)
func main() {
    m := map[string]int{
        "hello": 3,
    }
    key := []byte{'h', 'e', 'l', 'l', 'o'}
    val := m[string(key)] // no memory allocation
    fmt.Println(val)      // 3
}
```

进一步阅读

strconv 文档
http://golang.org/pkg/strconv/

golang.org/x 中的符文名
http://godoc.org/golang.org/x/text/unicode/runenames

来自 Dave Cheney 的“High-Performance Go Workshop”的“Using []byte as a Map Key”
https://dave.cheney.net/high-performance-go-workshop/gophercon-2019.html