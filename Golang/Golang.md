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
var e = [5]int{1, 2, 3, 4, 5}
var f = [3][4]int{  
 {0, 1, 2, 3} ,   /*  第一行索引为 0 */
 {4, 5, 6, 7} ,   /*  第二行索引为 1 */
 {8, 9, 10, 11}   /*  第三行索引为 2 */
}
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
* 闭包：闭包是匿名函数，可以在动态编程中使用。(一个函数的返回值是个函数)
	* 匿名函数是一个"内联"语句或表达式。匿名函数的优越性在于可以直接使用函数内的变量，不必申明。
	* 以下实例中，我们创建了函数 getSequence() ，返回另外一个函数。该函数的目的是在闭包中递增 i 变量，代码如下：
```golang
package main

import (
   "fmt"
)

func getSequence(i int) func() int {
	return func() int {
		i++
		return i
	}
}

func main(){
   NextNumber := getSequence(10)
   for i := 1; i < 5; i++ {
   		fmt.Println(NextNumber())
   }
}
```
* 方法：Go 语言中同时有函数和方法。一个方法就是一个包含了接受者的函数，接受者可以是命名类型或者结构体类型的一个值或者是一个指针。所有给定类型的方法属于该类型的方法集。在go语言中，我们可以明确的给类型T和*T声明一个方法，前提是T满足下面4点要求：
	* T必须是定义的类型
	* T的定义必须跟方法定义的在同一个包中。
	* T不能是指针类型
	* T不能是interface类型。注意：我们也可以给T和T的别名类型声明方法。效果等同于给T和T声明方法。
* 从上面列出的条件，我们将得出我们永远不能给符合下面条件的类型声明方法：
	* 内置基本类型，例如int, string,因为我们不能在标准库里声明方法；
	* interface类型，但是一个interface可以拥有方法。
	* 未定义类型，包括各种未命名的组合类型。如果一个未命名类型内嵌了一个其他的有方法的类型，编译器将明确的为这个匿名类型和其指针声明响应的方法。
