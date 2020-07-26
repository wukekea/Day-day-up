```golang
type Context struct {
    // responseWriter对ResponseWriter封装
    writermem responseWriter

    // http请求及响应writer
    Request   *http.Request
    Writer    ResponseWriter

    // 键值对切片类型
    Params   Params

    // HandlerFunc切片类型
    handlers HandlersChain
    index    int8
    fullPath string

    // 当前上下文属于哪个引擎
    engine *Engine

    // 每个请求上下文专用键值对
    Keys map[string]interface{}

    // *Error切片类型，Error封装error类型
    //所有使用当前上下文的handlers/middlewares对应的出错信息都存放在这里
    Errors errorMsgs

    // Accepted defines a list of manually accepted formats for content negotiation.
    Accepted []string

    // queryCache use url.ParseQuery cached the param query result from c.Request.URL.Query()
    queryCache url.Values

    // formCache use url.ParseQuery cached PostForm contains the parsed form data from POST, PATCH,
    // or PUT body parameters.
    formCache url.Values
}

```
### WithCancel的使用
* WithCancel()函数接受一个 Context 并返回其子Context和取消函数cancel
* 新创建协程中传入子Context做参数，且需监控子Context的Done通道，若收到消息，则退出
* 需要新协程结束时，在外面调用 cancel 函数，即会往子Context的Done通道发送消息
* 注意：当 父Context的 Done() 关闭的时候，子 ctx 的 Done() 也会被关闭
* 测试代码如下：
```golang
package main
import (
	"context"
	"fmt"
	"time"
)

func main() {
	// 父context(利用根context得到)
	ctx, cancel := context.WithCancel(context.Background())
	// 父context的子协程
	go watch1(ctx)
	// 子context，注意：这里虽然也返回了cancel的函数对象，但是未使用
	valueCtx, _ := context.WithCancel(ctx)
	// 子context的子协程
	go watch2(valueCtx)
	fmt.Println("现在开始等待3秒,time=", time.Now().Unix())
	time.Sleep(3 * time.Second)
	// 调用cancel()
	fmt.Println("等待3秒结束,调用cancel()函数")
	cancel()
	// 再等待5秒看输出，可以发现父context的子协程和子context的子协程都会被结束掉
	time.Sleep(5 * time.Second)
	fmt.Println("最终结束,time=", time.Now().Unix())
}
// 父context的协程
func watch1(ctx context.Context) {
	for {
		select {
		case <-ctx.Done(): //取出值即说明是结束信号
			fmt.Println("收到信号，父context的协程退出,time=", time.Now().Unix())
			return
		default:
			fmt.Println("父context的协程监控中,time=", time.Now().Unix())
			time.Sleep(1 * time.Second)
		}
	}
}
// 子context的协程
func watch2(ctx context.Context) {
	for {
		select {
		case <-ctx.Done(): //取出值即说明是结束信号
			fmt.Println("收到信号，子context的协程退出,time=", time.Now().Unix())
			return
		default:
			fmt.Println("子context的协程监控中,time=", time.Now().Unix())
			time.Sleep(1 * time.Second)
		}
	}
}
```


```golang
func main() {
	// 设置路由，其返回值是：*Engine
	// Default returns an Engine instance with the Logger and Recovery middleware already attached.
	// gin.Default()与gin.New()相比，前者为框架实例添加了Logger和Recovery中间件.
	engine := gin.Default()
	// 绑定路由规则，执行的函数
	engine.GET("/hello`", func(c *gin.Context) {
		c.String(http.StatusOK, "hello world")

	})

	engine.GET("/json", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"name:":"admin",
			"age:":24,
		})
	})

	engine.Run()

}
```

```golang
// gin请求路由

// 使用参数作为URL测试
package main

import (
	"github.com/gin-gonic/gin"
	"io/ioutil"
	"net/http"
	"time"
)

