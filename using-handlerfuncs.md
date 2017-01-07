# Using HandlerFuncs

HandlerFuncs should implement the Serve\(Context) func.
HandlerFunc is the most simple method to register a route or a middleware, but under the hood it acts like a Handler. It implements the Handler interface as well:

```go
type HandlerFunc func(*Context)

func (h HandlerFunc) Serve(c *Context) {
    h(c)
}
```

HandlerFuncs shoud have this function signature:

```go
func handlerFunc(c *iris.Context)  {
    c.Writef("Hello")
func handlerFunc(ctx *iris.Context)  {
    ctx.Writef("Hello")
}

iris.HandleFunc("GET","/letsgetit",handlerFunc)
//OR
iris.Get("/letsgetit", handlerFunc)
iris.Post("/letspostit", handlerFunc)
iris.Put("/letputit", handlerFunc)
iris.Delete("/letsdeleteit", handlerFunc)
```
