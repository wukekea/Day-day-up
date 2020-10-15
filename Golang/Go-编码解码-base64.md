# Go的Json解析：Marshal与Unmarshal
* Json(Javascript Object Nanotation)是一种数据交换格式，常用于前后端数据传输。任意一端将数据转换成json 字符串，另一端再将该字符串解析成相应的数据结构，如string类型，strcut对象等。
* go语言本身为我们提供了json的工具包”encoding/json”。
* func Marshal(v interface{}) ([]byte, error)：将数据编码成json字符串。
* func Unmarshal(data []byte, v interface{}) error：将json字符串解码到相应的数据结构
```golang
package main

import (
	"encoding/json"
	"fmt"
)

type Lover struct {
	Name 	string
	Age 	int
}
type Student struct {
	Age 		int				`json:"age"`
	Name 		string			`json:"name"`
	Hobbies 	[]string
	HisLover 	*Lover
	Score 		interface{}
	test 		int
}

func main() {
    student := Student{
		Age:      20,
		Name:     "wkk",
		Hobbies:  []string{"singing", "dancing", "studing"},
		HisLover: &Lover{Name: "kkk", Age: 20},
		Score:    map[string]int{"english":100, "math":100,"chinese":100},
		test:     10086,
	}

	jsonStudent, err := json.Marshal(student)
	if err != nil {
		fmt.Println("an error get, err: ", err.Error())
		return
	}

	// jsonStudent是byte类型，需要强制转换为string
	// {"age":20,"name":"wkk","Hobbies":["singing","dancing","studing"],"HisLover":{"Name":"kkk","Age":20},"Score":{"chinese":100,"english":100,"math":100}}
    fmt.Println(string(jsonStudent))
    
    // 1.Unmarshal的第一个参数是json字符串，第二个参数是接受json解析的数据结构。
	//第二个参数必须是指针，否则无法接收解析的数据，如studentBack仍为空对象Student{}
	var studentBack Student
	err1 := json.Unmarshal(jsonStudent, &studentBack)
	if err1 != nil {
		fmt.Println("unmarshal error. err: ", err1.Error())
		return
	}
	fmt.Printf("studentBack %+v\n", studentBack)
}
```
* 只要是可导出成员（变量首字母大写），都可以转成json。因成员变量test是不可导出的，故无法转成json。
* 如果变量打上了json标签，如Name旁边的 `json:"name"` ，那么转化成的json key就用该标签“name”，否则取变量名作为key，如“Hobbies”，“Score”。
* bool类型也是可以直接转换为json的value值。Channel， complex 以及函数不能被编码json字符串。当然，循环的数据结构也不行，它会导致marshal陷入死循环。
* 指针变量，编码时自动转换为它所指向的值，如Lover变量。 <br>
（当然，不传指针，Student struct的成员Lover如果换成Lover struct类型，效果也是一模一样的。只不过指针更快，且能节省内存空间。）