func main() {
	r := gin.Default()

	// 执行后返回路由：http://localhost:8080/test1/wkk/12
	// 获取请求路由
	r.GET("/test1/:name/:id", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"name": c.Param("name"),
			"id": c.Param("id"),
		})
	})

	// 获取请求参数
	// 获取get参数
	// 测试路由：http://localhost:8080/test3?first_name=wu
	r.GET("/test3", func(c *gin.Context) {
		// 不带默认参数
		firstName := c.Query("first_name")
		// 带默认值
		lastName := c.DefaultQuery("last_name", "last_default——name")
		c.String(http.StatusOK, "%s, %s", firstName, lastName)
	})

	// 获取body内容测试
	// 访问路由：http://localhost:8080/test4
	r.POST("/test4", func(c *gin.Context) {
		// 使用ioutil获取body内容
		// ReadAll函数返回一个字节数组和err
		// 注意，使用ReadAll函数读取完数据后将不再能拿到请求的数据
		// 即：使用ReadAll函数后再：c.PostForm()将无法获取值
		// 解决方法：将bodyBytes值重新写入c.Request.Body即可
		// c.Request.Body = ioutil.NopCloser(bytes.NewBuffer(bodyBytes))
		bodyBytes, err := ioutil.ReadAll(c.Request.Body)
		if err != nil {
			c.String(http.StatusBadRequest, err.Error())
			// 结束输出
			c.Abort()
		}
		c.String(http.StatusOK, string(bodyBytes))

		// 此时将输出default
		// test := c.DefaultPostForm("test", "default")
		// c.String(http.StatusOK, test)

		//能够打印出数据
		//c.Request.Body = ioutil.NopCloser(bytes.NewBuffer(bodyBytes))
		//test := c.DefaultPostForm("test", "default")
		//c.String(http.StatusOK, test)
	})

	// 泛绑定
	// 所有user前缀的路由都将调用该接口
	// 例如访问：http://localhost:8080/test2/hejafsdlf
	r.GET("/test2/*action", func(c *gin.Context) {
		c.String(http.StatusOK, "路由泛绑定")
	})

	// 参数绑定结构体测试
	// 使用get和post方法测试
	r.GET("/test5", testing)
	r.POST("/test5", testing)
	
	r.Run()

}

// 传递Birthday时出现问题
// 解决方法：time_format后面的时间必须为：2006-01-02
type Person struct {
	Name		string		`form:"name"`
	Address 	string		`form:"address"`
	Birthday 	time.Time	`form:"birthday" time_format:"2006-01-02"`
}

func testing(c *gin.Context) {
	var person Person
	// ShouldBind函数会根据请求的content-type来做不同的binding操作
	if err := c.ShouldBind(&person); err == nil {
		c.String(http.StatusOK, "%v", person)
	} else {
		c.String(http.StatusOK, "person bind error: %v", err)
	}
}
```

```golang
// 验证请求参数

// 用于参数验证
// 前提是参数已经绑定到了结构体中
package main

import (
	"github.com/gin-gonic/gin"
	"github.com/gin-gonic/gin/binding"
	"github.com/go-playground/validator/v10"
	"net/http"
)

type Person struct {
	// 使用binding绑定验证规则
	// binding后的验证规则如果有多个的话，需要使用逗号隔开，期间不能加空格
	// required:该参数为必须；
	// gt=10：年龄需要大于10
	// 其它一些验证规则参见：https://godoc.org/gopkg.in/go-playground/validator.v9
	Age 	int		`form:"age" binding:"required,gt=10"`
	Name 	string	`form:"name" binding:"required"`
	Address	string	`form:"address" binding:"required"`
}

type Booking struct {
	// maxPrice：自定义的验证规则
	Name	string 	`form:"name" binding:"required"`
	Price 	int		`form:"price" binding:"required,maxPrice"`
}

func maxPrice(fl validator.FieldLevel) bool {
	const MAXPRICE int64 = 100
	// 通过fl.Field().Int()获取Price值
	// 返回值类型为int64
	price := fl.Field().Int()
	if price < MAXPRICE {
		return true
	}
	return false
}


