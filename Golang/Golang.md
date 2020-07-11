```
Ctrl + Shift + V进行预览
```

### Go语言结构
* Go语言的基础组成有以下六个部分：`包声明`、`引入包`、`函数`、`变量`、`语句&表达式`、`注释`。一个基本的Go程序示例如下：
```golang
// package main 定义了包名。你必须在源文件中非注释的第一行指明这个文件属于哪个包，如：package main。package main表示一个可独立执行的程序，
// 每个 Go 应用程序都包含一个名为 main 的包。
package main

import "fmt"

// func main() 是程序开始执行的函数。main 函数是每一个可执行程序所必须包含的，一般来说都是在启动后第一个执行的函数（如果有 init() 函数则会先
// 执行该函数）。
func main() {
	fmt.Println("hello world")
}

// 当标识符（包括常量、变量、类型、函数名、结构字段等等）以一个大写字母开头，如：Group1，那么使用这种形式的标识符的对象就可以被外部包的代码所
// 使用（客户端程序需要先导入这个包），这被称为导出（像面向对象语言中的 public）；标识符如果以小写字母开头，则对包外是不可见的，但是他们在整
// 个包的内部是可见并且可用的（像面向对象语言中的 private ）。
```

### Go语言基础语法
* Go语言的字符串可以通过`+`进行连接
* Go语言中的25个关键字或保留字如下：

|			|				|		|			|		|
|----		|----			|----	|----		|----	|
|break		|default		|func	|interface	|select	|
|case		|defer			|go		|map		|struct	|
|chan		|else			|goto	|package	|switch	|
|const		|fallthrough	|if		|range		|type	|
|continue	|for			|import	|return		|var	|

### Go语言数据类型
|序号	|类型和描述	|
|----	|----		|
|1		|布尔型。布尔型的值只可以是常量 true 或者 false。一个简单的例子：var b bool = true。	|
|2		|数字类型。整型 int 和浮点型 float，Go 语言支持整型和浮点型数字，并且原生支持复数，其中位的运算采用补码。|
|3		|字符串类型。字符串就是一串固定长度的字符连接起来的字符序列。Go的字符串是由单个字节连接起来的。Go语言的字符串的字节使用UTF-8编码标识Unicode文本。|
|4		|派生类型:<br>包括：<br>(a) 指针类型（Pointer）<br>(b) 数组类型<br>(c) 结构化类型(struct)<br>(d) 联合体类型 (union)<br>(e) 函数类型<br>(f) 切片类型<br>(g) 接口类型（interface）<br>(h) Map 类型<br>(i) Channel 类型|

#### 数字类型
|序号		|类型和描述		|
|---		|---			|
|1			|uint8<br>无符号 8 位整型 (0 到 255)	|
|2			|uint16<br>无符号 16 位整型 (0 到 65535)|
|3			|uint32<br>无符号 32 位整型 (0 到 4294967295)|
|4			|uint64<br>无符号 64 位整型 (0 到 18446744073709551615)|
|5			|int8<br>有符号 8 位整型 (-128 到 127)	|
|6			|int16<br>有符号 16 位整型 (-32768 到 32767)|
|7			|int32<br>有符号 32 位整型 (-2147483648 到 2147483647)|
|8			|int64<br>有符号 64 位整型 (-9223372036854775808 到 9223372036854775807)|

#### 浮点型
|序号		|类型和描述		|
|---		|---			|
|1			|float32<br>IEEE-754 32位浮点型数	|
|2			|float64<br>IEEE-754 64位浮点型数	|
|3			|complex64<br>32 位实数和虚数		|
|4			|complex128<br>64 位实数和虚数		|

#### 其它数字类型
|序号		|类型和描述		|
|---		|---			|
|1			|byte<br>类似 uint8		|
|2			|rune<br>类似 int32		|
|3			|uint<br>32 或 64 位	|
|4			|int<br>与 uint 一样大小|
|5			|uintptr<br>无符号整型，用于存放一个指针|

### Go语言变量
```golang
// 指定变量类型，声明后若不赋值，使用默认值。下面的a默认为0
var a int
var a2 int = 1

// 不指定变量类型，b的类型根据后面赋的值推断出来
var b = 1

// 省略var, 注意 :=左侧的变量不应该是已经声明过的，否则会导致编译错误。这种声明只能出现在函数体内
// 注意，= 使用必须使用var进行声明，而 := 是声明并赋值，并且系统自动推断类型，不需要var关键字
c := 11

// 数组初始化，d = {1, 2, 0, 0, 0}
d := [5]int{1, 2}
```

#### 多变量的声明
```golang
var a, b, c int
a, b, c = 1, 2, 3

var d, e, f = a, b, c

// 这种声明只能出现在函数体内
g, h, i := d, e, f

var (
	a int = 1
	b int = 2
)
```

