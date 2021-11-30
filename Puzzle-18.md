## 一份工作

```go
package main

import (
    "fmt"
)

type Job struct {
    State string
    done  chan struct{}
}

func (j *Job) Wait() {
    <-j.done
}

func (j *Job) Done() {
    j.State = "done"
    close(j.done)
}

func main() {
    ch := make(chan Job)
    go func() {
        j := <-ch
        j.Done()
    }()

    job := Job{"ready", make(chan struct{})}
    ch <- job
    job.Wait()
    fmt.Println(job.State)
}
```

## 猜输出

此代码将打印：就绪


乍一看，代码好像没问题。您在 Job 结构体方法中使用了指针接收器。对 Wait 的调用终止的事实告诉您通道已关闭。

问题在于ch的定义。它是 Job 的通道，而不是 *Job，这意味着当您通过该通道发送可变作业时，您实际上发送了它的副本。 Go 中的通道是类似指针的类型，因此即使在 goroutine 中有 job 的副本，j.done 也指向 job.done 指向的同一个通道。

Go 中的字符串不像指针。当您调用 j.Done()（goroutine 中的字符串）时，您会更改作业的 goroutine 副本中 State 字段的值。此更改未反映在第 28 行声明的作业变量中。

解决办法是让ch type *Job。

```go
package main

import (
    "fmt"
)

type Job struct {
    State string
    done  chan struct{}
}

func (j *Job) Wait() {
    <-j.done
}

func (j *Job) Done() {
    j.State = "done"
    close(j.done)
}

func main() {
    ch := make(chan *Job)
    go func() {
        j := <-ch
        j.Done()
    }()

    job := Job{"ready", make(chan struct{})}
    ch <- &job
    job.Wait()
    fmt.Println(job.State)
}
```

## 进一步阅读

- Go 中没有传递引用
    http://dave.cheney.net/2017/04/29/there-is-no-pass-by-reference-in-go
- 通道类型规范
    http://golang.org/ref/spec#Channel_types
- Go 并发模式：管道和取消
    http://blog.golang.org/pipelines
- Go Tour 中的频道
    http://tour.golang.org/concurrency/2