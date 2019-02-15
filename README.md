# Flow [![Build Status](https://circleci.com/gh/facebook/flow/tree/master.svg?style=shield)](https://circleci.com/gh/facebook/flow/tree/master) [![Windows Build Status](https://ci.appveyor.com/api/projects/status/thyvx6i5nixtoocm/branch/master?svg=true)](https://ci.appveyor.com/project/Facebook/flow/branch/master)

Flow是一个JavaScript的静态类型检查器。关于Flow更多的消息，请查看[flow.org](https://flow.org/)。

关于项目的到背景，请阅读[概述](https://flow.org/en/docs/lang/)。

# 要求
* Mac OS X
* Linux (64-bit)
* Winodws (64-bit, 推荐Windows 10)

每个平台都有[二进制版本分布](https://github.com/facebook/flow/releases)，你也可以通过源码构建任意平台的版本。

## 安装flow

安装flow比较简单：你只需把`flow`的二进制文件的路径添加到系统环境变量即可。

### 局部安装flow

安装Flow的推荐方式通过[`flow-bin`](https://www.npmjs.com/package/flow-bin) `npm` 包管理。添加`flow-bin`到项目的`package.json`文件。

- 提供了更平滑的升级体验，因为Flow的正确版本会基于你检查的修订版自动使用
- 安装Flow作为已经存在的`npm install`工作流的一部分
- 让你在不同的项目中使用Flow的不同版本

```
npm install --save-dev flow-bin
node_modules/.bin/flow
```

### 全局安装Flow

虽然不推荐，但是你也可以全局安装Flow(例如，也许你不使用`npm`或`package.json`)。

全局安装的最好方式是通过`flow-bin`:

```
npm install -g flow-bin
flow # make sure `npm bin -g` is on your path
```
对于Mac OS X，你可以通过[Homebrew](http://brew.sh/)包管理器安装Flow:

```
brew update
brew install flow
```
你也可以通过OCaml [OPAM](https://opam.ocaml.org) 包管理器安装Flow。因为Flow有一些非OCaml的安装包，因此你需要使用[`depext`](https://opam.ocaml.org/doc/FAQ.html#Somepackagefailduringcompilationcomplainingaboutmissingdependenciesquotm4quotquotlibgtkquotetc)包，例如:

```
opam install depext
opam depext --install flowtype
```
如果你没有一个足够新的OCaml版本编译Flow，你也可以使用OPAM来引导一个当前版本。通过[二进制安装包](http://opam.ocaml.org/doc/Install.html#InstallOPAMin2minutes)为你的操作系统安装OPAM并运行:

```
opam init --comp=4.05.0
opam install flowtype
eval `opam config env`
flow --help
```

## 开始

使用flow非常容易。

- 在项目的根路径下运行下面的命令，初始化Flow
```
flow init
```

- 所有要进行类型检查的文件顶部都需要添加下面的程序语句
``` javascript
/* @flow */
```
或
``` javascript
// @flow
```

- 运行flow
```
flow check
```

 [flow.org](https://flow.org/)能找到更详细的文档和更多的示例。

## 构建 Flow

Flow使用OCaml编写的(需要OCaml4.05.0或更高版本)。你可以按照[ocaml.org](https://ocaml.org/docs/install.html)的说明，在Mac OS X和Linux上安装OCaml。

例如Ubuntu 16.04和相似的系统：

```
sudo apt-get install opam
opam init --comp 4.05.0
```

OS X要使用 [brew package manager](http://brew.sh/)

```
brew install opam
opam init --comp 4.05.0
```
然后重启shell并安装下面这些其他的库：

```
opam update
opam pin add flowtype . -n
opam install --deps-only flowtype
```

一旦你安装了这些依赖，运行下面的命令就可以构建Flow

```
make
```

运行上面的命令以后会生成一个包含`flow`二进制文件的`bin`文件夹。

为了生成flow.js文件，首先要安装js_of_ocaml：

```
opam install -y js_of_ocaml
```

之后就可以轻松的生成flow.js文件：

```
make js
```

新的`flow.js`文件也将会在`bin`w文件夹下。

*注意：OCaml依赖会阻止我们添加Flow到[npm](http://npmjs.org)。如果你需要一个npm二进制容器可以尝试[flow-bin](https://www.npmjs.org/package/flow-bin)。*

Flow也能把它的解析器编译成JavaScript。[如何解析](https://github.com/facebook/flow/blob/master/src/parser/README.md)

## Windows平台构建Flow

在Windows平台上构建Flow有点儿复杂。下面的构建过程有一些简化，不过依然能够成功构建。

通常的想法是在Cygwin中构建，目标是mingw。这样我们会得到一个即使在Cygwin外也能工作的二进制文件。

### 安装Cygwin
1. 从 https://cygwin.com/install.html 安装Cygwin 64位
2. 在powershell运行 `iex ((new-object net.webclient).DownloadString("https://raw.githubusercontent.com/ocaml/ocaml-ci-scripts/master/appveyor-install.ps1"))`，这可能会运行一个带有一堆Cygwin安装包和其他东西的Cygwin安装程序。这样可以帮助确认opam需要的每一个安装包都是可用的。

### 安装Opam
1. 打开cygwin64终端
2. 通过`curl -fsSL -o opam64.tar.xz https://github.com/fdopen/opam-repository-mingw/releases/download/0.0.0.1/opam64.tar.xz`下载opam
3. `tar -xf opam64.tar.xz`
4. `cd opam64`
5. 安装opam `./install.sh`
6. 初始化opam并指向一个mingw分支: `opam init -a default "https://github.com/fdopen/opam-repository-mingw.git" --comp "4.05.0+mingw64c" --switch "4.05.0+mingw64c"`
7. 确保opam的相关东西已经添加到环境变量中: ```eval `opam config env` ```

### 安装Flow
1. 克隆flow项目:`git clone https://github.com/facebook/flow.git`
2. `cd flow`
3. 告诉opam使用这个目录作为flow类型的项目：`opam pin add flowtype . -n`
4. 安装系统依赖：`opam depext -u flowtype`
5. 安装Flow的依赖：`opam install flowtype --deps-only`
6. 最后再构建Flow：`make all`
