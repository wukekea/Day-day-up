### Go语言字符串处理函数
* 使用字符串处理函数时需要导入`strings`包
* Go语言中的string不能直接通过下标访问某个字符。比如：
```golang
// str[0]结果是'h'的ascii码
// 可以使用string（）强制转换
str := "helo"
```
* `Contains`:
```
func Contains(s, str) bool
功能：字符串s中是否包含str,返回bool值
```
```golang
package main
import ("fmt"; "strings")
func main() {
    var input string
    fmt.Scan(&input)
    
    b := strings.Contains(input, "PM")
    fmt.Println(b)
}
// 输入：08:03:45PM
// 输出：true
```
* `Join`
```
func Join(slice, str) string
功能：将字符串切片slice使用str连接起来并返回一个string
```
```golang
package main
import ("fmt"; "strings")
func main() {
    str := []string{"first", "second", "third"}
    s := strings.Join(str, "**")    // s = first**second**third
    fmt.Println(s)
}
```
* `Index`
```
func Index(str1, str2) int
功能：查找str2在str1中第一次出现的位置。若不存在则返回-1
```
```golang
package main
import ("fmt"; "strings")
func main() {
    str1 := "hello world"
    str2 := "ll"
    i := strings.Index(str1, str2)  // i = 2
    fmt.Println(i)
}
```
* `Repeat`
```
func Repeat(str, num) string
功能：将字符串str重复num次
```
```golang
package main
import ("fmt"; "strings")
func main() {
    str := "ae"
    s := strings.Repeat(str, 3)     // s = aeaeae
    fmt.Println(s)
}
```
* `Replace`
```
func Replace(str, str1, str2, num) string
功能：将字符串str中的str1替换为str2，替换次数为num次。若num<0或大于str中str1的数量则全部替换
```
```golang
package main
import ("fmt"; "strings")
func main() {
    str := "hello world"
    s := strings.Replace(str, "l", "2", 1)
    fmt.Println(s)
}
```
* `Split`
```
func Split(str1, str2) 切片
功能：将字符串str1按照str2进行切割，结果为切片
```
```golang
package main
import ("fmt"; "strings")
func main() {
    str1 := "111-222-333"
    slice := strings.Split(str1, "-")   // slice = [111 222 333]
    fmt.Println(slice)
}
```
* `Trim`
```
func Trim(str1, str2) string
功能：去掉str1头尾中的str2部分，中间部分不会去除
```
```golang
package main
import ("fmt"; "strings")
func main() {
    str1 := "    111  -222-333 "
    s := strings.Trim(str1, " ")    // s = 111  -222-333
    fmt.Println(s)
}
```
* `Fields`
```
func Fields(str) 切片
功能：去除str中的空格并返回切片
```
```golang
package main
import ("fmt"; "strings")
func main() {
    str1 := "    111  222  333 "
    slice := strings.Fields(str1)       // slice =  [111 222 333]
    fmt.Println(slice)
}
```
### int型整数与string字符串的相互转换
* int整数转字符串
```golang
package main
import (
   "fmt"
   "strconv"
)
func main(){
    var num int = 8
    
    // 使用fmt.Sprintf() 格式化输出函数
    // func Sprintf(format string, a ...interface{}) string
    // 因为Sprintf（）接收的参数中有 interface，需要进行反射等操作，所以性能相比Itoa要差
	s1 := fmt.Sprintf("%d", num)
    fmt.Println(s1)
    
    // 使用strconv.Itoa()函数，需要引入strconv包
    // func Itoa(i int) string
    s2 := strconv.Itoa(num)
    fmt.Println(s2)
    
    // func FormatUint(i uint64, base int) string     // 无符号整型转字符串，base为进制
    // func FormatInt(i int64, base int) string       // 有符号整型转字符串
    // func Itoa(i int) string
    // 其中，Itoa 内部直接调用 FormatInt(i, 10) 实现的。base 参数可以取 2~36（0-9，a-z）
}
```
* 字符串转int整数
```golang
package main
import (
   "fmt"
   "strconv"
)
func main(){
    var str string = "12"
    
    // func Atoi(s string) (int, error)
	num, _ := strconv.Atoi(str)
    fmt.Println(num)
    
    // func ParseInt(s string, base int, bitSize int) (i int64, err error)。其功能较Atoi更为强大
    // 参数1 数字的字符串形式
    // 参数2 数字字符串的进制 比如二进制 八进制 十进制 十六进制
    // 参数3 返回结果的bit大小 也就是int8 int16 int32 int64
    // 下例中将str看作是3进制的string，并将其转换为10进制的num2
    num2, _ := strconv.ParseInt(str, 3, 8)
	fmt.Println(num2)
}
```

### 修改字符串
* 在 Go 语言中，字符串的内容是不能修改的，也就是说，你不能用 s[0] 这种方式修改字符串中的 UTF-8 编码，如果你一定要修改，那么你可以将字符串的内容复制到一个可写的缓冲区中，然后再进行修改。这样的缓冲区一般是 []byte 或 []rune。如果要对字符串中的字节进行修改，则转换为 []byte 格式，如果要对字符串中的字符进行修改，则转换为 []rune 格式，转换过程会自动复制数据。<br>
rune是Go语言中一种特殊的数据类型,它是int32的别名,几乎在所有方面等同于int32,用于区分字符值和整数值

```golang
package main
import (
    "fmt"
)
func main() {
s := “Hello 世界！”
b := []byte(s) // 转换为 []byte，自动复制数据
b[5] = ‘,’ // 修改 []byte
fmt.Printf("%s\n", s) // s 不能被修改，内容保持不变
fmt.Printf("%s\n", b) // 修改后的数据
}
修改字符串中的字符（用 []rune）：
func main() {
s := “Hello 世界！”
r := []rune(s) // 转换为 []rune，自动复制数据
r[6] = ‘中’ // 修改 []rune
r[7] = ‘国’ // 修改 []rune
fmt.Println(s) // s 不能被修改，内容保持不变
fmt.Println(string(r)) // 转换为字符串，又一次复制数据
}
```
* 注意：对于一个字符：a := '0',这里的a是rune(int32)类型的；对于一个字符串：str := "0"，这里的str[0]是byte(uint8)类型的

```golang
package main

import （
    "fmt"
    "unicode/utf8"
)

func main() {
    var str = "hello 世界"
    // len(str) = 12.
    // 从字符串字面值看len(str)的结果应该是8,但在Golang中string类型的底层是通过byte数组实现的,
    // 在unicode编码中,中文字符占两个字节,而在utf-8编码中,中文字符占三个字节而Golang的默认编码正是utf-8.
    fmt.Println("len(str):", len(str))

    // 如果想要获得真实的字符串长度而不是其所占用字节数,有两种方法实现
    // 方法一, 使用unicode/utf8包中的RuneCountInString方法
    fmt.Println("RuneCountInString:", utf8.RuneCountInString(str))

    // 方法二，将字符串转换为rune类型的数组再计算长度
    fmt.Println("rune:", len([]rune(str)))

}
```

```golang
package main

import (
	"fmt"
)

func main() {
	str := "hello world, 科科"
	fmt.Println(str)

	for i := 0; i < len(str); i++ {
		fmt.Printf("%c", str[i])
	}
	fmt.Println()

	for _, i := range str {
		fmt.Printf("%c", i)
	}
}

// 输出结果为：
// hello world, 科科
// hello world, ç§ç§
// hello world, 科科
// 由此可知，str[i]是byte型，而使用range得到的是int32型
```