* 值类型：所有像 int、float、bool 和 string 这些基本类型都属于值类型，值类型变量的值储存在栈中
* 引用类型：特指slice、map、channel这三种预定义类型。引用类型拥有更复杂的存储结构:<br>
(1)分配内存<br>
(2)初始化一系列属性等一个引用类型的变量r1存储的是r1的值所在的内存地址（数字），或内存地址中第一个字所在的位置，这个内存地址被称之为指针，这个指针实际上也被存在另外的某一个字中。  
* 值类型和引用类型的主要差别是：拷贝操作和函数传参。
* 局部变量声明但是不使用将会报错，全局变量允许声明但不使用
* 交换两个变量中的值：a, b = b, a
* 空白标识符 _ 也被用于抛弃值，如值 5 在：_, b = 5, 7 中被抛弃。<br>
_ 实际上是一个只写变量，你不能得到它的值。这样做是因为 Go 语言中你必须使用所有被声明的变量，但有时你并不需要使用从一个函数得到的所有返回值。
* 常量：go语言使用const定义常量。常量可以用len(), cap(), unsafe.Sizeof()常量计算表达式的值。常量表达式中，函数必须是内置函数，否则编译不过：<br>
```golang
package main

import "unsafe"
const (
    a = "abc"
    b = len(a)
    c = unsafe.Sizeof(a)	// 注意，c的类型为uintptr.语句：c int = unsafe.Sizeof(a)将会报错
)

func main(){
    println(a, b, c)
}
```
* iota：特殊常量，可以认为是一个可以被编译器修改的常量。相当于一个常量计数器<br>
在每一个const关键字出现时，被重置为0，然后再下一个const出现前，每出现一个常量，其所代表的数字会自动增加1。iota 可以被用作枚举值。<br>
注意：go语言的常量只能是布尔型、数字型（整数型、浮点型和复数）和字符串型。<br>
iota用法示例如下：
```golang
package main

import (
	"fmt"
	// "unsafe"
)

func main() {
	const (
		// a 			// 此处报错：常量声明过程中未赋值
		b = 10			// 第一个常量。此时iota = 0
		c 				// 此处可以不赋值，c的值默认与b的右值相同, c = 1； iota = 1
		g = "hello"		// iota = 2
		d = iota 		// d = iota = 3
		e = iota 		// e = iota = 4
		f 				// f = iota = 5
	)
	const (				// 出现新的const
		h = 20			// iota重置为 0
		i = iota 		// i = iota = 1
		j = 5<<iota 	// iota = 2, j为5左移2位，即j = 5 * 2 * 2 = 20
		k 				// iota = 3, k = 5<<3 = 40
	)
	fmt.Println(b, c, g, d, e, f, h, i, j, k)
}
```
### go语言运算符
* 位运算符：对整数在内存中的二进制位进行操作。位运算符 &, |, 和 ^ 的计算法制如下：

|p	|q	|p & q	|p \| q	|p ^ q	|
|---|---|---	|---	|---	|
|0	|0	|0		|0		|0		|
|0	|1	|0		|1		|1		|
|1	|1	|1		|1		|0		|
|1	|0	|0		|1		|1		|
* 假定 A 为60，B 为13,运算符如下表所示：

|运算符		|描述					|实例	|
|-----		|---					|---	|
|&	|按位与运算符"&"是双目运算符。<br>其功能是参与运算的两数各对应的二进位相与。|(A & B) 结果为 12, 二进制为 0000 1100|
|\|	|按位或运算符"|"是双目运算符。<br>其功能是参与运算的两数各对应的二进位相或	|(A | B) 结果为 61, 二进制为 0011 1101|
|^	|按位异或运算符"^"是双目运算符。<br>其功能是参与运算的两数各对应的二进位相异或，当两对应的二进位相异时，结果为1。|(A ^ B) 结果为 49, 二进制为 0011 0001|
|<<	|左移运算符"<<"是双目运算符。左移n位就是乘以2的n次方。<br>其功能把"<<"左边的运算数的各二进位全部左移若干位，由"<<"右边的数指定移动的位数，高位丢弃，低位补0。	|A << 2 结果为 240 ，二进制为 1111 0000|
|>>	|右移运算符">>"是双目运算符。右移n位就是除以2的n次方。<br>其功能是把">>"左边的运算数的各二进位全部右移若干位，">>"右边的数指定移动的位数。|A >> 2 结果为 15 ，二进制为 0000 1111|

```golang
package main

import (
	"fmt"
)

func main() {
	// a与b的数据类型必须相同，否则将会报错
	var a uint8 = 60
	var b uint8 = 13
	var c = a & b 		// c = 12
	var d = a << 2 		// d = 240
	var e = a << 3 		// e = 224

	fmt.Println(c, d, e)
}
```
### Go语言中的条件语句
* Go语言中的条件语句包括`if…………else`语句、`switch`语句以及`select`语句等。

