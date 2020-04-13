# elton-logger

The middleware has been archived, please use the middleware of [elton](https://github.com/vicanso/elton).

[![Build Status](https://img.shields.io/travis/vicanso/elton-logger.svg?label=linux+build)](https://travis-ci.org/vicanso/elton-logger)

Access logger for elton, it support multiple tags from request and response.

```go
package main

import (
	"bytes"
	"fmt"

	"github.com/vicanso/elton"

	codlogger "github.com/vicanso/elton-logger"
)

func main() {
	d := elton.New()

	d.Use(codlogger.New(codlogger.Config{
		Format: codlogger.CommonFormat,
		OnLog: func(str string, _ *elton.Context) {
			fmt.Println(str)
		},
	}))

	// http://127.0.0.1:7001/?_fields=foo,id
	d.GET("/", func(c *elton.Context) (err error) {
		c.StatusCode = 200
		c.SetHeader(elton.HeaderContentType, elton.MIMEApplicationJSON)
		c.BodyBuffer = bytes.NewBufferString(`{
			"foo": "bar",
			"id": 1,
			"price": 1.21
		}`)
		return
	})

	d.ListenAndServe(":7001")
}
```
