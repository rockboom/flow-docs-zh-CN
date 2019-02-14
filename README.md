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

## 项目内安装flow

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