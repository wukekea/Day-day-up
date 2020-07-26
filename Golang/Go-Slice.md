* slice本身是一个结构体，而不是一个指针，其底层实现是指向数组的指针。
```golang
func makeslice64(et *_type, len64, cap64 int64) unsafe.Pointer
type slice struct {
	array unsafe.Pointer
	len   int
	cap   int
}

```

* golang 的slice是一个指向底层的数组的指针结构体。 这个结构体有`三个属性`<br>
1.指向数组指针，<br>
2.len：slice中元素的数量。<br>
3.cap：slice占用内存数量。<br>

* golang 圣经中的一句话：<br>
多个slice之间可以共享底层的数据，并且引用的数组部分区间可能重叠。<br>
例如：使用`:=`运算符对slice进行复制，产生的slice将会共享底层数据。即slice对应位置的内存地址相同。举例如下：
```golang
package main
import (
    "fmt"
)
func main() {
    // a与b的地址相同
    // a[0], a[1], ……, a[5]与b[0], b[1], ……, b[5]的地址分别相同
    // c[0], c[1] 与a[1], a[2]的地址分别相同
    // 因此修改a共享内存部分的数据将会导致b、c做出相应的修改。
    // len(c)=2但是cap(c)=5。即c与a、b共享下标为1,2,3,4,5的数的内存空间
    a := []int{1,2,3,4,5,6}
	b := a
    c := a[1:3]             // c = {2, 3}
    
    // 因为c[2]的地址与a[3]、b[3]相同，所以当往c中添加一个数100时，a[3], b[3]也相应的改为100
    c = append(c, 100)      //（1）

    // 因为a中的len与cap相等，因此该操作将会导致：
    // 新开辟出一片内存地址，其cap将翻倍，len+1，并将原来的a中的值复制过来，之后在a后面加上100。
    // 此时a[0], a[1], ……, a[5]中的内存地址将不再与b[0], b[1], ……, b[5]相同
    a = append(a, 100)      // a = {1,2,3,4,5,6,100}

    // d的数值、len、cap均与a相同
    d := a
    // 这种方式得到的e与d没有区别。即，cap(e)==cap(d)==cap(a)
    e := a[0:len(a)]

    // 此时a的len<cap，因此添加操作在原来数据的地址不变的情况下，新开辟一个内存空间用来储存100
    // 但是d仍然是原来的a，即d并没有因为a增加了100而同时也增加了100
    a = append(a, 100)

    // 再往d后添加一个元素后导致的结果将和（1）相同。
    // 该操作将会把a[6]从原来的100修改为1。此时a[0], a[1], ……, a[6]与d[0], d[1], ……, d[6]的地址分别相同
    d = append(d, 1)
}
```

* 当使用slice作为函数的参数时踩过的坑<br>
slice作为参数传递时是值传递。因为slice内部实现是指向数组的指针的，所以函数内修改slice的值，相应的修改也会体现在函数外。

```golang
package main
import (
    "fmt"
)
func change(slice []int) {
	// main函数中的slice【0】将会变为100
	slice[0] = 100
	println(slice)

	// 此时需要分情况讨论
	// 1，slice中的cap==len时，使用append将会返回一个全新的切片，该切片与main中的slice地址不同
	//    在此之后对该切片的修改不会影响到main中的切片。即：执行完slice[0]=200后，main中的slice[0]=100
	// 2, slice中的len<cap时，此时append会在原切片的基础上开辟一个空间储存5
	//    在此之后对该切片的修改也会影响到main中的切片。即：执行完slice[0]=200后，main中的slice[0]=200
	//	  但是append()自己的操作不会影响到main中的slice。
	slice = append(slice, 5)
	println(slice)
	slice[0] = 200

}
func main() {
	slice := make([]int, 5, 10)
	fmt.Println(slice)

	change(slice)
	fmt.Println(slice)
}
```
