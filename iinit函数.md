# Init



每个源文件可以定义自己的不带参数的（niladic）init函数，来设置它所需的状态。（实际上每个文件可以有多个init函数。）init是在程序包中所有变量声明都被初始化，以及所有被导入的程序包中的变量初始化之后才被调用。
除了用于无法通过声明来表示的初始化以外，init函数的一个常用法是在真正执行之前进行验证或者修复程序状态的正确性

```go
func init() {
    if user == "" {
        log.Fatal("$USER not set")
    }
    if home == "" {
        home = "/home/" + user
    }
    if gopath == "" {
        gopath = home + "/go"
    }
    // gopath may be overridden by --gopath flag on command line.
    flag.StringVar(&gopath, "gopath", gopath, "override default GOPATH")
}

```

