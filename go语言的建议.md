# 	编写go 代码的建议



1. go tools 只是在你本地的工作区进行兼容

   要保证自身的gopath 的工作区等，以及pkg的各种版本，因为go代码没有很好的版本管理，有可能出现使用了新版本不兼容的问题，在如果使用需要的第三方外部的库时，要保持版本

2. 写go 语言需要使用高质量的import 导入，从 `$GOPATH` 开始导入

   ```go
   .
   └── src
       └── gopher
           ├── main.go
           └── sub
               └── sub.go
    
   main中的导入为
   import "gopher/sub"
   而不是 ./sub 等
   ```

3. 

`学go`

```
https://github.com/Unknwon/go-fundamental-programming  视频


https://github.com/chuhemiao/go-basic-example 

https://github.com/talkgo/read

https://github.com/bingohuang/effective-go-zh-en

https://github.com/golang/go/wiki/GoTalks
```



