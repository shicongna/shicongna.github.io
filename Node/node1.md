# node语法
### npm是一个node包管理器，完全由Javascript实现的命令行工具，通过Node...js执行
### Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。Node.js 使用了一个事件驱动、非阻塞式 I/O 的模型，使其轻量又高效。Node.js 的包管理器 npm，是全球最大的开源库生态系统。
********
###Node.js模块系统(模块类型)
为了让Node.js的文件可以相互调用，Node.js提供了一个简单的模块系统。
模块是Node.js 应用程序的基本组成部分，文件和模块是一一对应的。换言之，一个 Node.js 文件就是一个模块，这个文件可能是JavaScript 代码、JSON 或者编译过的C/C++ 扩展。
+ 自定义模块
```
module1：
var num1 = 10;
var num2 = 99;
function add(){
    return num1 + num2;
}
function sub(){
    return num1 - num2;
}

module.exports = {
    add:add,
    sub:sub
};
module2:
function fun(num1,num2,callback){
    setTimeout(function(){
        var result = num1 + num2;
        callback(result);
    },1000)

}
module.exports = {
    fun:fun
}
main:
var module1 = require("./module1");
var module2 = require("./module2");
var num = 1000;
module2.fun(999,11,function(result){
    console.log(result);
})
console.log("//////////////////////////////");
console.log("1");
console.log("2");
console.log("3");
console.log("//////////////////////////////");
```
+ 核心模块
```
var http = require("http");
http.createServer(function(req,res){
    res.end("你好");
}).listen(80,"192.168.3.199",function(){
    console.log("服务器已经启动")
})
```
+ 第三方模块
```
var express = require("express");
var path = require("path");
var app = express();
app.use(express.static(path.join(__dirname,"public")));
app.listen(3000,function(){
    console.log("服务器成功启动")
})
```