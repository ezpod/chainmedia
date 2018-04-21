# ubuntu下的以太坊DApp开发环境搭建

如果你不喜欢把时间浪费在搭建开发环境上，可以使用汇智网的在线教程和运行环境：
[以太坊DApp实战开发入门](http://xc.hubwiz.com/course/5a952991adb3847553d205d1?affid=github7878)

# 一、安装前的准备
## 1.1 查看当前CPU架构
在终端中执行以下命令，确定是32位架构还是64位架构：
```
~$ uname –p
x86_64
```
如果你看到输出x86_64，那么就是64位系统，否则是32位。
## 1.2下载工具
确保你安装了下载工具wget：
```
~$ wget –V
GNU Wget 1.17.1 built on linux-gnu
```
如果还没有安装wget，使用apt-get来安装
```
~$ sudo apt-get install wget
```
# 二、安装DApp开发环境
## 2.1 安装Node.js
首先根据你的ubuntu是32位还是64位，分别下载不同的预编译版本，我们使用官方长期支持的8.10.0LTS版本：
64位：
```
~$ wget https://nodejs.org/dist/v8.10.0/node-v8.10.0-linux-x64.tar.gz
```
32位：
```
~$ wget https://nodejs.org/dist/v8.10.0/node-v8.10.0-linux-x86.tar.gz
```
然后解压到当前目录，以64位为例：
```
~$ tar zxvf node-v8.10.0-linux-x64.tar.gz
```
然后接下来修改.bashrc来设置相关的环境变量：
```
~$ echo "export NODE_HOME=$HOME/node-v8.10.0-linux-x64" >> .bashrc
~$ echo "export NODE_PATH=$NODE_HOME/lib/node_modules" >> .bashrc
~$ echo "export PATH=$NODE_HOME/bin:$PATH" >> .bashrc
```
最后重新载入.bashrc（或者重新登陆）来使node生效：
```
~$ source .bashrc
```
现在，你可以使用node了：
```
~$ node –v
v8.10.0
```
## 2.2 安装节点仿真器
在终端执行以下命令：
```
~$ npm install –g ganache-cli
```
安装完毕后，执行命令验证安装成功：
```
~$ ganache-cli
Ganache CLI v6.0.3 (ganache-core: 2.0.2)
```
## 2.3 安装solidity编译器
```
~$ npm install –g solc
```
安装完毕后，执行命令验证安装成功
```
~$ solcjs –version
0.40.2+commit.3155dd80.Emscripten.clang
```
## 2.4安装web3
```
~$ npm install –g web3@0.20.2
```
安装验证：
```
~$ node –p 'require("web3")'
{[Function: Web3]
  providers:{…}}
```
## 2.5安装truffle框架
执行以下命令安装truffle开发框架：
```
~$ npm install –g truffle
```
验证安装：
```
~$ truffle version
Truffle v4.1.3 (core 4.1.3)
```
## 2.6安装webpack
执行以下命令安装webpack：
```
~$ npm install –g webpack@3.11.0
```
验证安装
```
~$ webpack –v
3.11.0
```
# 三、构建示例项目
## 3.1 新建DApp项目
执行以下命令创建项目目录并进入该目录：
```
~$ mkdir demo
~$ cd demo
```
然后用webpack模版初始化项目骨架结构：
```
~/demo$ truffle unbox webpack
Downloading…
Unpacking…
Setting up…
Unbox successful. Sweet!
```
## 3.2 安装项目依赖的NPM包
执行以下命令安装nmp包：
```
~/demo$ npm install
```
## 3.3 修改truffle配置
truffle.js中，修改port为8545，因为ganache-cli在8545端口监听：
```
module.exports = {
  networks:{
    development: {
      …
      port: 8545
      …
    }
  }
}
```
## 3.4 启动节点
执行以下命令启动节点仿真器，以便部署合约并执行交易：
```
~/demo$ ganache-cli
```
## 3.5 编译合约
执行以下命令编译项目合约：
```
~/demo$ truffle compile
```
## 3.6 部署合约：
执行以下命令来部署合约：
```
~/demo$ truffle migrate
```
## 3.7 启动DApp
执行以下命令来启动DApp:
```
~/demo$ npm run dev
```
在浏览器里访问http://localhost:8080即可
 
如果你希望从别的机器也可以访问你的DApp应用，修改一下package.json：
```
{
	scripts:{
		"dev": "webpack-dev-server –-host 0.0.0.0"
	}
}
```
