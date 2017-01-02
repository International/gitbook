# Named Parameters

Named parameters are just custom paths for your routes, you can access them for each request
using context

```go
// Param returns the string representation of the key's path named parameter's value
Param(key string) string

// ParamDecoded returns a url-query-decoded path parameter's value
ParamDecoded(key string) string

// ParamInt returns the int representation of the key's path named parameter's value
ParamInt(key string) (int, error)

// ParamInt64 returns the int64 representation of the key's path named parameter's value
ParamInt64(key string) (int64, error)
```

There's no limit on how long a path can be.

Usage:


```go
package main

import (
	"strconv"

	"github.com/kataras/iris"
)

func main() {
	// Matches /hello/iris,  (if PathCorrection:true match also /hello/iris/)
	// Doesn't match /hello or /hello/ or /hello/iris/something
	iris.Get("/hello/:name", func(c *iris.Context) {
		// Retrieve the parameter name
		name := c.Param("name")
		c.Writef("Hello %s", name)
	})

	// Matches /profile/iris/friends/1, (if PathCorrection:true match also /profile/iris/friends/1/)
	// Doesn't match /profile/ or /profile/iris
	// Doesn't match /profile/iris/friends or  /profile/iris/friends
	// Doesn't match /profile/iris/friends/2/something
	iris.Get("/profile/:fullname/friends/:friendID", func(c *iris.Context) {
		// Retrieve the parameters fullname and friendID
		fullname := c.Param("fullname")
		friendID, err := c.ParamInt("friendID")
		if err != nil {
			// Do something with the error
			return
		}
		c.HTML(iris.StatusOK, "<b> Hello </b>"+fullname+"<b> with friends ID </b>"+strconv.Itoa(friendID))
	})

	// Route Example:
	// /posts/:id and /posts/new conflict with each other for performance reasons and simplicity (dynamic value conficts with the static 'new').   
	// but if you need to have them you can do following:
	iris.Get("/posts/*action", func(ctx *iris.Context) {
		action := ctx.Param("action")
		if action == "/new" {
			// it's posts/new page
			ctx.Writef("POSTS NEW")
		} else {
			ctx.Writef("OTHER POSTS")
			// it's posts/:id page
			//doSomething with the action which is the id
		}
	})

	iris.Listen(":8080")
}


```

### Match anything

```go
// Will match any request which's url prefix is "/anything/" and has content after that
// Matches /anything/whateverhere/whateveragain or /anything/blablabla
// c.Param("randomName") will be /whateverhere/whateveragain, blablabla
// Doesn't match /anything or /anything/ or /something
iris.Get("/anything/*randomName", func(c *iris.Context) { } )
```
