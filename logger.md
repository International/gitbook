# Logger

[This is a middleware](https://github.com/kataras/iris/tree/master/middleware/logger)

Logs the incoming requests

```go
 New(theLogger *logger.Logger, options ...Options) iris.HandlerFunc
```

How to use

```go
package main

import (
	"github.com/kataras/iris"
	"github.com/kataras/iris/middleware/logger"
)

func main() {

	iris.Use(logger.New(iris.Logger()))

	iris.Get("/", func(ctx *iris.Context) {
		ctx.Write("hello")
	})

	iris.Get("/1", func(ctx *iris.Context) {
		ctx.Write("hello")
	})

	iris.Get("/3", func(ctx *iris.Context) {
		ctx.Write("hello")
	})

	// log http errors
	errorLogger := logger.New(iris.Logger(), logger.Options{Latency: false}) //here we just disable to log the latency, no need for error pages
	// yes we have options look at the logger.Options inside kataras/iris/middleware/logger.go
	iris.OnError(iris.StatusNotFound, func(ctx *iris.Context) {
		errorLogger.Serve(ctx)
		ctx.Write("My Custom 404 error page ")
	})
	//

	iris.Listen(":8080")

}

}

```

You can create your **own Logger** to use
```go

import (
    "github.com/kataras/iris/logger"
     mLogger "github.com/kataras/iris/middleware/logger"
)

theLogger := logger.New(config.DefaultLogger())

iris.Use(mLogger.New(theLogger))
```

The configuration for the ` config.Logger` 

