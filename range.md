# Range

go语言中range关键字用于for循环中迭代数组(array)、切片(slice)、通道(channel)或集合(map)的元素。

* 在数组和切片中返回元素的索引和索引对应的值
* 在集合中返回key-value的值对

```go
package main

import (
	"fmt"
)

func main() {
	//声明数组和sliec
	yyparr := [3]int{2, 3, 4}
	yypslice := []int{5, 6, 7}

	//声明map
	yypmap := map[string]string{"key1": "value1", "key2": "value2"}

	for i := range yyparr {
		fmt.Printf("数组的索引值 %d的值是 %v\n", i, yyparr[i])
	}

	for i := range yypslice {
		fmt.Printf("切片的索引值 %d的值是 %v\n", i, yypslice[i])
	}

	for i := range yypmap {
		fmt.Printf("map的索引值 %v的值是 %v\n", i, yypmap[i])
	}
}
```

输出

```go
数组的索引值 0的值是 2
数组的索引值 1的值是 3
数组的索引值 2的值是 4
切片的索引值 0的值是 5
切片的索引值 1的值是 6
切片的索引值 2的值是 7
map的索引值 key1的值是 value1
map的索引值 key2的值是 value2
```

