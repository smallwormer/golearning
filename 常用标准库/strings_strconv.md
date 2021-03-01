# 字符串操作

## strings

* 字符串比较`strings.Compare(a, b string) int`

  ```
  strings.Compare(a, b string) int
  
  a== b  返回0
  a<b 返回-1
  a>b 返回1
  ```

* 字符串包含`strings.Contains(s, substr string) bool`

  ```go
  package main
  
  import (
  	"fmt"
  	"strings"
  )
  
  func main() {
  	fmt.Println(strings.Contains("seafood", "foo"))
  	fmt.Println(strings.Contains("seafood", "bar"))
  	fmt.Println(strings.Contains("seafood", ""))
  	fmt.Println(strings.Contains("", ""))
  }
  ```

* 

