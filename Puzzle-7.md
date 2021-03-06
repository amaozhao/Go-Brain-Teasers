## 数字会撒谎吗？

```go
package main

import (
    "fmt"
)

func main() {
    n := 1.1
    fmt.Println(n * n)
}
```

## 猜输出

此代码将打印：1.2100000000000002


您可能已经预料到 1.21，这是正确的数学答案。

一些新开发人员看到这个或类似的输出时，来到留言板说：“我们在 Go 中发现了一个错误！”通常的答案是“阅读精美手册”（RTFM）。

> 浮点有点像量子物理学：你看得越近，它就越混乱。
>
> — 格兰特·爱德华兹

这个问题背后的基本思想是，在浮点数中，我们为了速度牺牲了准确性（即作弊）。不要感到震惊；这是我们在计算机科学中做了很多的权衡。

您看到的结果符合浮点规范。如果您在 Python、Java、C 中运行相同的代码……您将看到相同的输出。

如果您有兴趣了解有关浮点工作原理的更多信息，请参阅以下部分中的链接。您需要记住的要点是它们不准确，并且随着数字变大，准确度会变差。

一个含义是，当使用浮点测试时，您需要检查“大致相等”并确定可接受的阈值。测试库比如stretchr/testify有现成的函数（比如InDelta）来检查两个浮点数是否近似相等。

浮点数还有其他一些奇怪之处。例如，有一个特殊的 NaN 值（非数字的缩写）。 NaN 不等于任何数字，包括它自己。以下代码将打印错误：

```fmt.Println(math.NaN() == math.NaN())```
要检查您是否获得了 NaN，您需要使用一个特殊的函数，例如 math.IsNaN。

如果您需要更高的准确性，请查看 math/big 或外部包，例如 shopspring/decimal。

## 进一步阅读

- Julia Evans 的浮点杂志
    http://twitter.com/b0rk/status/986424989648936960
- 每个计算机科学家都应该了解的关于浮点运算的知识
    http://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html
- 维基百科上的浮点规范
    http://en.wikipedia.org/wiki/IEEE_754