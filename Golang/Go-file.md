```golang
package main

import (
	"fmt"
	"os"
)

func main() {
	fileName1 := "D:\\xng_project\\test.txt"
	fileInfo, err := os.Stat(fileName1)
	if err != nil {
		fmt.Println("err: ", err)
		return
	}
	fmt.Printf("%T\n", fileInfo)
	// 文件名
	fmt.Println(fileInfo.Name())
	// 文件大小。字节为单位
	fmt.Println(fileInfo.Size())
	// 是否是目录
	fmt.Println(fileInfo.IsDir())
	// 修改时间
	fmt.Println(fileInfo.ModTime())
	// 权限
	fmt.Println(fileInfo.Mode())
	fmt.Println(fileInfo.Sys())
}
```



```golang
package main

import (
	"fmt"
	"os"
	"path"
	"path/filepath"
)

func main() {
	fileName1 := "D:\\xng_project\\test.txt"
	fileName2 := "bb.txt"
	// IsAbs()函数用来判断其参数是否是绝对路径。是则返回true
	fmt.Println(filepath.IsAbs(fileName1))	// true
	fmt.Println(filepath.IsAbs(fileName2))	// false

	// Abs(path string) (string, error)
	// 返回当前参数路径的绝对路径
	fmt.Println(filepath.Abs(fileName1))	// D:\xng_project\test.txt <nil>
	fmt.Println(filepath.Abs(fileName2))	// D:\test\bb.txt <nil>

	fmt.Println("获取父目录：", path.Join("D:\\xng_project\\test.txt", "..", "helo"))	// 出现问题
	fmt.Println("获取父目录：", path.Join("", "d:///", "xng_project"))

	// 创建目录，不能创建多层目录
	// "D:\\xng_project\\helo"和"D:/xng_project/helo2"这两种写法都可以
	err := os.Mkdir("D:\\xng_project\\helo", os.ModePerm)
	err = os.Mkdir("D:/xng_project/helo2", os.ModePerm)

	// 创建多层文件夹
	err = os.MkdirAll("D:/xng_project/helo2/a/b/c/d", os.ModePerm)
	if err != nil {
		fmt.Println("err: ", err)
		return
	}

	// 创建文件:Create采用模式0666（任何人都可读写，不可执行）创建一个名为name的文件，如果文件已存在会截断它（为空文件）
	file1, err1 := os.Create("D:/xng_project/helo2/a/b/c/d/e.txt")
	if err1 != nil {
		fmt.Println("err: ", err1.Error())
		return
	}
	fmt.Println(file1)

	// 打开文件
	file2, err2 := os.Open("D:/xng_project/helo2/a/b/c/d/e.txt")
	if err2 != nil {
		fmt.Println("err: ", err2.Error())
		return
	}
	fmt.Println(file2)

	/*
		第一个参数：文件名称
		第二个参数：文件的打开方式
			const (
		// Exactly one of O_RDONLY, O_WRONLY, or O_RDWR must be specified.
			O_RDONLY int = syscall.O_RDONLY // open the file read-only.
			O_WRONLY int = syscall.O_WRONLY // open the file write-only.
			O_RDWR   int = syscall.O_RDWR   // open the file read-write.
			// The remaining values may be or'ed in to control behavior.
			O_APPEND int = syscall.O_APPEND // append data to the file when writing.
			O_CREATE int = syscall.O_CREAT  // create a new file if none exists.
			O_EXCL   int = syscall.O_EXCL   // used with O_CREATE, file must not exist.
			O_SYNC   int = syscall.O_SYNC   // open for synchronous I/O.
			O_TRUNC  int = syscall.O_TRUNC  // truncate regular writable file when opened.
		)
		第三个参数：文件的权限：文件不存在创建文件，需要指定权限
	*/
	file3, err3 := os.OpenFile("D:/xng_project/helo2/a/b/c/d/e.txt", os.O_RDONLY|os.O_WRONLY, os.ModePerm)
	if err3 != nil {
		fmt.Println("err: ", err3)
		return
	}
	fmt.Println(file3)

	// 关闭文件
	file3.Close()

	// 删除文件
	// 需要将该文件全部关闭才可以删除
	err4 := os.Remove("D:/xng_project/helo2/a/b/c/d/e.txt")
	if err4 != nil {
		fmt.Println("err4: ", err4)
		return
	}
}
```

```golang
package main
import (
	"fmt"
	"io"
	"os"
)
func main() {
	fileName := "D:\\xng_project\\test.txt"
	file, err := os.Open(fileName)
	if err != nil {
		fmt.Println("err: ", err)
		return
	}
	defer file.Close()

	bs := make([]byte, 4, 4)

	n := -1
	for{
		n,err = file.Read(bs)
		if n == 0 || err == io.EOF{
			fmt.Println("读取到了文件的末尾，结束读取操作。。")
			break
		}
		fmt.Println(string(bs[:n]))
	}
}
```

```golang
package main

import (
	"fmt"
	"log"
	"os"
)

func main() {
	/*
		写出数据：
	*/

	fileName := "D:\\xng_project\\test.txt"
	//step1：打开文件
	//step2：写出数据
	//step3：关闭文件
	//file,err := os.Open(fileName)
	file,err := os.OpenFile(fileName,os.O_CREATE|os.O_WRONLY|os.O_APPEND,os.ModePerm)
	if err != nil{
		fmt.Println(err)
		return
	}
	defer file.Close()

	//写出数据
	//bs :=[]byte{65,66,67,68,69,70}//A,B,C,D,E,F
	bs :=[] byte{97,98,99,100} //a,b,c,d
	//n,err := file.Write(bs)
	n,err := file.Write(bs[:2])
	fmt.Println(n)
	HandleErr(err)
	file.WriteString("\n")

	//直接写出字符串
	n,err = file.WriteString("HelloWorld")
	fmt.Println(n)
	HandleErr(err)

	file.WriteString("\n")
	n,err =file.Write([]byte("today"))
	fmt.Println(n)
	HandleErr(err)

}

func HandleErr(err error){
	if err != nil{
		log.Fatal(err)
	}
}

```

```golang

```
