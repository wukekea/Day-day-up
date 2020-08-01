* Go语言中的`uintptr`、`unsafe.Pointer`和`普通指针`的区别
* unsafe.Pointer
    * Pointer代表指向任意类型的指针（类似于空指针），它不能进行指针运算（即不能解引用、ptr++等运算）。Pointer可以进行四种特有的操作：
        - A pointer value of any type can be converted to a Pointer.
    	- A Pointer can be converted to a pointer value of any type.
    	- A uintptr can be converted to a Pointer.
    	- A Pointer can be converted to a uintptr.
        ```golang
        package main
        import (
	        "fmt"
	        "unsafe"
        )
        func main() {
	        var f float64 = 10.0

	        // 两条地址输出语句相同
	        fmt.Println(&f)
            u := unsafe.Pointer(&f)
            fmt.Println(u)

            // 错误，u无法进行解引用操作
            // fmt.Println(*u)

            ptr := (*uint64)(u)
            fmt.Println(ptr)
            fmt.Println(*ptr)

            // 输出结果：
            // 0xc000066058
            // 0xc000066058
            // 0xc000066058
            // 4621819117588971520
        }
        ```
   

* uintptr
    * 一个uintptr是一个整数，而不是引用。它可以进行指针运算<br>
    p = unsafe.Pointer(uintptr(p) + offset):<br>
    它可以用来获取一个结构体中某个属性的地址或数组中某个元素的地址。
    ```golang
    // equivalent to f := unsafe.Pointer(&s.f)
	f := unsafe.Pointer(uintptr(unsafe.Pointer(&s)) + unsafe.Offsetof(s.f))

	// equivalent to e := unsafe.Pointer(&x[i])
	e := unsafe.Pointer(uintptr(unsafe.Pointer(&x[0])) + i*unsafe.Sizeof(x[0]))
    ```



* unsafe包的使用
```golang
// unsafe包中的类型及函数
type ArbitraryType int
type Pointer *ArbitraryType

// Sizeof()函数接收任意类型的变量，返回该类型的值所占的内存大小（字节数）
// 需要注意的是，如果x是切片类型，其返回该切片类型的大小而不是切片引用的内存的大小
// 该函数的返回值是一个常量
func Sizeof(x ArbitraryType) uintptr

// Offsetof()函数返回变量x所属的结构体中某个属性的偏移量
// 换句话说，它将会返回从结构体开始的地址到某个属性开始的地址之间的字节数
// 其返回值是一个常量
func Offsetof(x ArbitraryType) uintptr

// Alignof()函数返回一个类型的对齐值，也可以叫对齐系数或对齐倍数
// 对齐值一般是2^n,且不会超过8
// 该函数等效于：reflect.TypeOf(x).Align()
func Alignof(x ArbitraryType) uintptr
```

```golang
// Sizeof(x)函数测试代码
package main
import (
	"fmt"
	"unsafe"
)

type ICanTalk interface {
	Talk(str string)
}
type Nothing struct {}
type Student struct {
	Name 	string
	Age 	int
}
type RStudent struct {
	Age 	int 
	Name 	string 
}
type test struct {
	a 		int8
	b 		int64
	c 		int32
}
func main() {
	// 	切片占位符占24字节
	var a []int 
	var b = []int{1,2,3,4}
	var c = []int{1,2,3,4,5,6,7,8}
	var d = []string{"hello", "world"}
	fmt.Println(unsafe.Sizeof(a))		// 24
	fmt.Println(unsafe.Sizeof(b))		// 24
	fmt.Println(unsafe.Sizeof(c))		// 24
	fmt.Println(unsafe.Sizeof(d))		// 24

	// 数组类型返回该数组的大小
	var e = [...]int{1,2,3,4,5,6,7,8}
	var f = [...]string{"hello", "world"}
	fmt.Println(unsafe.Sizeof(e))		// 8*8=64
	fmt.Println(unsafe.Sizeof(f))		// 16*2=32

	// 接口、空接口类型占16字节
	var g ICanTalk
	var h interface{}
	fmt.Println(unsafe.Sizeof(g))		// 16
	fmt.Println(unsafe.Sizeof(h))		// 16

	// 空结构体占0字节
	var i Nothing
	fmt.Println(unsafe.Sizeof(i))		// 0t

	// 结构体占位，需要考虑内存对齐
	var j Student
	fmt.Println(unsafe.Sizeof(j))		// 16+8=24
	// 最大对齐量为8，所以此处不是16+16.string本质是一个结构体。
	// string本质上是一个结构体
	var k RStudent
	fmt.Println(unsafe.Sizeof(k))		// 8+16=24
	var L RStudent = RStudent{Age:10, Name:"wukeke"}
	fmt.Println(unsafe.Sizeof(L))
	// 类型的尺寸需要为对齐的倍数，因此c需要加4字节
	var m test
	fmt.Println(unsafe.Sizeof(m))		// 8+8+8=24
}
```

```golang
package main
import (
	"fmt"
	"unsafe"
)
type test struct {
	a 		int8
	b 		int64
	c 		int32
}

func main() {
	var t test = test{1,2,3}
	fmt.Println(t)
	// 实参必须是结构体中的某个属性
	fmt.Println(unsafe.Offsetof(t.a))		// 0
	fmt.Println(unsafe.Offsetof(t.b))		// 8
	fmt.Println(unsafe.Offsetof(t.c))		// 16

}
```

```golang
package main
import (
	"fmt"
	"unsafe"
)
type test struct {
	a 		int8
	b 		int64
	c 		int32
}

func main() {
	var b bool
	var c int8
	var d int16
	var e int32
	var f int64
	var g string
	fmt.Println(unsafe.Alignof(b))			// 1
	fmt.Println(unsafe.Alignof(c))			// 1
	fmt.Println(unsafe.Alignof(d))			// 2
	fmt.Println(unsafe.Alignof(e))			// 4
	fmt.Println(unsafe.Alignof(f))			// 8
	fmt.Println(unsafe.Alignof(g))			// 8

	var t test
	fmt.Println(unsafe.Alignof(t.a))		// 1
	fmt.Println(unsafe.Alignof(t.b))		// 8
	fmt.Println(unsafe.Alignof(t.c))		// 4
}
```

```golang
package main
import (
	"fmt"
	"unsafe"
)
type test struct {
	a 		int8
	b 		int64
	c 		int32
}

func main() {
	var t *test = &test{1,2,3}
	a := (*int8)(unsafe.Pointer(t))		// a的地址即t.a的地址
	fmt.Println(*a)						// 1(t.a的值)
	*a = 10
	fmt.Println(t.a)					// 10

	// uintptr是一个整型类型，它可以获取一个Pointer指针的地址
	// b的地址即是t.b的地址
	b := (*int64)(unsafe.Pointer(uintptr(unsafe.Pointer(t))+unsafe.Offsetof(t.b)))
	fmt.Println(*b)
}
```
* 详见：https://www.cnblogs.com/-wenli/p/12682477.html