#### Go语言switch语句
* switch 语句执行的过程从上至下，直到找到匹配项，匹配项后面不需要像C++语言一样再加break。示例如下：
```golang
package main

import (
	"fmt"
)

func main() {
	var a int = 15
	switch a {
	case 5:
		fmt.Println("a == 5")
		fmt.Println("end without break")
	case 10:
		fmt.Println("a == 10")
		fmt.Println("end without break")
	case 15:
		fmt.Println("a == 15")
		fmt.Println("end without break")
	case 20:
		fmt.Println("a == 10")
		fmt.Println("end without break")
	default:
		fmt.Println("no match")
	}
}
```
* switch 语句还可以被用于 type-switch 来判断某个 interface 变量中实际存储的变量类型。
```golang
package main

import (
	"fmt"
)

func main() {
	var a interface{}
	// a必须是一个接口类型的变量，而所有的case语句后面跟的类型必须实现了a的接口类型。
	switch a.(type) {	
	case nil:	  
        fmt.Printf(" x 的类型 :nil") 
	case int:
		fmt.Println("x 的类型是 int型")
	case float32:
		fmt.Println("x 的类型是 float型")
	default:
		fmt.Println("wrong")
	}
}
```
#### Go语言select语句
* select语句属于条件分支流程控制方法，不过它只能用于通道。它可以包含若干条case语句，并根据条件选择其中的一个执行。进一步说，select语句中的case关键字只能后跟用于通道的发送操作的表达式以及接收操作的表达式或语句。注意事项：
	* 每个case都必须是一个通信
	* 所有channel表达式都会被求值
	* 所有被发送的表达式都会被求值
	* 如果任意某个通信可以进行，它就执行；其他被忽略。
	* 如果有多个case都可以运行，Select会随机公平地选出一个执行。其他不会执行。
```golang
package main

import "fmt"

func main() {
   var c1, c2, c3 chan int
   var i1, i2 int
   select {
      case i1 = <-c1:
         fmt.Printf("received ", i1, " from c1\n")
      case c2 <- i2:
         fmt.Printf("sent ", i2, " to c2\n")
      case i3, ok := (<-c3):  // same as: i3, ok := <-c3
         if ok {
            fmt.Printf("received ", i3, " from c3\n")
         } else {
            fmt.Printf("c3 is closed\n")
         }
      default:
         fmt.Printf("no communication\n")
   }    
}
// 输出结果：no communication
```
### Go语言循环语句
#### for循环
* 和 C++ 语言的 for 一样：<br>
注意：
	* init： 一般为赋值表达式，给控制变量赋初值；
	* condition： 关系表达式或逻辑表达式，循环控制条件；
	* post： 一般为赋值表达式，给控制变量增量或减量。
```golang
for init; condition; post { }
```
* 和 C++ 语言的 while 一样：
```golang
for condition { }
```
* 和 C++ 的 for(;;) 一样：
```golang
for { }
```
示例如下：
```golang
package main

import "fmt"

func main() {
   var b int = 15
   var a int
   numbers := [6]int{1, 2, 3, 5} 
   /* for 循环 */
   for a := 0; a < 10; a++ {
      fmt.Printf("a 的值为: %d\n", a)
   }
   for a < b {
      a++
      fmt.Printf("a 的值为: %d\n", a)
   }
   for i,x:= range numbers {
      fmt.Printf("第 %d 位 x 的值 = %d\n", i,x)
   }   
}
```
### Go语言中的函数
* Go语言函数定义格式如下所示：
	* func：函数由 func 开始声明
	* function_name：函数名称，函数名和参数列表一起构成了函数签名。
	* parameter list]：参数列表，参数就像一个占位符，当函数被调用时，你可以将值传递给参数，这个值被称为实际参数。参数列表指定的是参数类型、	顺序、及参数个数。参数是可选的，也就是说函数也可以不包含参数。
	* return_types：返回类型，函数返回一列值。return_types 是该列值的数据类型。有些功能不需要返回值，这种情况下 return_types 不是必须的。
		函数体：函数定义的代码集合。
```golang
func function_name( [parameter list] ) ([return_types]) {
   函数体
}
```
* Go语言函数作为值：Go 语言可以很灵活的创建函数，并作为值使用。例如：
```golang
package main

import (
   "fmt"
   "math"
)

func main(){
   /* 声明函数变量 */
   getSquareRoot := func(x float64) float64 {
      return math.Sqrt(x)
   }

   /* 使用函数 */
   fmt.Println(getSquareRoot(9))
}
```
* 闭包：闭包是匿名函数，可以在动态编程中使用。
	* 匿名函数是一个"内联"语句或表达式。匿名函数的优越性在于可以直接使用函数内的变量，不必申明。
	* 以下实例中，我们创建了函数 getSequence() ，返回另外一个函数。该函数的目的是在闭包中递增 i 变量，代码如下：
```golang

```