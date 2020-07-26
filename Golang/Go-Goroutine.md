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

* 生产者消费者模型：
```golang
package main
import (
	"time"
	"fmt"
)
type Product struct {
	Commodity 	string
	id			int

}
func main() {
	CHStorage := make(chan Product, 5)
	CHShop := make(chan Product, 5)
	CHDone := make(chan bool)
	ID := 1

	go Produce(CHStorage, ID)
	go Logistics(CHStorage, CHShop)
	go Consumer(CHShop, CHDone)

	if <- CHDone {
		fmt.Println("main end ...")
	}
}

func Produce(CHStorage chan Product, ID int) {
	for {
		product := Product {
			Commodity: "book",
			id: ID,
		}
		ID++
		CHStorage <- product
		fmt.Printf("%s, %d：转入仓库\n", product.Commodity, product.id)
		time.Sleep(1*time.Second)
	}
}

func Logistics(CHStorage chan Product, CHShop chan Product) {
	for {
		product := <- CHStorage
		fmt.Printf("\t%s, %d：转出仓库\n", product.Commodity, product.id)
		CHShop <-product
		fmt.Printf("\t%s, %d：转入商店\n", product.Commodity, product.id)
		time.Sleep(2*time.Second)
	}
}

func Consumer(CHShop chan Product, CHDone chan bool) {
	for {
		product := <- CHShop
		fmt.Printf("\t\t%s, %d：进行消费\n", product.Commodity, product.id)
		time.Sleep(1*time.Second)
		if product.id == 30 {
			CHDone <- true
		}
	}
}
```