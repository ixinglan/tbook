#go语言环境配置

1.使用brew安装最新版本的go 1.9
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
brew install go@1.9

==> Downloading https://homebrew.bintray.com/bottles/go@1.9-1.9.7.catalina.bottle.1.tar.gz
==> Downloading from https://akamai.bintray.com/68/6820e19509cbcdd77f30cb8c16a4ca9e67aa3e9eb6e4c2da33c9f9a7dc223840?__gda__=exp=1588478588~hmac=cc6882a8e0c0e5660b852e1a4b3ee8
######################################################################## 100.0%
==> Pouring go@1.9-1.9.7.catalina.bottle.1.tar.gz
==> Caveats
go@1.9 is keg-only, which means it was not symlinked into /usr/local,
because this is an alternate version of another formula.

If you need to have go@1.9 first in your PATH run:
  echo 'export PATH="/usr/local/opt/go@1.9/bin:$PATH"' >> ~/.zshrc

==> Summary
🍺  /usr/local/Cellar/go@1.9/1.9.7: 7,670 files, 294.2MB
==> `brew cleanup` has not been run in 30 days, running now...
Removing: /Users/zhaojianqiang/Library/Logs/Homebrew/yarn... (64B)
Removing: /Users/zhaojianqiang/Library/Logs/Homebrew/rmtrash... (64B)
```
到这一步go即安装完成了  

2.配置go
```
go env

GOARCH="amd64"
GOBIN=""
GOEXE=""
GOHOSTARCH="amd64"
GOHOSTOS="darwin"
GOOS="darwin"
GOPATH="/Users/***/Documents/code/go"
GORACE=""
GOROOT="/usr/local/opt/go@1.9"
GOTOOLDIR="/usr/local/opt/go@1.9/pkg/tool/darwin_amd64"
GCCGO="gccgo"
CC="clang"
GOGCCFLAGS="-fPIC -m64 -pthread -fno-caret-diagnostics -Qunused-arguments -fmessage-length=0 -fno-common"
CXX="clang++"
CGO_ENABLED="1"
CGO_CFLAGS="-g -O2"
CGO_CPPFLAGS=""
CGO_CXXFLAGS="-g -O2"
CGO_FFLAGS="-g -O2"
CGO_LDFLAGS="-g -O2"
PKG_CONFIG="pkg-config"
```
将GOROOT, GOPATH配置到.bash_profile中
```
vim .bash_profile
#添加如下 注意GOPATH不要配置到go的安装目录下,可以自定义一个目录
#GOROOT： go安装目录
#GOPATH：go工作目录
#GOBIN：go可执行文件目录
#PATH：将go可执行文件加入PATH中，使GO命令与我们编写的GO应用可以全局调用
export GOROOT=/usr/local/opt/go\@1.9
export GOPATH=/Users/zhaojianqiang/Documents/code/go
export GOBIN=$GOPATH/bin
export PATH=$PATH:$GOBIN:$GOROOT/bin
#完成后保存并使之生效
source .bash_profile
```
再次使用go env查看, 即可看到刚才配置的信息  
到此go就完全安装完成了, 常用工具用GoLand, 开始你的go编程之路吧


