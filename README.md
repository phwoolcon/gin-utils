Utilities for Gin Web Framework
==

## File-based Session
The [Gin middleware for session management](https://github.com/gin-contrib/sessions) has backends like `cookie-based`, `Redis`, `memcached`, `MongoDB`, `memstore`.

But what if you want a `file-based` session?
```go
package main

import (
	"github.com/gin-contrib/sessions"
	"github.com/gin-gonic/gin"
	psession "github.com/phwoolcon/gin-utils/session"
)

func main() {
	engine := gin.Default()
	apiRouter := engine.Group("/api")
	fsStore := psession.NewFileStore("./data/session", []byte("secret"))
	apiRouter.Use(sessions.Sessions("auth", fsStore))

	apiRouter.GET("/hello", func(context *gin.Context) {
		session := sessions.Default(context)

		if session.Get("hello") != "world" {
			session.Set("hello", "world")
			session.Save()
		}

		context.JSON(200, gin.H{"hello": session.Get("hello")})
	})
	engine.Run(":8000")
}
```