* 方法声明类似于函数声明，但它有一个额外的参数声明部分。额外部分能包含并且仅能包含一个这个方法的接受者类型的参数。这个唯一的接受者参数被称为方法声明的接受者参数。接受者参数必须被()括住，并且声明在func和函数名称之间。
```golang
// Age and int are two distinct types. We can't declare
// methods for int and *int, but can for Age and *Age.
type Age int
func (age Age) LargerThan(a Age) bool {
    return age > a
}
func (age *Age) Increase() {
    *age++
}

// Receiver of custom defined function type.
type FilterFunc func(in int) bool
func (ff FilterFunc) Filte(in int) bool {
    return ff(in)
}

// Receiver of custom defined map type.
type StringSet map[string]struct{}
func (ss StringSet) Has(key string) bool {
    _, present := ss[key]
    return present
}
func (ss StringSet) Add(key string) {
    ss[key] = struct{}{}
}
func (ss StringSet) Remove(key string) {
    delete(ss, key)
}

// Receiver of custom defined struct type.
type Book struct {
    pages int
}
func (b Book) Pages() int {
    return b.pages
}
func (b *Book) SetPages(pages int) {
    b.pages = pages
}
```
### Go语言中的指针
* nil为Go语言中的空指针
```golang
// Go语言中的指针定义方法
var ptr *int
```
* Go语言指针数组
```golang
var ptr [3]*int
```
### Go语言结构体
* 结构体的定义如下：
```golang
// 结构体的定义
type struct_variable_type struct {
   member definition;
   member definition;
   ...
   member definition;
}

// 结构体的使用
variable_name := structure_variable_type {value1, value2...valuen}
```
```golang
package main

import (
   "fmt"
)

type Book struct {
	title string
	price int
	author string
}

func main(){
	var book Book
	book.title = "三体"
	book.price = 30
	book.author = "刘慈欣"

	fmt.Println(book)			// {三体 30 刘慈欣}
	fmt.Println(book.title)		// 三体

	var bookptr *Book = &book
	fmt.Println(bookptr)		// &{三体 30 刘慈欣}
	fmt.Println(*bookptr)		// {三体 30 刘慈欣}
	fmt.Println(bookptr.title)	// 三体
	fmt.Println(bookptr.title)	// 报错
	fmt.Println((*bookptr).title)	// 三体

	book1 := Book{"西游记", 20, "吴承恩"}
	fmt.Println(book1)
}
```
### Go语言切片
* Go语言切片是对数组的抽象
* Go 数组的长度不可改变，在特定场景中这样的集合就不太适用，Go中提供了一种灵活，功能强悍的内置类型切片("动态数组"),与数组相比切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大。
* 切片(slice)是对数组一个连续片段的引用(该数组我们称之为相关数组，通常是匿名的)，所以切片是一个引用类型(因此更类似于 C/C++ 中的数组类型)。这个片段可以是整个数组，或者是由起始和终止索引标识的一些项的子集。需要注意的是，`终止索引标识的项不包括在切片内`。切片提供了一个相关数组的动态窗口。
* 可以使用`len()`函数来获取切片或者数组的长度。
* 给定项的切片索引可能比相关数组的相同元素的索引小。和数组不同的是，切片的长度可以在运行时修改，最小为 0 最大为相关数组的长度:切片是一个`长度可变的数组`。
* 切片对象非常小，是因为它是只有3个字段的数据结构：`指向底层数组的指针`，`切片的长度`，`切片的容量`。这3个字段，就是Go语言操作底层数组的元数据，有了它们，我们就可以任意的操作切片了。注意，因为slice本身包含指针，所以slice的传递具有指针的性质。举个例子，将slice作为参数传递给函数时，如果在函数内修改了该slice的值，则函数外的slice也会相应的进行改变；而如果在函数内使用了append()函数进行修改，则该修改和位于append()后面的修改都不会对原来的slice造成影响。示例代码如下：
```golang
package main

import (
	"fmt"
)

func change(slice []int) {
    // 该修改会影响到main中的slice,导致main中的slice[0] = 111
	slice[0] = 111
	for i := 0; i < 5; i++ {
        使用了append函数
		slice = append(slice, i)
	}
	slice[0] = 222
}

func main() {
	var slice []int
	slice = append(slice, 9)
	change(slice)
	fmt.Println(slice)
}
```
* 定义切片的方法如下：
```golang
// 声明一个未指定大小的数组来定义切片：
var slice1 []int

// 或使用make()函数来创建切片:
var slice2 []int = make([]int, len)

// 也可以指定容量，其中capacity为可选参数。
make([]T, length, capacity)
```
* 切片初始化
```golang
// 直接初始化切片，[]表示是切片类型，{1,2,3}初始化值依次是1,2,3.其cap=len=3
s :=[] int {1,2,3 } 

// 初始化切片s,是数组arr的引用
s := arr[:] 

// 将arr中从下标startIndex到endIndex-1 下的元素创建为一个新的切片
s := arr[startIndex:endIndex] 

// 缺省endIndex时将表示一直到arr的最后一个元素
s := arr[startIndex:] 

// 缺省startIndex时将表示从arr的第一个元素开始
s := arr[:endIndex] 

// 通过切片s初始化切片s1
s1 := s[startIndex:endIndex] 

// 通过内置函数make()初始化切片s,[]int 标识为其元素类型为int的切片
s :=make([]int,len,cap) 
```
* len() 和 cap() 函数分别计算切片的当前长度和最大容量
```golang
package main

import "fmt"

func main() {
   /* 创建切片 */
   numbers := []int{0,1,2,3,4,5,6,7,8}   
   printSlice(numbers)

   /* 打印原始切片 */
   fmt.Println("numbers ==", numbers)

   /* 打印子切片从索引1(包含) 到索引4(不包含)*/
   fmt.Println("numbers[1:4] ==", numbers[1:4])

   /* 默认下限为 0*/
   fmt.Println("numbers[:3] ==", numbers[:3])

   /* 默认上限为 len(s)*/
   fmt.Println("numbers[4:] ==", numbers[4:])

   numbers1 := make([]int,0,5)
   printSlice(numbers1)

   /* 打印子切片从索引  0(包含) 到索引 2(不包含) */
   number2 := numbers[:2]
   printSlice(number2)

   /* 打印子切片从索引 2(包含) 到索引 5(不包含) */
   number3 := numbers[2:5]
   printSlice(number3)
}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```
* 如果想增加切片的容量，我们必须创建一个新的更大的切片并把原分片的内容都拷贝过来。<br>
下面的代码描述了从拷贝切片的 copy 方法和向切片追加新元素的 append 方法。
```golang
package main

import "fmt"

func main() {
   var numbers []int
   printSlice(numbers)

   /* 允许追加空切片 */
   numbers = append(numbers, 0)
   printSlice(numbers)

   /* 向切片添加一个元素 */
   numbers = append(numbers, 1)
   printSlice(numbers)

   /* 同时添加多个元素 */
   numbers = append(numbers, 2,3,4)
   printSlice(numbers)

   /* 创建切片 numbers1 是之前切片的两倍容量*/
   numbers1 := make([]int, len(numbers), (cap(numbers))*2)

   /* 拷贝 numbers 的内容到 numbers1 */
   copy(numbers1,numbers)
   printSlice(numbers1)   
}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```
* 清空切片可以使用一下几种方法：
```golang
package main

import (
	"fmt"
)

func main() {
	slice := []int{1,2,3,4,5}
	fmt.Printf("%v\n", slice)
	println(slice)
	fmt.Printf("======================\n")

	// 清空slice
	// 策略1
	println("策略一：")
	slice = slice[:0]
	fmt.Printf("%v\n", slice)
	println(slice)
	fmt.Println()

	fmt.Printf("再次访问原数组：\n")
	slice = slice[:5]
	fmt.Println(slice)
	println(slice)
	// 可以看出使用策略1并不会真正清空切片，只是将切片的长度变为1
	// 切片的地址、容量以及原来储存的值均不变
	fmt.Printf("======================\n")


	// 策略2
	println("策略2：")
	slice = slice[:0:0]
	fmt.Printf("%v\n", slice)
	println(slice)
	fmt.Println()

	// slice = slice[:5]		// 无法再次访问原数组
	slice = append(slice, 9)
	fmt.Println(slice)
	println(slice)
	println("==========================\n")

	// 策略二的slice在未追加元素前，虽然仍指向原数组，
	// 但不能再访问原数组的元素。追加元素后，slice的地址也会改变，
	// 也就是一个全新的数组。
}
```
* 注意，对切片的赋值是地址传递。如下代码：
```golang
package main

func main() {
	a := []int{2,3,4}
	// a与b地址相同
	b := a
	println(a)
	println(b)
	a[0]=111
	// 输出111
	println(b[0])
}
```

