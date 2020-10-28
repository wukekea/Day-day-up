### go语言中filepath包的使用

* func Abs(path string) (string, error)  // 返回绝对路径
```golang
package main

import (
	"fmt"
	"path/filepath"
)

func main() {
	slice := []string{"./a.txt", "./b.txt", "./c/d.txt", "/e/f.txt"}
	for _, path := range slice {
		str, err := filepath.Abs(path)
		fmt.Printf("path: %s; str: %s; err: %+v\n", path, str, err)
	}
}

/* windows系统下的结果
 * path: ./a.txt; str: D:\test\a.txt; err: <nil>
 * path: ./b.txt; str: D:\test\b.txt; err: <nil>
 * path: ./c/d.txt; str: D:\test\c\d.txt; err: <nil>
 * path: /e/f.txt; str: D:\e\f.txt; err: <nil>
 */
/* linux系统下的结果
 * path: ./a.txt; str: /root/Documents/a.txt; err: <nil>
 * path: ./b.txt; str: /root/Documents/b.txt; err: <nil>
 * path: ./c/d.txt; str: /root/Documents/c/d.txt; err: <nil>
 * path: /e/f.txt; str: /e/f.txt; err: <nil>
 */
```

* func `Base(path string)` string<br>
base返回path的最后一个元素。如果path为空，将返回"."。
```golang
package main

import (
"fmt"
"path/filepath"
)

func main() {
	slice := []string{"./c/d.txt", "", "./h/i/", "g.txt"}
	for _, path := range slice {
		baseName := filepath.Base(path)
		fmt.Printf("baseName: %s \n", baseName)
	}
}

/*
 * baseName: d.txt 
 * baseName: . 
 * baseName: i 
 * baseName: g.txt 
 */
```

* func `Clean(path string)` string
```golang
package main

import (
"fmt"
"path/filepath"
)

func main() {
	slice := []string{"./c/d.txt", "", "./h/i/", "g.txt", "../..//././//a/b/c.txt"}
	for _, path := range slice {
		clean := filepath.Clean(path)
		fmt.Printf("baseName: %s \n", clean)
	}
}

/* 
 * baseName: c\d.txt 
 * baseName: . 
 * baseName: h\i 
 * baseName: g.txt 
 * baseName: ..\..\a\b\c.txt 
 * /
```

* func `Dir(path string)` string<br>
dir返回所有除了最后一个元素的path
```golang
package main

import (
"fmt"
"path/filepath"
)

func main() {
	slice := []string{"./c/d.txt", "", "./h/i/", "g.txt", "../..//././//a/b/c.txt"}
	for _, path := range slice {
		dir := filepath.Dir(path)
		fmt.Printf("dir: %s \n", dir)
	}
}

/* 
 * dir: c 
 * dir: . 
 * dir: h\i 
 * dir: . 
 * dir: ..\..\a\b 
 */
```

* func `IsAbs(path string)` bool<br>
判断当前的path是否是绝对路径
```golang
package main

import (
"fmt"
"path/filepath"
)

func main() {
	slice := []string{"./c/d.txt", "", "./a.txt"}
	for _, path := range slice {
		ok := filepath.IsAbs(path)
		fmt.Printf("path: %s, isAbs: %+v\n", path, ok)
	}
}

/* 
 * path: ./c/d.txt, isAbs: false
 * path: , isAbs: false
 * path: ./a.txt, isAbs: false
 */
```

* func `Join(elem ...string)` string<br>
将elem连接成一个path
```golang
package main

import (
	"fmt"
	"path/filepath"
)

func main() {
	result := filepath.Join("a", "////b", "/c/d", "e.txt")
	fmt.Println(result)
}

/* 
 * a\b\c\d\e.txt
 */ 
```

* func `Walk(root string, walkFn WalkFunc)` error<br>
该方法主要用于递归遍历目录:<br>
walk方法会遍历root下的所有文件(包含root)并对每一个目录和文件都调用`walkFunc`方法。在访问文件和目录时发生的错误都会通过error参数传递给WalkFunc方法。文件是按照词法顺序进行遍历的，这个通常让输出更漂亮，但是也会导致处理非常大的目录时效率会降低。另外，Walk函数不会遍历符号链接。<br>
WalkFunc是一个方法类型，Walk函数在遍历文件或者目录时调用。调用时将参数传递给path，将Walk函数中的root作为前缀。将root + 文件名或者目录名作为path传递给WalkFunc函数。例如在"dir"目录下遍历到"a"文件，则path="dir/a"；Info是path所指向文件的文件信息。如果在遍历过程中出现了问题，传入参数err会描述这个问题。WalkFunc函数可以处理这个问题，Walk将不会再深入该目录。如果函数会返回一个错误，Walk函数会终止执行；只有一个例外，我们也通常用这个来跳过某些目录。当WalkFunc的返回值是filepaht.SkipDir时，Walk将会跳过这个目录，照常执行下一个文件。<br>
