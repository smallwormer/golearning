# map

map是一种无序的键值对的集合，map最重要的一点是通过key 来快速检索数据，key类似于索引，指向数据的值。Map 是一种集合，所以我们可以像迭代数组和切片那样迭代它。不过，Map 是无序的，我们无法决定它的返回顺序，这是因为 Map 是使用 hash 表来实现的。



## 声明map

```
var var_map map[key_type]value_type

var_map := make(map[key_type][value_type])
```

## 使用map



```go
package main

import (
	"fmt"
)

func main() {
	//声明 map
	//var mapvars map[string]string
	mapvars := make(map[string]string)
	//mapvar2 := map[string]string{"yy": "xx", "ssss": "ss12"}

	key1 := "mapkey1"
	value1 := "mapvalue1"
	//使用map
	mapvars["yyp"] = "ma"
	mapvars["emma"] = "drangon"
	mapvars[key1] = value1

	for name := range mapvars {
		fmt.Println(name, "daihao", mapvars[name])
	}
	fmt.Println("xxxxx   delete keys1 startingxxxxxx")
	delete(mapvars, key1)
	for name := range mapvars {
		fmt.Println(name, "daihao", mapvars[name])
	}
}
```

输出

```go
yyp daihao ma
emma daihao drangon
mapkey1 daihao mapvalue1
xxxxx   delete keys1 startingxxxxxx
yyp daihao ma
emma daihao drangon
```