### Go语言范围range
* Go 语言中 range 关键字用于for循环中迭代数组(array)、切片(slice)、链表(channel)或集合(map)的元素。在数组和切片中它返回元素的索引值，在集合中返回 key-value 对的 key 值。
```golang
package main
import "fmt"
func main() {
    //这是我们使用range去求一个slice的和。使用数组跟这个很类似
    nums := []int{2, 3, 4}
    sum := 0
    for _, num := range nums {
        sum += num
    }
    fmt.Println("sum:", sum)
    //在数组上使用range将传入index和值两个变量。上面那个例子我们不需要使用该元素的序号，所以我们使用空白符"_"省略了。有时侯我们确实需要知道它的索引。
    for i, num := range nums {
        if num == 3 {
            fmt.Println("index:", i)
        }
    }
    //range也可以用在map的键值对上。
    kvs := map[string]string{"a": "apple", "b": "banana"}
    for k, v := range kvs {
        fmt.Printf("%s -> %s\n", k, v)
    }
    //range也可以用来枚举Unicode字符串。第一个参数是字符的索引，第二个是字符（Unicode的值）本身。
    for i, c := range "go" {
        fmt.Println(i, c)
    }
}
```
### Go语言的Map（集合）
* Map 是一种集合，所以我们可以像迭代数组和切片那样迭代它。不过，Map 是无序的，我们无法决定它的返回顺序，这是因为 Map 是使用 hash 表来实现的。
* 定义Map方法如下：
```golang
/* 声明变量，默认 map 是 nil */
var tmp map[string]string

/* 使用 make 函数 */
map_variable = make(map[int]string)
```
```golang
package main

import "fmt"

func main() {
   var countryCapitalMap map[string]string
   /* 创建集合 */
   countryCapitalMap = make(map[string]string)
   
   /* map 插入 key-value 对，各个国家对应的首都 */
   countryCapitalMap["France"] = "Paris"
   countryCapitalMap["Italy"] = "Rome"
   countryCapitalMap["Japan"] = "Tokyo"
   countryCapitalMap["India"] = "New Delhi"
   
   /* 使用 key 输出 map 值 */
   for country := range countryCapitalMap {
      fmt.Println("Capital of",country,"is",countryCapitalMap[country])
   }
   
   /* 查看元素在集合中是否存在 */
   captial, ok := countryCapitalMap["United States"]
   /* 如果 ok 是 true, 则存在，否则不存在 */
   if(ok){
      fmt.Println("Capital of United States is", captial)  
   }else {
      fmt.Println("Capital of United States is not present") 
   }
}
```
* delete() 函数用于删除集合的元素, 参数为 map 和其对应的 key。实例如下：
```golang
package main

import "fmt"

func main() {   
   /* 创建 map */
   countryCapitalMap := map[string] string {"France":"Paris","Italy":"Rome","Japan":"Tokyo","India":"New Delhi"}
   
   fmt.Println("原始 map")   
   
   /* 打印 map */
   for country := range countryCapitalMap {
      fmt.Println("Capital of",country,"is",countryCapitalMap[country])
   }
   
   /* 删除元素 */
   delete(countryCapitalMap,"France");
   fmt.Println("Entry for France is deleted")  
   
   fmt.Println("删除元素后 map")   
   
   /* 打印 map */
   for country := range countryCapitalMap {
      fmt.Println("Capital of",country,"is",countryCapitalMap[country])
   }
}
```
### Go语言类型转换
```golang
package main

import "fmt"

func main() {
   var sum int = 17
   var count int = 5
   var mean float32
   
   mean = float32(sum)/float32(count)
   fmt.Printf("mean 的值为: %f\n",mean)
}
```
### Go语言接口
```golang
/* 定义接口 */
type interface_name interface {
   method_name1 [return_type]
   method_name2 [return_type]
   method_name3 [return_type]
   ...
   method_namen [return_type]
}

/* 定义结构体 */
type struct_name struct {
   /* variables */
}

/* 实现接口方法 */
func (struct_name_variable struct_name) method_name1() [return_type] {
   /* 方法实现 */
}
...
func (struct_name_variable struct_name) method_namen() [return_type] {
   /* 方法实现*/
}
```
### Go语言使用mongodb数据库
* ORM：对象关系映射（Object Relational Mapping，简称ORM）是通过使用描述对象和数据库之间映射的元数据，将面向对象语言程序中的对象自动持久化到关系数据库中。本质上就是将数据从一种形式转换到另外一种形式。
* gorm是go语言中实现数据库访问的ORM（对象关系映射）库。使用这个库，我们可以利用面向对象的方法，更加方便的对数据库中的数据进行CRUD(增删改查)。
##### gorm的基本使用
* 下载依赖
```bash
// 通过该语句可以查看GOPATH路径（环境变量设置修改），该路径即go get下载的依赖的存储路径
go env

// 下载的依赖位于：D:\Language\Go1.13.12\pkg\mod\github.com
go get github.com/jinzhu/gorm
// 下载gopkg.in/mgo.v2
go get gopkg.in/mgo.v2
// 引入gopkg.in/mgo.v2
import "gopkg.in/mgo.v2"
```
* 使用方法
```golang
package main

import (
	"fmt"
	"gopkg.in/mgo.v2"
	"gopkg.in/mgo.v2/bson"
	"log"
)
type Student struct {
	Name			string 	`bson: "name"`
	Age 			int		`bson: "age"`
	Status 			int		`bson: "status"`
	StudentNumber	string	`bson: "student_number"`
}
func main() {
	mongo, err := mgo.Dial("127.0.0.1") // 建立连接
	defer mongo.Close()
	if err != nil {
		log.Fatal("get one error: %v", err)
	}
	// 选择数据库和集合
	client := mongo.DB("t").C("used")
	data := Student {
		Name:	"mongo",
		Age:	18,
		Status:	1,
		StudentNumber: "1612899",
	}
	//插入数据
	cErr := client.Insert(&data)
	if cErr != nil {
		log.Fatal("insert error:%v", cErr)
	}

	// 查找数据
	user := Student{}
	cErr2 := client.Find(bson.M{"studentnumber":"1612899"}).One(&user)
	if cErr2 != nil {
		log.Fatal("find error:%v", cErr2.Error())
	}

	// 查找所有数据 限制15条
	iter := client.Find(bson.M{"age":18}).Sort("_id").Limit(15).Iter()
	stu := Student{}
	var stus []Student
	for iter.Next(&stu) {
		stus = append(stus, stu)
	}

	// 更新数据
	// 该函数将只更新一条数据
	cErr3 := client.Update(bson.M{"status":1}, bson.M{"$set":bson.M{"age":99}})
	if cErr3 != nil {
		log.Fatal("updata error:%v", cErr3)
	}

	// 删除数据
	// 只会删除一条数据
	client.Remove(bson.M{"status":1})

	fmt.Println(stus)
}
```
### 按位与运算符的妙用
* 对于一个数N，如果N = 2^n（N可以为负数），那么有N&(N-1) = 0；<br>
因此，对任意一个N = 2^a1+2^a2+……+2^ak，N&(N-1) = N - 2^ak
### 排序算法
https://blog.csdn.net/sinat_26058371/article/details/86765055

