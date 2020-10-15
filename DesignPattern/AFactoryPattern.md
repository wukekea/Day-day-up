#设计模式中的工厂模式。
* 工厂模式属于创建型模式。
* 它提供了一种创建对象的最佳方式。在工厂模式中，我们在创建对象时不会对客户端暴露创建逻辑，并且是通过使用一个共同的接口来指向新创建的对象。
* 应用实例： <br>
1、您需要一辆汽车，可以直接从工厂里面提货，而不用去管这辆汽车是怎么做出来的，以及这个汽车里面的具体实现。<br>
2、Hibernate 换数据库只需换方言和驱动就可以。 
* 优点：<br> 
1、一个调用者想创建一个对象，只要知道其名称就可以了。 <br>
2、扩展性高，如果想增加一个产品，只要扩展一个工厂类就可以。 3、屏蔽产品的具体实现，调用者只关心产品的接口。 
* 缺点：<br>
每次增加一个产品时，都需要增加一个具体类和对象实现工厂，使得系统中类的个数成倍增加，在一定程度上增加了系统的复杂度，同时也增加了系统具体类的依赖。这并不是什么好事。
* 使用场景： 1、日志记录器：记录可能记录到本地硬盘、系统事件、远程服务器等，用户可以选择记录日志到什么地方。<br> 
2、数据库访问，当用户不知道最后系统采用哪一类数据库，以及数据库可能有变化时。 <br>
3、设计一个连接服务器的框架，需要三个协议，"POP3"、"IMAP"、"HTTP"，可以把这三个作为产品类，共同实现一个接口。 
* 注意事项：作为一种创建类模式，在任何需要生成复杂对象的地方，都可以使用工厂方法模式。有一点需要注意的地方就是复杂对象适合使用工厂模式，而简单对象，特别是只需要通过 new 就可以完成创建的对象，无需使用工厂模式。如果使用工厂模式，就需要引入一个工厂类，会增加系统的复杂度。
* 工厂模式示例如下：

![工厂模式类图](assets/FactoryPattern.PNG)

```golang
package main
import (
	"fmt"
	"math"
)

type Shape interface {
	CalcArea() float64
	String() string
}

type circle struct {
	Radius	float64
}
type rectangle struct {
	Height	float64
	Width	float64
}
type square struct {
	SideLenght 	float64
}

func (c circle) CalcArea() float64 {
	return math.Pi*c.Radius*c.Radius
}
func (c circle) String() string {
	return fmt.Sprintf("I am a circle")
}
func (c rectangle) CalcArea() float64 {
	return c.Height*c.Width
}
func (c rectangle) String() string {
	return fmt.Sprintf("I am a rectangle")
}
func (c square) CalcArea() float64 {
	return c.SideLenght*c.SideLenght
}
func (c square) String() string {
	return fmt.Sprintf("I am a square")
}
func NewCircle(radius float64) circle {
	return circle{Radius:radius}
}
func NewRectangle(height, width float64) rectangle {
	return rectangle{
		Height: height,
		Width:  width,
	}
}
func NewSquare(sideLength float64) square {
	return square{SideLenght:sideLength}
}

func main() {
	shape := make([]Shape, 0)
	shape = append(shape, NewCircle(5.0))
	shape = append(shape, NewRectangle(5.0, 6.0))
	shape = append(shape, NewSquare(6.0))
	for _, value := range shape {
		fmt.Println(value.String(), value.CalcArea())
	}
}
```
