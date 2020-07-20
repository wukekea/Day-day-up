* 协程示例如下
```golang
package main

import (
   "fmt"
   "runtime"
)

func main(){
	// 获取goroot目录
	goroot := runtime.GOROOT()
	fmt.Println(goroot)

	// 获取当前用户操作系统
	goos := runtime.GOOS
	fmt.Println(goos)

	// 获取逻辑CPU的数量
	numcpu := runtime.NumCPU()
	fmt.Println(numcpu)
	// 设置Go程序执行的最大的可同时使用的CPU核数
	n := runtime.GOMAXPROCS(runtime.NumCPU())
    fmt.Println(n)
    
    
    
    go func() {
		for i := 0; i < 7; i++ {
			fmt.Println("wu lala")
		}
	}()
	for i := 0; i < 5; i++ {
		//让出时间片，先让别的协议执行，它执行完，再回来执行此协程
		runtime.Gosched()
		fmt.Println("second")
	}
}

```