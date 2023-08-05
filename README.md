# L-goctl-swagger

### 1. 下载goctl-swagger插件

```
go install github.com/ningzining/L-goctl-swagger@latest
```

### 2. 配置环境

将$GOPATH/bin中的L-goctl-swagger添加到环境变量

* 生成swagger.json 文件

    ```shell script
    goctl api plugin -plugin L-goctl-swagger="swagger -filename user.json" -api user.api -dir .
    ```
