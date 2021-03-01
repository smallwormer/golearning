# 如何使用func



## 单一回传值

```
package main

import (
	"fmt"
)

func add(i, j int) int {
	return i + j
}
func main() {
	fmt.Println(add(1, 2))

}
```

多回传值

```
package main

import (
	"fmt"
)


func swap(i, j int) (int, int) {
	return j, i
}
func main() {
	a := 1
	b := 2
	a, b = swap(a, b)
	fmt.Print("a:", a)       //2
	fmt.Print("b:", b)       //1
}
```



## ss

```

	fmt.Println("b:", b)
	foo := func() string {
		return "hello fooo"
	}
	fmt.Println(foo())

	bar := func() {
		fmt.Println("Hello World bar")

	}
	bar()
```



## 设计

```go
package main

import (
	"fmt"
	"strings"
)


func getUserListSql(username, email string) string {
	sql := "select * from user"
	where := []string{}
	if username != "" {
		where = append(where, fmt.Sprintf("username= '%s'", username))
	}

	if email != "" {
		where = append(where, fmt.Sprintf("email= '%s'", email))
	}

	return sql + " where " + strings.Join(where, " or ")
}

func main() {
	fmt.Println(getUserListSql("yyp", ""))
	fmt.Println(getUserListSql("yyp2", "ssss@gmail.com"))
}
```

`note`  函数设计一般也需要使用struct 来做统管，方便扩展等。上述可以改变如下,

如果需要增加新的参数，则只需要增加 struct 即可，方法中可以使用opts 来调用其他的方法

```go
package main

import (
	"fmt"
	"strings"
)

type searchOpts struct {
	username string
	email    string
	age		 int
}

func getUserOptsSql(opts searchOpts) string {
	sql := "select * from user"
	where := []string{}
	if opts.username != "" {
		where = append(where, fmt.Sprintf("username= '%s'", opts.username))
	}

	if opts.email != "" {
		where = append(where, fmt.Sprintf("email= '%s'", opts.email))
	}
	
	if opts.age != 0 {
		where = append(where, fmt.Sprintf("age= '%d'", opts.age))
	}
	
	return sql + " where " + strings.Join(where, " or ")
}

func main() {
	fmt.Println(getUserOptsSql(searchOpts{
		username: "yyy1",
		email:    "e123@gmail.com",
    age: 10
	}))
}

```

## 如何回错误信息

```go
package main

import (
	"errors"
	"fmt"
	"strings"
)


func checkUsernameExist(username string) (bool, error) {
	if username == "sss" {
		return true, fmt.Errorf("username %s aleady exist", username)
	}
	if username == "foo" {
		return true, errors.New("username foo aleady exist")
	}
	return false, nil
}


func main() {
	if _, err := checkUsernameExist("sss"); err != nil {
		fmt.Println(err)
	}
	if _, err := checkUsernameExist("foo"); err != nil {
		fmt.Println(err)
	}
}
```

