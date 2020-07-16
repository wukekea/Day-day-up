### Go语言字符串处理函数
* 使用字符串处理函数时需要导入`strings`包
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