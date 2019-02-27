# cod-logger

[![Build Status](https://img.shields.io/travis/vicanso/cod-logger.svg?label=linux+build)](https://travis-ci.org/vicanso/cod-logger)

Access logger for cod, it support multiple tags from request and response.

```go
package main

import (
	"bytes"
	"fmt"

	"github.com/vicanso/cod"

	codlogger "github.com/vicanso/cod-logger"
)

func main() {
	d := cod.New()

	d.Use(codlogger.New(codlogger.Config{
		Format: codlogger.CommonFormat,
		OnLog: func(str string, _ *cod.Context) {
			fmt.Println(str)
		},
	}))

	// http://127.0.0.1:7001/?_fields=foo,id
	d.GET("/", func(c *cod.Context) (err error) {
		c.StatusCode = 200
		c.SetHeader(cod.HeaderContentType, cod.MIMEApplicationJSON)
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