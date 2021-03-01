# Go interface



在 Golang 中，interface 是一组 method 的集合，是 duck-type programming 的一种体现。不关心属性（数据），只关心行为（方法）。具体使用中你可以自定义自己的 struct，并提供特定的 interface 里面的 method 就可以把它当成 interface 来使用。下面是一种 interface 的典型用法，定义函数的时候参数定义成 interface，调用函数的时候就可以做到非常的灵活。

```
type MyInterface interface{
    Print()
}

func TestFunc(x MyInterface) {}
type MyStruct struct {}
func (me MyStruct) Print() {}

func main() {
    var me MyStruct
    TestFunc(me)
}
```



## 接口举例

```go
package main

import (
	"fmt"
)

type Animal interface {
	Speak() string
}

type Dog struct {
}

func (d Dog) Speak() string {
	return "Woof!"
}

func main() {
	animals := []Animal{Dog{}}
	for _, animal := range animals {
		fmt.Println(animals)
		fmt.Println(animal.Speak())
	}
}
```

输出

```
{}
Woof!
```