* 对于一些常用的类型（整数、浮点数、字符串），sort包直接提供了可排序的集合类型，和排序函数，
```golang
type IntSlice []int
type Float64Slice []float64
type StringSlice []string

func Ints(a []int)           //整数排序
func Float64s(a []float64)   //浮点数排序
func Strings(a []string)     //字符串排序

// 使用方法如下：
var a sort.IntSlice = []int{3, 2, 4, 1}
sort.Sort(a)
fmt.Println(a)      //output: [1 2 3 4]
// ------------------------------------
// 或者：
a := []int{3, 2, 4, 1}
sort.Ints(a) 
fmt.Println(a)      //output: [1 2 3 4]
```

### go语言生成随机数的方法
* golang中产生随机数主要有两个包，分别是“math/rand”和“crypto/rand”。<br>
“math/rand”的rand包实现了伪随机数生成器。<br>
"crypto/rand"的rand包实现了用于加解密的更安全的随机数生成器。<br>
```golang
// 生成伪随机数
package main
import (
	"fmt"
	"math/rand"
	"time"
)
func main() {
	a := time.Now()
	b := a.UnixNano()
	fmt.Println(a)
	fmt.Println(b)
	// 随机种子
	rand.Seed(b)
	// 生成20个【0，100）范围的伪随机数
	for i := 0; i < 20; i++ {
		result := rand.Intn(100)
		fmt.Println(result)
	}
}
```
```golang
package main
import (
	"fmt"
	"math/big"
	"crypto/rand"
)
func main() {
	// 生成20个【0，100）范围的真随机数
	for i := 0; i < 20; i++ {
		result, _ := rand.Int(rand.Reader, big.NewInt(100))
		fmt.Println(result)
	}
}
``` 
### go语言从控制台输入语句的方法
* 使用fmt包下的Scan()函数或者Scanf()函数。示例如下：
```golang
package main
import (
	"fmt"
)
func main() {
	n, m := 0, 0
	// 这两种方法读取的如果是字符串，则只会读取空格前面的字符串。
	fmt.Scan(&n)
	fmt.Scanf("%d", &m)
}
```
* 使用bufio和os包
```golang
package main
import (
	"bufio"
	"os"
    "fmt"
)
func main() {
	in := bufio.NewReader(os.Stdin)
	// 得到的str是一个切片，里面的值是rune类型的ascii码。需要使用string()将其转换为字符串。
	str, _, _ := in.ReadLine()
	fmt.Println(string(str))
}
```
### Go工具的使用
* go默认已经有了gofmt工具，但是强烈建议使用goimport工具，这个在gofmt的基础上增加了自动删除和引入包.<br>
如果使用GoLand，则可采用其默认的格式化
```bash
go get golang.org/x/tools/cmd/goimports
```

