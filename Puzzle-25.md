## 你有我的许可

```go
package main

import (
    "fmt"
)

const (
    Read = 1 << iota
    Write
    Execute
)

func main() {
    fmt.Println(Execute)
}
```

## 猜输出

此代码将打印：4


iota 是枚举类型的 Go 版本。它可以在 const 声明中使用。对于同一组中的每个常数，iota 增长 1。如果你在 iota 上指定一个操作（例如，加法），它将继续。 << 是左移运算符。它将向左移动 n 个位置中的位：

- 1 in binary (8 bit) is 00000001.
- 1<<3 is 00001000, which is 8.

在这种情况下，我们从 Read = 1<<iota 开始，它转换为 1<<0 → 0001，即 1。然后有一个隐式的 1<<1 → 0010 for Write，即 2，最后，一个隐式的1<<2 → 0100 表示 Execute，即 4。

你将主要将这些位操作用于标志。你可以使用 | 在一个字节中打包八个标志（按位或）（称为位掩码）并有效地检查是否使用 &（按位与）设置了标志。

```go
package main

import (
    "fmt"
)

const (
    Read = 1 << iota
    Write
    Execute
)

func main() {
    mask := Read | Execute
    
    if mask&Execute == 0 {
        fmt.Println("can't execute")
    } else {
        fmt.Println("can execute") // will be printed
    }
    
    if mask&Write == 0 {
        fmt.Println("can't write") // will be printed
    } else {
        fmt.Println("can write")
    }
}
```


iota 值是数字。在某些情况下，你希望常量具有人类可读的表示形式。这是通过给这些常量一个类型并为这个类型实现 fmt.Stringer 接口来完成的。

```go
package main

import (
    "fmt"
)

type FilePerm uint16 // 16 flags are enough

const (
    Read FilePerm = 1 << iota
    Write
    Execute
)

// String implements fmt.Stringer interface
func (p FilePerm) String() string {
    switch p {
        case Read:
        return "read"
        case Write:
        return "write"
        case Execute:
        return "execute"
    }

    return fmt.Sprintf("unknown FilePerm: %d", p) // don't use %s here :)
}

func main() {
    fmt.Println(Execute)        // execute
    fmt.Printf("%d\n", Execute) // 4
}
```

我将把它留作练习，让你实现一个支持位掩码的 String() 字符串方法。 ☺

## 进一步阅读

- 维基百科上的枚举类型
    http://en.wikipedia.org/wiki/Enumerated_type
- 物联网
    http://github.com/golang/go/wiki/Iota
- 有效围棋中的常量
    http://golang.org/doc/effective_go.html#constants
- 维基百科上的按位运算
    http://en.wikipedia.org/wiki/Bitwise_operation

## 脚注
[1] https://golang.org/ref/spec#String_literals

[2]https://golang.org/ref/spec#Lexical_elements