# 设计模式中的单例模式
* 单例模式是最简单的设计模式之一，这种类型的设计模式属于创建型模式。
* 它提供了一种创建对象的最佳方式。<br>
这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。
* 单例类只能有一个实例。
* 单例类必须自己创建自己的唯一实例。
* 单例类必须给所有其他对象提供这一实例。

* 主要解决：一个全局使用的类频繁地创建与销毁。
* 何时使用：当您想控制实例数目，节省系统资源的时候。
* 应用实例：<br>
1，Windows 是多进程多线程的，在操作一个文件的时候，就不可避免地出现多个进程或线程同时操作一个文件的现象，所以所有文件的处理必须通过唯一的实例来进行。<br>
2，一些设备管理器常常设计为单例模式，比如一个电脑有两台打印机，在输出的时候就要处理不能两台打印机打印同一个文件。
* 优点：<br>
1、在内存里只有一个实例，减少了内存的开销，尤其是频繁的创建和销毁实例（比如管理学院首页页面缓存）。<br>
2、避免对资源的多重占用（比如写文件操作）。 <br>
* 缺点：没有接口，不能继承，与单一职责原则冲突，一个类应该只关心内部逻辑，而不关心外面怎么样来实例化。

* 一下是golang单例模式的四种实现（懒汉式、饿汉式、双重检查、sync.Once）
* 1，懒汉模式————非线程安全<br>
懒汉模式即在需要使用的时候才进行创建
```golang
package main

import (
	"fmt"
	"time"
)

var instance *earth

type earth struct {
	Weight	int
	Size	int
	Density	int
}
func (e *earth) Add() {
	e.Weight++
	e.Size++
	e.Density++
}
func GetInstance() *earth {
	// 非线程安全。instance可能被多次创建
	if instance == nil {
		time.Sleep(1*time.Second)
		instance = &earth{1, 1, 1}
	}
	return instance
}
func TestGoroutine(ch chan bool) {
	instance := GetInstance()
	instance.Add()
	fmt.Println(instance.Weight, instance.Size, instance.Density)
	ch <- true
}
func main() {
	num := 5
	ch := make(chan bool)
	for i := 0; i < num; i++ {
		go TestGoroutine(ch)
	}
	for i := 0; i < num; i++ {
		fmt.Println(<-ch)
	}
	fmt.Println("end...", instance.Weight, instance.Size, instance.Density)
}
```

* 2，懒汉模式————线程安全
```golang
package main

import (
	"fmt"
	"sync"
	"time"
)

var instance *earth
var mutex sync.Mutex

type earth struct {
	Weight	int
	Size	int
	Density	int
}
func (e *earth) Add() {
	e.Weight++
	e.Size++
	e.Density++
}
func GetInstance() *earth {
	// 线程安全。进行了加锁操作
	mutex.Lock()
	defer mutex.Unlock()
	if instance == nil {
		time.Sleep(1*time.Second)
		instance = &earth{1, 1, 1}
	}
	return instance
}
func TestGoroutine(ch chan bool) {
	instance := GetInstance()
	instance.Add()
	fmt.Println(instance.Weight, instance.Size, instance.Density)
	ch <- true
}
func main() {
	num := 5
	ch := make(chan bool)
	for i := 0; i < num; i++ {
		go TestGoroutine(ch)
	}
	for i := 0; i < num; i++ {
		fmt.Println(<-ch)
	}
	fmt.Println("end...", instance.Weight, instance.Size, instance.Density)
}
```

* 3，饿汉模式<br>
直接创建好对象，这样不需要判断为空，同时也是线程安全。唯一的缺点是在导入包的同时会创建该对象，并持续占有在内存中。
```golang
package main

import (
	"fmt"
	"sync"
)

var instance *earth = &earth{1,1,1}
var mutex sync.Mutex

type earth struct {
	Weight	int
	Size	int
	Density	int
}
func (e *earth) Add() {
	// 实际上这里也需要加锁
	mutex.Lock()
	e.Weight++
	e.Size++
	e.Density++
	mutex.Unlock()
}
func GetInstance() *earth {
	return instance
}
func TestGoroutine(ch chan bool) {
	instance := GetInstance()
	instance.Add()
	fmt.Println(instance.Weight, instance.Size, instance.Density)
	ch <- true
}
func main() {
	num := 5
	ch := make(chan bool)
	for i := 0; i < num; i++ {
		go TestGoroutine(ch)
	}
	for i := 0; i < num; i++ {
		fmt.Println(<-ch)
	}
	fmt.Println("end...", instance.Weight, instance.Size, instance.Density)
}
```

* 4，双重检查<br>
在懒汉式（线程安全）的基础上再进行忧化，判少加锁的操作。保证线程安全同时不影响性能
```golang
package main

import (
	"fmt"
	"sync"
)

var instance *earth
var mutex sync.Mutex

type earth struct {
	Weight	int
	Size	int
	Density	int
}
func (e *earth) Add() {
	// 实际上这里也需要加锁
	mutex.Lock()
	e.Weight++
	e.Size++
	e.Density++
	mutex.Unlock()
}
func GetInstance() *earth {
	// 先不进行加锁。保证在创建实例完成后再调用该函数时不需要进行加锁
	if instance == nil {
		// 为了线程安全，此时加锁
		mutex.Lock()
		if instance == nil {
			instance = &earth{1,1,1}
		}
		mutex.Unlock()
	}
	return instance
}
func TestGoroutine(ch chan bool) {
	instance := GetInstance()
	instance.Add()
	fmt.Println(instance.Weight, instance.Size, instance.Density)
	ch <- true
}
func main() {
	num := 5
	ch := make(chan bool)
	for i := 0; i < num; i++ {
		go TestGoroutine(ch)
	}
	for i := 0; i < num; i++ {
		fmt.Println(<-ch)
	}
	fmt.Println("end...", instance.Weight, instance.Size, instance.Density)
}
```

* 5，sync.Once<br>
通过sync.Once 来确保创建对象的方法只执行一次<br>
sync.Once内部本质上也是双重检查的方式，但在写法上会比自己写双重检查更简洁，
```golang
package main

import (
	"fmt"
	"sync"
)

var instance *earth
var mutex sync.Mutex
var once sync.Once


type earth struct {
	Weight	int
	Size	int
	Density	int
}
func (e *earth) Add() {
	// 实际上这里也需要加锁
	mutex.Lock()
	e.Weight++
	e.Size++
	e.Density++
	mutex.Unlock()
}
func GetInstance() *earth {
	once.Do(func() {
		instance = &earth{1,1,1}
	})
	return instance
}
func TestGoroutine(ch chan bool) {
	instance := GetInstance()
	instance.Add()
	fmt.Println(instance.Weight, instance.Size, instance.Density)
	ch <- true
}
func main() {
	num := 5
	ch := make(chan bool)
	for i := 0; i < num; i++ {
		go TestGoroutine(ch)
	}
	for i := 0; i < num; i++ {
		fmt.Println(<-ch)
	}
	fmt.Println("end...", instance.Weight, instance.Size, instance.Density)
}
```