vet工具可以帮我们静态分析我们的源码存在的各种问题，例如多余的代码，提前return的逻辑，struct的tag是否符合标准等。
```bash
go get golang.org/x/tools/cmd/vet
```
使用如下：
```bash
go vet .
```

* 大陆不能访问golang.org，所以使用 go get golang.org/x/xxx 包的时候都会失败<br>
可以手动下载的方式解决，golang.org/x所有包都在GitHub有镜像：https://github.com/golang/tools


* GO学习中有用的网址
```
// 使用go get下载某些依赖包时由于墙的原因下载不下来，需要网络代理
https://goproxy.io/zh/				GO模块的全球代理

```

* gin框架配置swagger自动生成文档
	* swagger使用手册参见：https://swaggo.github.io/swaggo.io/declarative_comments_format/general_api_info.html
	* 下载安装swagger（此处可能需要配置网络代理，并在power shell下执行下列命令）
		```
		go get -u github.com/swaggo/swag/cmd/swag
		go get -u github.com/swaggo/gin-swagger
		go get -u github.com/swaggo/files
		go get -u github.com/alecthomas/template
		```
	* gin中router代码需要修改的地方：
		```
		import (
			// 相应的包的路径
			_ "modulename/docs"
			"github.com/gin-gonic/gin"
			"github.com/swaggo/gin-swagger"
			"github.com/swaggo/gin-swagger/swaggerFiles"
		)

		func InitHttpServer() {
			var router *gin.Engine
			router = gin.Default()
			router.Static("/html", "./public")
	
			// 重要。这条语句相当与添加一个接口，以便浏览器访问
			router.GET("/swagger/*any", ginSwagger.WrapHandler(swaggerFiles.Handler))

			router.Run(":" + _const.HTTP_SERVER_PORT)
		}
		```
	* api接口的注释：<br>
	一定要注意在方法上添加的注释，如果不规范，swagger不会识别<br>
	标准注释如：https://swaggo.github.io/swaggo.io/declarative_comments_format/general_api_info.html
		```
		// @Summary 根据条件查询实例
		// @Description 根据条件查询实例
		// @Accept  json
		// @Produce  json
		// @Param   pageNum     path    int     true        "pageNum"
		// @Param expName body string false "expName"
		// @Param expType body string false "expType"
		// @Param expTrade body string false "expTrade"
		// @Param expScene body string false "expScene"
		// @Param expRemark body string false "expRemark"
		// @Param expDeg body int false "expDeg"
		// @Param expCreateUser body string false "expCreateUser"
		// @Success 200 {string} json "{"errcode":"200","data":"[{"expId":"111","expName":"TensorFlow","expType":"TensorFlow","expTrade":"制造业","expScene":"零售","expRemark":"零售零售零售零售","expDeg":"2","expCreateUser":"tfg"}]","msg":""}"
		// @Failure 400 {string} json "{"errcode":"400","data":"","msg":"error......"}"
		// @Router /api/v1/exp/list/{pageNum} [post]
		func ListExp(c *gin.Context){
			pageNum,err := strconv.Atoi(c.Param("pageNum"))
			if err != nil {
				rltStr := util.Rltjson( _const.SERVICE_FAIL_CODE,err.Error(),"")
				c.String(http.StatusBadRequest, rltStr)
				return
			}
		}
		```
	* main.go的注释
		```golang
		// @title App后端接口
		// @version 1.0
		// @description App后端
		// @host 127.0.0.1:8989			// 需要跟api的访问接口相同
		// @BasePath /
		```
	* 接着在命令行执行swag init
		```
		PS D:\Job\project\app-service> swag init
		2020/07/30 21:40:04 Generate swagger docs....
		2020/07/30 21:40:04 Generate general API Info, search dir:./
		2020/07/30 21:40:04 Generating appinterface.DeviceAliasParam
		2020/07/30 21:40:04 create docs.go at docs/docs.go
		2020/07/30 21:40:04 create swagger.json at docs/swagger.json
		2020/07/30 21:40:04 create swagger.yaml at docs/swagger.yaml
		```
	* 然后就就可以在浏览器中访问了：<br>
	http://localhost:8989/swagger/index.html<br>
	其中的端口号同其它接口的端口号。

* Go语言中常见的类型占用的内存大小
```golang
var a int8			// 8/8=1字节
var b int 32		// 32/8=4字节
var c int			// 比较特殊，，int占位多少取决于CPU。在32位CPU中占4字节，64位占8字节
// go语言的string本质上是一个结构体
// type StringHeader struct {
//         Data uintptr
//         Len  int
// }
// uintptr和int在64位cpu下都占8字节，一共16字节。
var d string		// 16字节
var e bool 			// 1字节
var f float32		// 32/8=4字节
var g float64		// 64/8=8字节
```
# 方法，select语句