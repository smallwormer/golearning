# 切片

为什么有了数组还要切片。数组是静态的长度是不可变得，切片可以理解为动态数组，是对数组的抽象，在某些特殊的场景下 切片比数组更加的灵活。

可以理解为 slice总是指向底层的一个数组，slice是一个指向array的指针，这是其与array不同的地方.

实际的是获取数组的某一部分，**len切片<=cap切片<=len数组**，切片由三部分组成：指向底层数组的指针、len、cap

切片与数组都是同样是以 0 开始作为索引值

## 声明定义切片

```go
var slice1 []type

slc := make([]int, 10)
```

* 创建了一个保存10个元素的切片slc,元素类型为`int`



## len and cap

切片的索引，使用len() 方法获取切片的长度, cap() 方法是计算切片的容量的方法，可以获取到切片最长可以达到多少(与默认的引用数组有关)

```
slc := make([]int, 10, 15)
make([]T, length, capacity)
一般capacity 是可以省略的，最大容量长度
r
表示初始化的sli 切片，目前长度是10，最大容量为15
```

`note` 切片在未初始化之前为nil，长度为0



## 切片的增删

* append()
* opy()

1. 如果想对切片进行扩展，就必须增加切片的容量，需要创建一个新的更大的切片将原来的切片的内容拷贝过来

   ```go
   package main
   
   import (
   	"fmt"
   )
   
   func modify(foo []string) {
   	foo[1] = "c"
   
   	fmt.Println("modify foo", foo)
   }
   
   func addValuse(foo []string, value string) {
   	foo = append(foo, value)
   
   	fmt.Println("modify foo", foo)
   }
   
   func main() {
   	foo := []string{"a", "b"}
   	fmt.Println("before foo:", foo)
   
   	// modify(foo)
   	// fmt.Println("after foo:", foo)
   
   	addValuse(foo, "c")
   	fmt.Println("addvalue foo:", foo)
   
   	//增加多个元素
   	var bar []int
   	printSliceinfo(bar)
   	bar = append(bar, 1, 3)
   	printSliceinfo(bar)
   	//创建新扩展切片
   	bar2 := make([]int, len(bar), (cap(bar) * 2))
   	copy(bar2, bar)
   	printSliceinfo(bar2)
   	bar2 = append(bar2, 3, 1, 4, 5, 5, 6, 6)
   	printSliceinfo(bar2)
   }
   
   func printSliceinfo(x []int) {
   	fmt.Printf("slice len=%d, cap=%d, slice=%v\n", len(x), cap(x), x)
   }
   ```

   输出

   ```go
   before foo: [a b]
   modify foo [a b c]
   addvalue foo: [a b]
   slice len=0, cap=0, slice=[]
   
   slice len=2, cap=2, slice=[1 3]
   //cap 不变
   slice len=2, cap=4, slice=[1 3]
   // 这里cap 会变化，如果append时，原来的cap 能容纳新增的数据，则cap 不变，如果不可以容纳，则会计算出可以容纳的最大值
   slice len=9, cap=10, slice=[1 3 3 1 4 5 5 6 6]
   ```

`note` 后续看go 代码实现再来补充确认 `$GOROOT/src/runtime/slice.go`

```go
当cap<1024时，cap的变化，是自动扩充原切片的两倍来满足需要增加的切片内容。
goroot 源码

newcap := old.cap
doublecap := newcap + newcap
if cap > doublecap {
    newcap = cap
} else {
    if old.len < 1024 {
        newcap = doublecap
    } else {
        // Check 0 < newcap to detect overflow
        // and prevent an infinite loop.
        for 0 < newcap && newcap < cap {
            newcap += newcap / 4
        }
        // Set newcap to the requested cap when
        // the newcap calculation overflowed.
        if newcap <= 0 {
            newcap = cap
        }
    }
}


首先判断，如果新申请容量（cap）大于2倍的旧容量（old.cap），最终容量（newcap）就是新申请的容量（cap）。
否则判断，如果旧切片的长度小于1024，则最终容量(newcap)就是旧容量(old.cap)的两倍，即（newcap=doublecap），
否则判断，如果旧切片长度大于等于1024，则最终容量（newcap）从旧容量（old.cap）开始循环增加原来的1/4，即（newcap=old.cap,for {newcap += newcap/4}）直到最终容量（newcap）大于等于新申请的容量(cap)，即（newcap >= cap）
如果最终容量（cap）计算值溢出，则最终容量（cap）就是新申请容量（cap）
```



## 数组与切片的对比



做函数调用时，slice 按引用传递，array 按值传递

```go
package main

import "fmt"

func main(){
  changeSliceTest()
}

func changeSliceTest() {
    arr1 := []int{1, 2, 3}
    arr2 := [3]int{1, 2, 3}
    arr3 := [3]int{1, 2, 3}

    fmt.Println("before change arr1, ", arr1)
    changeSlice(arr1) // slice 按引用传递
    fmt.Println("after change arr1, ", arr1)

    fmt.Println("before change arr2, ", arr2)
    changeArray(arr2) // array 按值传递
    fmt.Println("after change arr2, ", arr2)

    fmt.Println("before change arr3, ", arr3)
    changeArrayByPointer(&arr3) // 可以显式取array的 指针
    fmt.Println("after change arr3, ", arr3)
}

func changeSlice(arr []int) {
    arr[0] = 9999
}

func changeArray(arr [3]int) {
    arr[0] = 6666
}

func changeArrayByPointer(arr *[3]int) {
    arr[0] = 6666
}
```

输出：

```
before change arr1,  [1 2 3]
after change arr1,  [9999 2 3]
before change arr2,  [1 2 3]
after change arr2,  [1 2 3]
before change arr3,  [1 2 3]
after change arr3,  [6666 2 3]
```

