# windows下的以太坊DApp开发环境搭建

如果你不喜欢把时间浪费在搭建开发环境上，可以使用汇智网的在线练习环境及教程：

[以太坊DApp实战开发入门](http://xc.hubwiz.com/course/5a952991adb3847553d205d1?affid=github7878)


# 一、安装DApp开发环境
## 1.1 安装Node.js
我们使用官方长期支持的8.10.0LTS版本，点击这个[链接](https://nodejs.org/dist/v8.10.0/node-v8.10.0-x86.msi)下载32位安装包，32位安装包即可用于32位系统，也可用于64位系统。
如果你确认你的系统是64位，也可以下载[64位包装包](https://nodejs.org/dist/v8.10.0/node-v8.10.0-x64.msi)。
下载后直接安装即可。安装完毕，打开一个控制台窗口，可以使用node了：
```
C:\Users\hubwiz> node –v
v8.10.0
```
## 1.2 安装节点仿真器
在控制台执行以下命令：
```
C:\Users\hubwiz> npm install –g ganache-cli
```
安装完毕后，执行命令验证安装成功：
```
C:\Users\hubwiz> ganache-cli
Ganache CLI v6.0.3 (ganache-core: 2.0.2)
```
如果你是Win10，也可以下载预编译的Win10软件包，安装图形版的ganache。
## 1.3 安装solidity编译器
```
C:\Users\hubwiz> npm install –g solc
```
安装完毕后，执行命令验证安装成功
```
C:\Users\hubwiz> solcjs –version
0.40.2+commit.3155dd80.Emscripten.clang
```
## 1.4安装web3
```
C:\Users\hubwiz> npm install –g web3@0.20.2
```
安装验证：
```
C:\Users\hubwiz> node –p 'require("web3")'
{[Function: Web3]
  providers:{…}}
```
## 1.5安装truffle框架
执行以下命令安装truffle开发框架：
```
C:\Users\hubwiz> npm install –g truffle
```
验证安装：
```
C:\Users\hubwiz> truffle.cmd version
Truffle v4.1.3 (core 4.1.3)
```
## 1.6安装webpack
执行以下命令安装webpack：
```
C:\Users\hubwiz> npm install –g webpack@3.11.0
```
验证安装：
```
C:\Users\hubwiz> webpack –v
3.11.0
```

# 二、构建示例项目
## 2.1 新建DApp项目
执行以下命令创建项目目录并进入该目录：
```
C:\Users\hubwiz> mkdir demo
C:\Users\hubwiz> cd demo
```
然后用webpack模版初始化项目骨架结构：
```
C:\Users\hubwiz\demo> truffle.cmd unbox webpack
Downloading…
Unpacking…
Setting up…
Unbox successful. Sweet!
```
## 2.2 安装项目依赖的NPM包
执行以下命令安装nmp包：
```
C:\Users\hubwiz\demo> npm install
```
## 2.3 修改truffle配置
如果你使用图形版的ganache，不需要修改truffle.js配置文件。否则，需要在truffle.js中，修改port为8545，因为ganache-cli在8545端口监听：
```
module.exports = {
  networks:{
    development: {
      port: 8545
    }
  }
}
```
## 2.4 启动节点
执行以下命令启动节点仿真器，以便部署合约并执行交易：
```
C:\Users\hubwiz\demo> ganache-cli
```
## 2.5 编译合约
执行以下命令编译项目合约：
```
C:\Users\hubwiz\demo> truffle.cmd compile
```
## 2.6 部署合约：
执行以下命令来部署合约：
```
C:\Users\hubwiz\demo> truffle.cmd migrate
```
## 2.7 启动DApp
执行以下命令来启动DApp:
```
C:\Users\hubwiz\demo> npm run dev
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
