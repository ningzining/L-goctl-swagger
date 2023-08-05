# goctl-swagger

### 1. 编译goctl-swagger插件

```
go install github.com/ningzining/L-goctl-swagger@latest
```

### 2. 配置环境

将$GOPATH/bin中的L-goctl-swagger添加到环境变量

### 3. 使用姿势

* 创建api文件

    ```go
    syntax = "v1"
  
  info(
  title: "type title here"
  desc: "type desc here"
  author: "type author here"
  email: "type email here"
  version: "type version here"
  )
  type UserInfoReq {
  UserId int64 `form:"user_id"`
  }
  
  type UserInfoResp {
  UserId int64 `json:"user_id"`
  UserName string `json:"user_name"`
  Age int64 `json:"age"`
  Email string `json:"email"`
  }
  
  type UserListReq{
  
  }
  
  type UserListResp{
  UserId int64 `json:"user_id"`
  UserName string `json:"user_name"`
  Age int64 `json:"age"`
  Email string `json:"email"`
  }
  @server (
  prefix :/v1/system/user
  group :user
  )
  service system-api{
  @doc "用户信息"
  @handler userInfo
  get /userInfo (UserInfoReq) returns (UserInfoResp)
  @doc "用户列表"
  @handler userList
  get /userList (UserListReq) returns (UserListResp)
  }
    ```

* 生成swagger.json 文件

    ```shell script
    goctl api plugin -plugin L-goctl-swagger="swagger -filename user.json" -api user.api -dir .
    ```

* 指定Host，basePath [api-host-and-base-path](https://swagger.io/docs/specification/2-0/api-host-and-base-path/)

    ```shell script
    goctl api plugin -plugin goctl-swagger="swagger -filename user.json -host 127.0.0.2 -basepath /api" -api user.api -dir .
    ```

* swagger ui 查看生成的文档

    ```shell script
     docker run --rm -p 8083:8080 -e SWAGGER_JSON=/foo/user.json -v $PWD:/foo swaggerapi/swagger-ui
   ```

* Swagger Codegen 生成客户端调用代码(go,javascript,php)

  ```shell script
  for l in go javascript php; do
    docker run --rm -v "$(pwd):/go-work" swaggerapi/swagger-codegen-cli generate \
      -i "/go-work/rest.swagger.json" \
      -l "$l" \
      -o "/go-work/clients/$l"
  done
   ```
