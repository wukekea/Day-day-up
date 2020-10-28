### Go中自定义结构体的排序方法
* sort 包中有一个 sort.Interface 接口，该接口有三个方法 Len() 、 Less(i,j) 和 Swap(i,j) 。 通用排序函数 sort.Sort 可以排序任何实现了 sort.Inferface 接口的对象(变量)。对于 [] int 、[] float64 和 [] string 除了使用特殊指定的函数外，还可以使用改装过的类型 IntSclice 、 Float64Slice 和 StringSlice ， 然后直接调用它们对应的 Sort() 方法；因为这三种类型也实现了 sort.Interface 接口， 所以可以通过 sort.Reverse 来转换这三种类型的 Interface.Less 方法来实现逆向排序， 这就是前面最后一个排序的使用。

* 示例如下，对自定义的结构体student按照年龄进行排序（方法一）
    ```golang
    package main
    import (
        "fmt"
        "os"
        "sort"
    )
    // 结构体进行排序(按照age)
    type student struct {
        Age   int
        Name  string
        Score int
    }
    type students []student
    // 实现了sort.Interface接口中的三个方法
    func (s students) Len() int {
        return len(s)
    }
    func (s students) Swap(i int, j int) {
        s[i], s[j]=s[j], s[i]
    }
    func (s students) Less(i int, j int) bool {
        return s[i].Age < s[j].Age
    }
    func (s students) print() {
        for _, j := range s {
            fmt.Printf("age: %d, name: %s, score: %d\n", j.Age, j.Name, j.Score)
        }
    }
    func main() {
        s1 := students{
            {10, "hello", 100},
            {20, "wkk", 100},
            {30, "yes", 100},
            {15, "yes", 100},
            {25, "yes", 100},
        }
        sort.Sort(s1)
        s1.print()
    }
    ```

* 方法二<br>
注：使用方法一进行排序有个缺点，根据Age排序需要重新定义三个方法 Len 、 Less 和 Swap，如果根据其它字段（Name或score）排序，又需要重新定义这三种方法。而事实上，根据不同的方法排序，只有Less方法需要改变。因此可以将 Less 抽象出来， 每种排序的 Less 让其变成动态的。<br>
事实上，go 的排序未必就是针对的一个数组或是 slice， 而可以是一个对象中的数组或是 slice 。例子中的排序则是针对对象StudentWrapper中的数组Students。<br>

    ```golang
    package main

    import (
        "fmt"
        "sort"
    )

    // 结构体进行排序(按照age)
    type student struct {
        Age   int
        Name  string
        Score int
    }
    type StudentWrapper struct {
        Students []student
        By func(s1 *student, s2 *student) bool
    }
    func (s *student) print() {
        if s == nil {
            fmt.Println("s is nil.")
            return
        }
        fmt.Printf("age: %d, name: %s, score: %d\n", s.Age, s.Name, s.Score)
    }
    type students []student
    func (s students) print() {
        for _, j := range s {
            fmt.Printf("age: %d, name: %s, score: %d\n", j.Age, j.Name, j.Score)
        }
    }
    func (s StudentWrapper) Len() int {
        return len(s.Students)
    }
    func (s StudentWrapper) Swap(i int, j int) {
        s.Students[i], s.Students[j]=s.Students[j], s.Students[i]
    }
    func (s StudentWrapper) Less(i int, j int) bool {
        return s.By(&s.Students[i], &s.Students[j])
    }
    func main() {
        s := &student{
            Age:   10,
            Name:  "hello",
            Score: 100,
        }
        s.print()

        s1 := []student{
            {10, "hello", 80},
            {20, "wkk", 70},
            {30, "yes", 90},
            {15, "yes", 60},
            {25, "yes", 95},
        }
        // 针对Age进行排序
        sort.Sort(StudentWrapper{
            Students: s1,
            By:       func(p, q *student) bool {
                return p.Age<q.Age
            },
        })
        students(s1).print()
        fmt.Println("------------------------------")
        // 针对Score进行排序
        sort.Sort(StudentWrapper{
            Students: s1,
            By:       func(p, q *student) bool {
                return p.Score < q.Score
            },
        })
        students(s1).print()
        fmt.Println("------------------------------")
    }
    /* 输出结果如下
    * age: 10, name: hello, score: 100
    * age: 10, name: hello, score: 80
    * age: 15, name: yes, score: 60
    * age: 20, name: wkk, score: 70
    * age: 25, name: yes, score: 95
    * age: 30, name: yes, score: 90
    * ------------------------------
    * age: 15, name: yes, score: 60
    * age: 20, name: wkk, score: 70
    * age: 10, name: hello, score: 80
    * age: 30, name: yes, score: 90
    * age: 25, name: yes, score: 95
    * ------------------------------
    */
    ```