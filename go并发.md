# go并发

go只是的并发是使用go 关键字开启goroutine。goroutine是轻量级的线程，调度方式是由golang管理



## 语法结构

```
go 函数名(参数列表)
go f(x,y)
```

Go 允许使用 go 语句开启一个新的运行期线程， 即 goroutine，以一个不同的、新创建的 goroutine 来执行一个函数。 同一个程序中的所有 goroutine 共享同一个地址空间。

```go
package main

import (
	"fmt"
	"time"
)

func say(s string) {
	for i := 0; i < 5; i++ {
		time.Sleep(100 * time.Millisecond)
		fmt.Println(s)
	}
}
func main() {
	go say("world")
	say("hello")
}
```

输出：(执行多次，输出结果是不同的，也没有一定的先后顺序，这就是goroutine)  有时候也会出现 在使用goroutine的时候，并没有全部执行，因为这里会出现，在线程还没有执行完的时候，这里主进程就退出了。有某种方式来保证线程的正常执行完成后，主函数才退出呢，引入了 通道channel的概念

```
hello
world
world
hello
hello
world
world
hello
hello
world
```

## channel

### 创建channel

```go
var c chan int   			// 声明一个 接收int 类型的 chan
ci := make(chan int)     //  创建 channel ci 用于接收以及发送整数
cs := make(chan string)  //  创建channel 用于字符串
cf := make(chan interface{})  //  创建channel cf 使用空接口来满足各种类型。
```

### 使用channel

```go
ci <- 1   // 发送整数1  到 channel ci
<-ci			// 从ci channel 中接收整数
a := <- ci // 定义a 用于接收channel的数据1，也可以理解为 从 channel ci中接收整数，并保存到a中
```



goroutine 是 golang 中在语言级别实现的轻量级线程，仅仅利用 go 就能立刻起一个新线程。多线程会引入线程之间的同步问题，在 golang 中可以使用 channel 作为同步的工具。

通过 channel 可以实现两个 goroutine 之间的通信。

创建一个 channel， make(chan TYPE {, NUM}) TYPE 指的是 channel 中传输的数据类型，第二个参数是可选的，指的是 channel 的容量大小。

向 channel 传入数据， CHAN <- DATA ， CHAN 指的是目的 channel 即收集数据的一方， DATA 则是要传的数据。

从 channel 读取数据， DATA := <-CHAN ，和向 channel 传入数据相反，在数据输送箭头的右侧的是 channel，形象地展现了数据从隧道流出到变量里。



```go

package main

import "fmt"

func main() {
	ch := make(chan int, 2)

	ch <- 1   //将数值1  传入channel通道
	a := <-ch // channel的数值 ch 被a 接收。通道中已经没有了数据
	ch <- 2   // 继续传值进隧道 2
	ch <- 3   // 继续传值进隧道 3

	fmt.Println(<-ch) //	先进先出。这里是2 先被接收
	fmt.Println(<-ch) //	后进后出。这里是3 先被接收
	fmt.Println(a)    // a 1
}

```

