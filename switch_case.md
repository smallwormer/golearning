# Switch case



如果if else 超过两到3个得话，建议使用switch case 方便阅读

fallthrough

```
package main

import (
	"fmt"
)

func checkValue(s int) {
	switch s {
	case 0:
		fallthrough
	case 1:
		fmt.Println("check value is ", s)
	}
}

func main() {
	checkValue(0)
	checkValue(1)
}
```

