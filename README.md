# go-ci-release


使用goreleaser 发布go 项目的二进制项目； 

### 步骤
1、<project>/Settings/Secrets 添加 githubtoken(Settings/ Developer settings/Personal access tokens 生成token)
2、<project>/Actions/New workflow，选择go ，用go.yml 替换掉 网页中的内容；
3、add tag 完成，push tag，即可自动构建二进制到release。

### go.yml
```
name: goreleaser
   
   on:
     push:
       tags:
         - '*'
     pull_request:
   
   jobs:
     goreleaser:
       runs-on: ubuntu-latest
       steps:
         -
           name: Checkout
           uses: actions/checkout@v2
           with:
             fetch-depth: 0
         -
           name: Set up Go
           uses: actions/setup-go@v2
           with:
             go-version: 1.15
         -
           name: Run GoReleaser
           uses: goreleaser/goreleaser-action@v2
           with:
             version: latest
             args: release --rm-dist
           env:
             GITHUB_TOKEN: ${{ secrets.GITHUBTOKEN }}

```

[goreleaser 支持的平台参考](https://goreleaser.com/ci/actions/)