func main(){
	r := gin.Default()

	// 默认验证规则
	// 访问网址：http://localhost:8080/test?age=10&name=wukeke&address=江西
	r.GET("/test", func(c *gin.Context) {
		var person Person
		if err := c.ShouldBind(&person); err != nil {
			c.String(http.StatusBadRequest, "%v", err)
			return
		}
		c.String(http.StatusOK, "%v", person)
	})

	// 自定义验证规则
	//将验证方法注册到验证器中
	if v, ok := binding.Validator.Engine().(*validator.Validate); ok {
		v.RegisterValidation("maxPrice", maxPrice)
	}
	// 访问地址为：http://localhost:8080/bookable?name=wukeke&price=10
	r.GET("/bookable", func(c *gin.Context) {
		var b Booking
		if err := c.ShouldBind(&b); err != nil {
			c.JSON(500, gin.H{"error":err.Error()})
			return
		}
		c.JSON(200, gin.H{
			"message":"ok",
			"booking":b,
		})
	})
	// 多语言翻译验证
	// 暂缺
	r.Run()
}

```

```golang
// 中间件

// 中间件是基于gin服务器和执行的回调函数之间的一个中间层
// 可以用来请求拦截和日志打印等
package main

import (
	"fmt"
	"github.com/gin-gonic/gin"
	"io"
	"net/http"
	"os"
)

func IPAuthMiddleware() gin.HandlerFunc {
	return func(c *gin.Context) {
		ipList := []string {
			"127.0.0.1",
		}
		flag := false
		clientIP := c.ClientIP()
		fmt.Print(clientIP)
		for _, host := range ipList {
			if clientIP == host {
				flag = true
				break
			}
		}
		// 不在白名单中
		if !flag {
			c.String(401, "%s, clientIP not in ipList", clientIP)
			return
		}
	}
}

func main() {
	r := gin.New()
	// Logger中间件用来在发送请求时在控制台（默认）打印出url及其耗时等信息
	// 比如：[GIN] 2020/07/15 - 11:34:49 | 200 |            0s |             ::1 | GET      "/test?name=wukeke"
	// 如果想要修改打印信息的位置，方法如下：

	// 新建打印的文件f
	f, _ := os.Create("Logger_test.log")
	gin.DefaultWriter = io.MultiWriter(f)
	gin.DefaultErrorWriter = io.MultiWriter(f)

	r.Use(gin.Logger())

	r.GET("/test", func(c *gin.Context) {
		name := c.DefaultQuery("name", "default_name")
		c.String(http.StatusOK, name)
	})

	// 自定义中间件
	r.Use(IPAuthMiddleware())
	r.GET("test2", func(c *gin.Context) {
		c.String(http.StatusOK, "test2 中间件自定义")
	})

	r.Run()
}
```

```golang
// 优雅关停

package main

import (
	"context"
	"github.com/gin-gonic/gin"
	"log"
	"net/http"
	"os"
	"os/signal"
	"syscall"
	"time"
)

func main() {
	r := gin.Default()

	r.GET("/test", func(c *gin.Context) {
		time.Sleep(5*time.Second)
		c.String(http.StatusOK, "hello test")
	})

	// 使用http.Server来创建一个Server，而不再使用gin.Run()
	server := &http.Server{
		Addr: ":8085",
		Handler: r,
	}
	// 将server放入协程
	go  func() {
		if err := server.ListenAndServe(); err != nil && err != http.ErrServerClosed {
			log.Fatal("listen: %s\n", err)
		}
	}()
	// 将请求拦截交给os.Signal信号
	// 将os.Signal放入一个chan
	quit := make(chan os.Signal)

	// os.Signal信号退出的捕获
	signal.Notify(quit, syscall.SIGINT, syscall.SIGTERM)
	<-quit
	log.Println("shut down server...")

	// 创建一个超时上下文
	ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
	defer cancel()

	// 执行关闭服务器
	if err := server.Shutdown(ctx); err != nil {
		log.Fatal("server shutdown: ", err)
	}
	log.Println("server exiting")

}
```

```golang
// 模板渲染

// 需要在同目录下创建：tmp/index.html
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)

func main() {
	r := gin.Default()
	// 将tmp里面的模板载入gin框架里
	// goland 找文件夹是基于整个项目的根目录，即：D:\Job\project\app-service
	r.LoadHTMLGlob("tmp/*")
	r.GET("/index", func(c *gin.Context) {
		c.HTML(http.StatusOK, "index.html", gin.H{
			"title":"index.html",
		})
	})
	r.Run()
}
```

