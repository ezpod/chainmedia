# windows下的ipfs安装部署与ipfs-api开发环境搭建
如果你不喜欢浪费时间折腾开发环境，可以使用汇智网的在线教程：

[以太坊电商DApp实战](http://xc.hubwiz.com/course/5abbb7acc02e6b6a59171dd6?affid=github7878)

# 一、ipfs节点安装与使用
## 1.1下载节点软件
到官网下载windows版的ipfs节点软件：[32位](https://dist.ipfs.io/go-ipfs/v0.4.14/go-ipfs_v0.4.14_windows-386.zip)，[64位](https://dist.ipfs.io/go-ipfs/v0.4.14/go-ipfs_v0.4.14_windows-amd64.zip)
如果你不能访问官网，可以使用百度云盘镜像：[32位](https://pan.baidu.com/s/1XivzokWIMIy9MwAUUpOBQg)，[64位](https://pan.baidu.com/s/1H9DRYZLKmGvdEzP0-DzjJA)
## 1.2解压节点软件
下载后解压到指定目录，例如d:\go-ipfs，开一个控制台窗口，测试：
```
D:\go-ipfs > ipfs version
Ipfs version 0.4.14
```
可以将该目录加入环境变量PATH，
 ![path env](https://img-blog.csdn.net/2018042113410117?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NoZWJhbzMzMzM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
或者将d:\go-ipfs\ipfs.exe拷贝到windows系统目录，以便在任何目录中可以启动ipfs.exe。
## 1.3 初始化本地仓库
和git类似，ipfs节点也需要先初始化一个本地仓库。执行init子命令来初始化本地仓库：
```
D:\go-ipfs> ipfs init
Initializing IPFS node at C:\Users\hubwiz\.ipfs
generating 2048-bit RSA keypair...done
peer identity: QmQaTgU1TLNHPBEvLGgWK1G9FgVByyUZNVhDs789uWPtku
to get started, enter:

     ipfs cat /ipfs/QmS4ustL54uo8FzR9455qaxZwuMiUhyvMcX9Ba8nUH4uVv/readme
```
默认情况下，ipfs将在当前用户主目录（例如：对于hubwiz用户，其主目录就是C:\Users\hubwiz）下建立.ipfs子目录，作为本地仓库的根目录。

如果你的C盘空间不够大，或者你就是希望使用其他目录作为本地仓库根目录，可以设置IPFS_PATH环境变量，使其指向目标路径，例如D:\my_ipfs_root
![ipfs-path](https://img-blog.csdn.net/20180421135057417?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NoZWJhbzMzMzM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
 
## 1.4重新初始化
如果你期望重新初始化节点，会提醒你不能这么做，否则会改写你的密钥：
```
D:\go-ipfs> ipfs init
Initializing IPFS node at C:\Users\hubwiz\.ipfs
Error: ipfs configuration file already exists!
Reinitializing would overwrite your keys.
```
这挡不住我们。如果你必须重新初始化的话，先删除原来的仓库根目录即可：
```
D:\go-ipfs> del C:\users\hubwiz\.ipfs
```
## 1.5将文件添加到本地仓库
使用add子命令将指定的文件添加到本地仓库，例如将当前目录的README.md文件添加到本地仓库：
```
D:\go-ipfs> ipfs add README.md
465 B / ? [-------------------------------------------------------=--] 
added QmXBpD37vBm5537pqHwyJRGSaX7hMrkHyp866wqEVU2BE8 README.md
```
ipfs会根据文件的内容生成一个哈希值，例如：
```
QmXBpD37vBm5537pqHwyJRGSaX7hMrkHyp866wqEVU2BE8
```
你需要记录下这个编码，因为需要使用它来访问本地仓库（或ipfs网络）中的文件。
注意：ipfs并不会无节制地将你本地仓库中的文件分布到其他ipfs节点中，如果没有其他的ipfs节点搜索你的文件（的哈希值），那么你本地仓库中的文件将始终只存在于本地。
## 1.6访问ipfs文件
Ipfs网络中只能通过内容的哈希值来访问文件，例如对于上面的README.md文件，我们使用cat子命令通过其哈希值来查看其内容：
```
D:\go-ipfs> ipfs cat QmXBpD37vBm5537pqHwyJRGSaX7hMrkHyp866wqEVU2BE8
```
控制台将输出内容：
```
\# ipfs commandline tool

This is the [ipfs](http://ipfs.io) commandline tool. It contains a full ipfs node.
......
```
## 1.7 将节点接入网络
执行daemon子命令将节点接入ipfs网络：
```
D:\go-ipfs> ipfs daemon
Initializing daemon...
......
Daemon is ready
```
只有当启动监听后，节点才能够接受ipfs网络中的内容检索请求，参与内容的交换与分布。

可以按Ctrl+C退出监听状态。
 
# 二、ipfs-api安装与使用
Ipfs节点提供和REST API接口，可供我们在程序代码中操作节点进行文件的上传等操作。不过大多数情况下，我们并不需要直接操作这个REST开发接口，而是使用经过封装的更友好的ipfs-api，一个nodejs包。
## 2.1安装nodejs
到官网下载nodejs安装包：[32位](https://nodejs.org/dist/v8.11.1/node-v8.11.1-x86.msi)，[64位](https://nodejs.org/dist/v8.11.1/node-v8.11.1-linux-x64.tar.xz)。下载后双击进行安装即可。

开一个控制台窗口，测试：
```
C:\Users\hubwiz> node -v
V8.11.1
```
## 2.2安装ipfs-api
Ipfs-api的安装需要git命令行，因此我们先安装git。从官网下载git安装包：[32位](https://github.com/git-for-windows/git/releases/download/v2.16.2.windows.1/Git-2.16.2-32-bit.exe)，[64位](https://github.com/git-for-windows/git/releases/download/v2.16.2.windows.1/Git-2.16.2-64-bit.exe)。下载后双击安装即可。

执行git命令测试：
```
D:\test-ipfs-api> git version
git version 2.16.2.windows.1
```
ipfs-api需要编译原生node模块，因此需要安装VisualStudio 2015和python27。

官网下载[VisualStudio 2015社区版](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15)，双击安装即可。
官网下载[Python27安装包](https://www.python.org/download/releases/2.7/)，双击安装并设置PATH环境变量使python可用。

重新开一个控制台，使环境变量生效。现在安装ipfs-api：
```
D:\test-ipfs-api> npm install ipfs-api
+ ipfs-api@20.0.1
added 1 package, updated 1 package and moved 1 package in 22.138s
```
## 2.3测试代码 – nodejs
在D:\test-ipfs-api目录下创建一个测试脚本test.js：
```
const ipfsAPI = require('ipfs-api')
const ipfs = ipfsAPI('localhost', '5001', {protocol: 'http'})
const buffer = Buffer.from('this is a demo')
ipfs.add(buffer)
    .then( rsp => console.log(rsp[0].hash))
.catch(err => console.error(err))
```
执行这个脚本：
```
D:\test-ipfs-api> node test.js
QmfQS4vm9YZTAyGZEkDqm81xripwsK3NgqfNkbCdoeEw5i
```
也就是说，我们将内容`this is a demo`添加到本地仓库后，得到哈希值
`QmfQS4vm9YZTAyGZEkDqm81xripwsK3NgqfNkbCdoeEw5i`。现在可以使用cat子命令来查看这个哈希值对应的内容：
```
D:\test-ipfs-api> ipfs cat QmfQS4vm9YZTAyGZEkDqm81xripwsK3NgqfNkbCdoeEw5i                                                      
```
控制台会输出我们之前上传的内容：
```
this is a demo
```
ipfs进入监听状态后，提供了一个http网关，让我们可以使用浏览器来访问ipfs上的内容。网关默认在本机（127.0.0.1）的8080端口监听，因此使用你的浏览器访问这个URL：
```
http://127.0.0.1:8080/ipfs/QmfQS4vm9YZTAyGZEkDqm81xripwsK3NgqfNkbCdoeEw5i
```
同样可以看到我们之前上传的内容。
注意：需要首先启动监听器（ipfs daemon）并且你的浏览器和ipfs节点在同一台计算机。
 
# 三、在浏览器中访问ipfs
ipfs-api也支持在browser使用。最简单的方法是使用专门针对浏览器的封装库，在html中引用即可：
```
<script src="https://unpkg.com/ipfs-api/dist/index.js"></script>
```
这个特别封装的库会创建一个全局对象ipfsAPI，我们在浏览器脚本中可以直接使用，例如：
```
var ipfs = window.IpfsApi('localhost', '5001')
```
这种方法比较简单，因此下文不再描述。接下来我们将使用更加工程化的方法，
采用webpack来直接在前端脚本中使用ipfs-api的nodejs包。
## 3.1 HTML页面
在D:\test-ipfs-api目录下创建index.html：
```
<!doctype html>
<html>
<head>
  <meta charset="utf-8">
</head>
<body>
  <textarea id="content">THIS IS ANOTHER DEMO</textarea>
  <button id="upload">Upload</button>
  <script src="./bundle.js"></script>
</body>
</html>
```
我们的目标是，当点击按钮时，我们将文本框的内容上传到ipfs
## 3.2前端脚本
在D:\test-ipfs-api目录下编写脚本app.js：
```
import ipfsAPI from 'ipfs-api'
const ipfs = ipfsAPI('localhost', '5001', {protocol: 'http'})

window.addEventListener('load', function() {
  let btn = document.querySelector('#upload')
let txt = document.querySelector('#content')
btn.addEventListener('click',()=>{
    let buffer = Buffer.from(txt.value, 'utf-8');
    ipfs.add(buffer)
      .then( rsp => console.log(rsp[0].hash))
.catch(err => console.error(err))
})  
})
```
## 3.3 webpack配置
在D:\test-ipfs-api目录下编写配置文件webpack.config.js：
```
const webpack = require('webpack');
const path = require('path');
const CopyWebpackPlugin = require('copy-webpack-plugin');

module.exports = {
  entry: './app.js',
  output: {
    path: path.resolve(__dirname),
    filename: 'bundle.js'
  },
  module: {
    loaders: [
      {
        test: /\.js$/,
        exclude: /(node_modules|bower_components)/,
        loader: 'babel-loader',
        query: {
          presets: ['es2015'],
          plugins: ['transform-runtime']
        }
      }
    ]
  }
}
```
## 3.4前端脚本打包
执行webpack打包：
```
D:\test-ipfs-api> webpack
```
## 3.5 配置ipfs的CORS策略
由于需要从网页中访问ipfs节点，这就引入了跨域安全问题，因此我们需要配置ipfs节点使其允许跨域请求：
```
D:\>ipfs config --json API.HTTPHeaders.Access-Control-Allow-Origin '["*"]'
```
## 3.6 配置ipfs的API监听地址
由于ipfs节点默认在本机（127.0.0.1）的5001端口监听API请求，因此如果你的浏览器和ipfs节点不在同一台机器上，需要让ipfs节点监听公开地址：
```
D:\> ipfs config --json Addresses.API '"/ip4/0.0.0.0/tcp/5001"'
```
当然，如果你的浏览器和ipfs节点在同一台机器上，就不需要进行这个配置了。
## 3.7配置ipfs的网关的监听地址
由于ipfs节点旳http网关默认在本机（127.0.0.1）的8080端口监听http请求，因此如果你的浏览器和ipfs节点不在同一台机器上，就需要让ipfs网关监听公开地址：
```
D:\> ipfs config --json Addresses.Gateway '"/ip4/0.0.0.0/tcp/8080"'
```
## 3.8测试网页
首先启动ipfs监听：
```
D:\>ipfs daemon
```
然后在测试目录下启动web服务器，这里使用python内置的简单服务器，当然你可以使用任何熟悉的web服务器：
```
D:\test-ipfs-api> python –m SimpleHTTPServer
Serving HTTP on 0.0.0.0 port 8000 ...
```
现在打开你的浏览器，访问http://127.0.0.1:8000/，一切顺利的话，你可以看到一个文本框和一个按钮，点击按钮，即可将文本框的内容上传到ipfs节点。

本文的中文pdf电子书可在百度云盘下载：https://pan.baidu.com/s/1QjnqQhj_Az11iZSJUFDH-Q
