# 以太坊DApp私链开发环境搭建 - windows - geth

如果你不喜欢浪费时间在开发环境的搭建上，可以使用汇智网的在线教程：

[以太坊DApp开发实战入门](http://xc.hubwiz.com/course/5a952991adb3847553d205d1?affid=github7878)

# 一、安装DApp开发环境
## 1.1 安装Node.js
我们使用官方长期支持的8.10.0LTS版本，点击这个链接下载32位安装包，32位安装包即可用于32位系统，也可用于64位系统。
如果你确认你的系统是64位，也可以下载64位包装包。
下载后直接安装即可。安装完毕，打开一个控制台窗口，可以使用node了：
```
C:\Users\hubwiz> node –v
v8.10.0
```
## 1.2 安装Geth
下载64位或32位Geth安装程序，然后进行安装。
安装完毕后打开一个控制台，执行命令验证安装成功：
```
C:\Users\hubwiz> geth version
Geth
Version: 1.8.3-stable
```
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
Web3的安装过程使用了git，因此需要先安装windows版的git命令行。下载64位或32位的git安装程序，本地安装后在继续安装web3。
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
验证安装
```
C:\Users\hubwiz> webpack –v
3.11.0
```
 
# 三、运行私链节点
## 2.1创世块配置
创建一个节点目录node1，并在其中创建私链的创世块配置文件：
```
C:\Users\hubwiz> mkdir node1
C:\Users\hubwiz> cd node1
C:\Users\hubwiz\node1> notepad private.json
```
然后编辑内容如下：
```
{
    "config": {
        "chainId": 7878,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
    "difficulty": "200",
    "gasLimit": "2100000",
    "alloc": {
        "7df9a875a174b3bc565e6424a0050ebc1b2d1d82": { "balance": "300000" },
        "f41c74c9ae680c1aa78f42e5647a62f353b7bdde": { "balance": "400000" }
    }
}
```
- `config.chainId`用来声明以太坊网络编号，选择一个大于10的数字即可。
- `difficulty`用来声明挖矿难度，越小的值难度越低，也就能更快速地出块。
## 2.2初始化私链节点
执行geth的init命令初始化私链节点：
```
C:\Users\hubwiz\node1> geth --datadir .\data init private.json
```
这会在当前目录下创建data目录，用来保存区块数据及账户信息：
```
C:\Users\hubwiz\node1> dir
data private.json
```
可以上述命令写到一个脚本init.cmd里，这样避免每次都输入那么多记不住的东西。文件内容如下：
```
geth --datadir .\data init private.json
```
在部署下一个节点时，就可以直接执行这个脚本进行初始化了。例如，在另一台机器上：
```
C:\Users\hubwiz\node1> init.cmd
```
## 2.3启动私链节点
从指定的私链数据目录启动并设定一个不同的网络编号来启动节点：
```
C:\Users\hubwiz\node1> geth --rpc --datadir .\data --networkid 7878 console
```
同样，你可以用一个脚本console.cmd来简化启动节点时的输入,文件内容如下：
```
geth --rpc \
      --rpcaddr 0.0.0.0 \
      --rpccorsdomain "*" \
      --datadir ./data \
      --networkid 7878 \
      console
```
- `rpcaddr`参数用来声明节点RPC API的监听地址，设为0.0.0.0就可以从其他机器访问API了；
-  `rpccorsdomain`参数是为了解决web3从浏览器中跨域调用的安全限制问题。
以后启动节点，只要直接执行这个脚本即可：
```
C:\Users\hubwiz\node1> console.cmd
```
## 2.4 账户管理
### 2.4.1 查看账户列表
在geth控制台，使用eth对象的accounts属性查看目前的账户列表：
```
> eth.accounts
[]
```
因为我们还没有创建账户，所以这个列表还是空的。
### 2.4.2创建新账户
在geth控制台，使用personal对象的newAccount()方法创建一个新账户，参数为你自己选择的密码：
```
> personal.newAccount('78787878')
0xd8bcf1324d566cbec5d3b67e6e14485b06a41d49
```
输出就是新创建的账户地址（公钥），你的输出不会和上面的示例相同。geth会保存到数据目录下的keystore文件中。密码要自己记住，以后还需要用到。
### 2.4.3查询账户余额
在geth控制台，使用personal对象的getBalance()方法获取指定账户的余额，参数为账户地址：
```
> eth.getBalance(eth.accounts[0])
0
```
或者直接输入账户地址：
```
> eth.getBalance('0xd8bcf1324d566cbec5d3b67e6e14485b06a41d49')
0
```
新创建的账户，余额果然为0。
### 2.4.4挖矿
没钱的账户什么也干不了，需要挖矿来挣点钱。
在geth控制台执行miner对象的start()方法来启动挖矿：
```
> miner.start(1)
```
等几分钟以后，检查账户余额：
```
> eth.getBalance(eth.accounts[0])
2.695e+21
```
钱不少了，2695ETH了，目前市值将近500万人民币了，哈。
执行miner对象的stop()方法停止挖矿：
```
> miner.stop()
```
### 2.4.5解锁账户
在部署合约时需要一个解锁的账户。在geth控制台使用personal对象的unlockAccount()方法来解锁指定的账户，参数为账户地址和账户密码（在创建账户时指定的那个密码）：
```
> eth.unlockAccount(eth.accounts[0],'78787878')
true
```
 
# 三、构建示例项目
## 3.1 新建DApp项目
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
## 3.2 安装项目依赖的NPM包
执行以下命令安装nmp包：
```
C:\Users\hubwiz\demo$ npm install
```
## 3.3 修改truffle配置
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
## 3.4 启动节点
执行以下命令启动节点仿真器，以便部署合约并执行交易：
```
C:\Users\hubwiz\node1> console.cmd
```
注意：为了在节点上部署合约，别忘了启动geth后先解锁账户：
```
> personal.unlockAcount(eth.accounts[0],'78787878')
true
```
## 3.5 编译合约
执行以下命令编译项目合约：
```
C:\Users\hubwiz\demo> truffle.cmd compile
```
## 3.6 部署合约
执行以下命令来部署合约：
```
C:\Users\hubwiz\demo> truffle.cmd migrate
```
如果你之前忘了在geth控制台解锁账户，会看到如下错误，参考前面说明进行解锁即可：
```
Error: authentication needed: password or unlock
```
如果已经正确地解锁了账户，你会看到部署过程停止在如下状态：
```
Replacing Migrations…
… 0x3088762a5bc9…
```
这是因为truffle在等待部署交易提交，但是我们在私链中还没有启动挖矿。
现在切换回geth终端窗口，查看交易池的状态：
```
> txpool.status
{
  pending:1,
  queued:0
}
```
果然有一个挂起的交易！启动挖矿就是了：
```
> miner.start(1)
```
稍等小会儿，再查看交易池的状态：
```
> txpool.status
{
  pending:0,
  queued:0
}
```
交易已经成功提交了。我们可以停止挖矿了，因为它太占CPU了：
```
> miner.stop()
```
现在切换回truffle那个终端，部署过程也正确地执行完了。
## 3.7 启动DApp
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
