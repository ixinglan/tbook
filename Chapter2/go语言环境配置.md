# go语言环境配置

1.使用brew安装最新稳定版本的go 1.13
```
brew search go

==> Formulae
algol68g                     gnu-go                       gobuster                     google-java-format           govendor                     pango
anycable-go                  go                           gocr                         google-sparsehash            gowsdl                       pangomm
arangodb                     go-bindata                   gocryptfs                    google-sql-tool              gox                          powerline-go
argon2                       go-jira                      godep                        googler                      gst-plugins-good             protoc-gen-go
aws-google-auth              go-md2man                    goenv                        goolabs                      gx-go                        pygobject3
baidupcs-go                  go-statik                    gofabric8                    goose                        hugo ✔                       ringojs
bogofilter                   go@1.10                      goffice                      gopass                       jfrog-cli-go                 spaceinvaders-go
cargo-c                      go@1.11                      golang-migrate               gor                          jpegoptim                    spigot
cargo-completion             go@1.12                      gollum                       goreleaser                   katago                       svgo
cargo-instruments            go@1.13                      golo                         goreman                      lego                         wego
certigo                      go@1.9                       gom                          gosu                         lgogdownloader               wireguard-go

```
```
brew install go@1.13

Updating Homebrew...
==> Downloading https://homebrew.bintray.com/bottles/go@1.13-1.13.10_1.catalina.bottle.tar.gz
==> Downloading from https://akamai.bintray.com/25/2584dae283ebba63091d06fa1fd15ee9d218b79a60f0c19ba38a7ef8b9e08fdc?__gda__=exp=1588738427~hmac=9ae05b9de1d
######################################################################## 100.0%
==> Pouring go@1.13-1.13.10_1.catalina.bottle.tar.gz
==> Caveats
go@1.13 is keg-only, which means it was not symlinked into /usr/local,
because this is an alternate version of another formula.

If you need to have go@1.13 first in your PATH run:
  echo 'export PATH="/usr/local/opt/go@1.13/bin:$PATH"' >> ~/.zshrc

==> Summary
🍺  /usr/local/Cellar/go@1.13/1.13.10_1: 9,279 files, 414.5MB
==> `brew cleanup` has not been run in 30 days, running now...
Removing: /Users/zhaojianqiang/Library/Caches/Homebrew/gcc--9.2.0_2.catalina.bottle.tar.gz... (84.8MB)
Removing: /Users/zhaojianqiang/Library/Caches/Homebrew/gmp--6.1.2_2.catalina.bottle.1.tar.gz... (996.4KB)
Removing: /Users/zhaojianqiang/Library/Caches/Homebrew/isl--0.21.catalina.bottle.tar.gz... (1.4MB)
Removing: /Users/zhaojianqiang/Library/Caches/Homebrew/libmpc--1.1.0.catalina.bottle.tar.gz... (114.4KB)
Removing: /Users/zhaojianqiang/Library/Caches/Homebrew/mpfr--4.0.2.catalina.bottle.tar.gz... (1.1MB)
Removing: /Users/zhaojianqiang/Library/Caches/Homebrew/gradle--6.0.1.zip... (134.7MB)
Removing: /Users/zhaojianqiang/Library/Logs/Homebrew/gmp... (64B)
Removing: /Users/zhaojianqiang/Library/Logs/Homebrew/mpfr... (64B)
Removing: /Users/zhaojianqiang/Library/Logs/Homebrew/gcc... (64B)
Removing: /Users/zhaojianqiang/Library/Logs/Homebrew/gradle... (102B)
Removing: /Users/zhaojianqiang/Library/Logs/Homebrew/isl... (64B)
Removing: /Users/zhaojianqiang/Library/Logs/Homebrew/groovy... (64B)
Removing: /Users/zhaojianqiang/Library/Logs/Homebrew/libmpc... (64B)
Pruned 14 symbolic links and 1 directories from /usr/local
```
到这一步go即安装完成了  

2.配置go
```
go env

GO111MODULE=""
GOARCH="amd64"
GOBIN="/Users/zhaojianqiang/Documents/code/go/bin"
GOCACHE="/Users/zhaojianqiang/Library/Caches/go-build"
GOENV="/Users/zhaojianqiang/Library/Application Support/go/env"
GOEXE=""
GOFLAGS=""
GOHOSTARCH="amd64"
GOHOSTOS="darwin"
GONOPROXY=""
GONOSUMDB=""
GOOS="darwin"
GOPATH="/Users/zhaojianqiang/Documents/code/go"
GOPRIVATE=""
GOPROXY="https://proxy.golang.org,direct"
GOROOT="/usr/local/opt/go@1.13/libexec"
GOSUMDB="sum.golang.org"
GOTMPDIR=""
GOTOOLDIR="/usr/local/opt/go@1.13/libexec/pkg/tool/darwin_amd64"
GCCGO="gccgo"
AR="ar"
CC="clang"
CXX="clang++"
CGO_ENABLED="1"
GOMOD=""
CGO_CFLAGS="-g -O2"
CGO_CPPFLAGS=""
CGO_CXXFLAGS="-g -O2"
CGO_FFLAGS="-g -O2"
CGO_LDFLAGS="-g -O2"
PKG_CONFIG="pkg-config"
GOGCCFLAGS="-fPIC -m64 -pthread -fno-caret-diagnostics -Qunused-arguments -fmessage-length=0 -fdebug-prefix-map=/var/folders/1v/hmp36dtx5wlbpc2zpkk2w2h80000gp/T/go-build469326368=/tmp/go-build -gno-record-gcc-switches -fno-common"
```
将GOROOT, GOPATH配置到.bash_profile中
```
vim .bash_profile
#添加如下 注意GOPATH不要配置到go的安装目录下,可以自定义一个目录
#GOROOT： go安装目录
#GOPATH：go工作目录
#GOBIN：go可执行文件目录
#PATH：将go可执行文件加入PATH中，使GO命令与我们编写的GO应用可以全局调用
export GOROOT=/usr/local/opt/go\@1.13
export GOPATH=/Users/zhaojianqiang/Documents/code/go
export GOBIN=$GOPATH/bin
export PATH=$PATH:$GOBIN:$GOROOT/bin
#完成后保存并使之生效
source .bash_profile
```
再次使用go env查看, 即可看到刚才配置的信息  
到此go就完全安装完成了, 常用工具用GoLand, 开始你的go编程之路吧

3.linux配置
以上是mac os系统配置go, linux的go环境配置相当简单
```shell
# step1 下载包
wget https://dl.google.com/go/go1.14.4.linux-amd64.tar.gz
# step2 解压
tar -C /usr/local -xzf go1.14.4.linux-amd64.tar.gz
# step3 环境变量配置 在/etc/profile文件追加
export PATH=$PATH:/usr/local/go/bin
# step4 测试安装 创建一个hello.go文件,包含以下代码
package main

import "fmt"

func main() {
	fmt.Printf("hello, world\n")
}

# 编译
go build hello.go
# 运行,成功后输入hello, world
./hello 
```

