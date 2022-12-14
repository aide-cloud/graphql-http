# graphql-http
graphql http api

# 使用示例

- gin工程中

```go
// service.go
package service

import (
	"embed"
	"github.com/aide-cloud/graphql-http"
	"github.com/gin-gonic/gin"
)

type GraphqlService struct {}

type Root struct {}

func (r *Root) Ping() string {
	return "pong"
}

// Content holds all the SDL file content.
//go:embed sdl
var content embed.FS

func NewRoot() *Root {
	return &Root{}
}

func NewGraphqlService() *GraphqlService {
	return &GraphqlService{}
}

func (g *GraphqlService) RegisterGraphqlGinRouter(root *Root, r *gin.Engine) {
	r.GET("/query", gin.WrapF(graphql.NewGraphQLNetHttpHandlerFunc("/graphql")))
	r.POST("/graphql", gin.WrapH(graphql.NewNetHttpHandler(root, content)))
}

```

```graphql
# root.graphql
schema {
    query: RootQuery
}

type RootQuery {
    ping: String!
}

```

- 目录结构

```text
.
├── README.md
├── sdl
│   └── root.graphql
├── service.go

```
