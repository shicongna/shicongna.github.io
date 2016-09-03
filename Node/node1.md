# node语法
### npm是一个node包管理器，完全由Javascript实现的命令行工具，通过Node.js执行
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
********
### json:js对象表示法，数据格式(对象，数组，字符串......)
### package.json: 记录依赖关系；     
+ 如  npm install --save-dev express/jade/body-parser/mongodb......

+ 语法：{
    "name":"my-web-site",
    "version":"0.0.0"
}

### webstorm:javascript开发环境/工具(编辑器--node)
### 模板引入继承（header&&footer公共部分)
```
    extends layout
    block content
```

********
###mongodb（数据库）
+ mongodb 一个基于分布式文件存储的数据库:
+ mongod --dbpath=e:/db   ~~~~~~启动数据库（新建）
+ mongodb下载一般存放地址:
    windowC盘-program Files-Mongodb-sever-3.2-bin- (环境变量)
+ mongo    ~~~~~~connect test 数据库连接成功
+ 操作数据库：增 删 改 查
+ 客户端服务器与数据库连接操作数据库(在node环境下)
 ```
 var MongoClient = require("mongodb").MongoClient;
var url = "mongodb://127.0.0.1:27017/mydb";
MongoClient.connect(url,function(err,db) {
    var col = db.collection("students");
    col.find().toArray(function (err, docs) {
        console.log(docs);
        db.close();
    })
    var student = {
        "name": "xiaoming",
        "age": 15,
        "sex": "male"
    };
    // col.insertOne(student,function(){
    //     db.close();
    // })
})
 ```

 ********

 ### 服务器(sever.js)
 #### 前台请求：
 + a href=" "
 + 表单form action=" "
 + 原生ajax请求：XMLHttpRequest(浏览器内置对象，发送请求)
 + $.ajax({ })
 
 #### 后台接收响应：

 ##### 后台接收：
 + get方法 req.query
 + post方法 req.body(依赖body-parser模块)
 + 
 ##### 后台响应：
 + res.send("a")(依赖express模块)
 + res.end("a")
 + res.render("home")(依赖jade模块)
 + res.redirect("/home")
 
 ********
 ###前后台数据交互
 ```
 sever.js
 var express = require("express");
var path = require("path");
var bodyParser = require("body-parser");
var datahandle = require("./my_modules/datahandle");
var app = express();
app.use(express.static(path.join(__dirname,"public")));
app.use(bodyParser.urlencoded({extended:false}));
app.set("view engine","jade");
app.listen(3000,function(){
    console.log("ok");
})

app.get("/",function (req,res) {
    datahandle.finddata(function(data){
        res.render("index",{data:data});
    })
})
app.post("/insertdata",function(req,res){
    var data = req.body;
    datahandle.insertdata(data,function(){
        res.redirect("/");
    })
})

app.post("/removedata",function(req,res){
    var data = req.body;
    datahandle.removedata(data,function(){
        res.redirect("/");
    })
})

app.post("/updatedata",function(req,res){
    var data = req.body;
    var condition = {"number":data.number}
    datahandle.updatedata(condition,data,function(){
        res.redirect("/");
    })
})
datahandle.js:
var MongoClient = require("mongodb").MongoClient;
var url = "mongodb://127.0.0.1:27017/mytest";
function insertdata(options,next){
    MongoClient.connect(url,function(err,db){
        db.collection("teachers").insertOne(options,function(){
            next();
            db.close();
        })
    })
}

function  updatedata(condition,options,next){
    MongoClient.connect(url,function(err,db){
        db.collection("teachers").updateOne(condition,options,function(){
            next();
            db.close();
        })
    })
}

function removedata(condition,next){
    MongoClient.connect(url,function(err,db){
        db.collection("teachers").removeOne(condition,function(){
            next();
            db.close();
        })
    })
}

function finddata(next){
    MongoClient.connect(url,function(err,db){
        db.collection("teachers").find().toArray(function(err,docs){
            next(docs);
            db.close();
        })
    })
}
module.exports = {
    insertdata:insertdata,
    updatedata:updatedata,
    removedata:removedata,
    finddata:finddata
}
 ```
 ********
 ### cookie:进行session跟踪而存储在用户本地终端上的数据
```
var express = require("express");
var cookieParser = require("cookie-parser");
var app = express();
app.use(cookieParser());
app.listen(3000,function(){
    console.log("ok");
})

app.get("/",function (req,res) {
    if(req.cookies.username){
        res.send("再次访问")
    }else{
        res.cookie("username","teacherLi");
        res.send("欢迎第一次访问");
    }
})
```
### session:对象存储特定用户会话所需的属性及配置信息(服务器端)
```
var express = require("express");
var session = require("express-session");
var app = express();
app.use(session({
    "secret":"hello",
    "cookie":{maxAge:600 * 1000}
}));
app.listen(3000,function(){
    console.log("ok");
})
app.get("/",function (req,res) {
    if(req.session.money){
        req.session.money++;
        var str = "欢迎第" + req.session.money + "次访问";
        res.send(str);
    }else{
        req.session.money = 1;
        res.send("欢迎第1次访问")
    }
})
session保持登录(综合)
var express = require("express");
var path = require("path");
var session = require("express-session");
var app = express();
app.set("view engine","jade");
app.use(express.static(path.join(__dirname,"public")));
app.use(session({
    "secret":"test",
    "cookie":{maxAge:10*1000}
}))
app.listen(3000,function () {
    console.log("服务器已经启动");
})
app.get("/",function (req,res) {
    res.render("login");
})
app.post("/login",function(req,res){
    req.session.username = "lee";
    res.redirect("/home");
})
app.get("/home",function (req,res) {
    if(req.session.username){
        res.render("home",{user:req.session.username})
    }else{
        res.redirect("/")
    }
})

```
********
### 后台验证用户返回信息
```
var express = require("express");
var path = require("path");
var bodyParser = require("body-parser");
var app = express();
app.use(express.static(path.join(__dirname,"public")));
app.use(bodyParser.urlencoded({extended:false}));
app.set("view engine","jade");
app.listen(3000,function(){
    console.log("ok");
})
app.get("/",function(req,res){
    var h3info = req.query.showinfo;
    res.render("index",{info:h3info});
});

app.post("/checkqwer",function(req,res){
    var postinfo = req.body.logininfo;
    if(postinfo === "qwer"){
        res.render("test")
    }else{
        res.redirect("/?showinfo=登录失败")
    }
})
{jade中与上logoininfo对应：
form(action="/checkqwer",method="post")
        input(name="logininfo")
        button 登录
    h3 #{info}
}
```
### 数据库验证用户回调设置
```
sever.js:
var checkuser = require("./my_modules/checkuser");
var user = {"username":"aaa","password":"111"};
checkuser.checkuser(user,function(result){
    console.log(result)
    if(result === 0){
        console.log("用户名不存在");
    }else if(result ===1){
        console.log("密码错误");
    }else{
        console.log("登录成功");
    }
})

checkuser.js:
var MongoClient = require("mongodb").MongoClient;
var url = "mongodb://127.0.0.1:27017/checkuser";

function checkuser(options,next){
    //0 用户名不存在
    //1 密码错误
    //2 登录成功
    MongoClient.connect(url,function(err,db){
        var condition = {"username":options.username};
        db.collection("users").find(condition).toArray(function(err,docs){
            console.log(err);
            db.close();
            if(docs.length == 0){
                next(0);
            }else if(docs[0].password === options.password){
                next(2);
            }else {
                next(1);
            }
        })
    })
}

module.exports = {
    checkuser:checkuser
}

```
********
### ajax:AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。
AJAX 不是新的编程语言，而是一种使用现有标准的新方法。
AJAX 是与服务器交换数据并更新部分网页的艺术，在不重新加载整个页面的情况下。

###原生ajax请求：
```
    <button id="get">get请求</button>
    <button id="post">post请求</button>
    <script>

        document.querySelector("#get").onclick = function(){
            myAjax("get","/hello",function(data){
                alert(data);
            })
        }

        document.querySelector("#post").onclick = function(){
            myAjax("post","/hello",function(data){
                alert(data);
            })
        }

        function myAjax(method,url,next){
            var xhr = new XMLHttpRequest();
            xhr.open(method,url);
            xhr.send();
            xhr.onreadystatechange = function(){
                if(xhr.readyState == 4 && xhr.status == 200){
                    next(xhr.responseText);
                }
            }
        }
```
### jquery中的ajax方法:
```
    <button id="get">get请求</button>
    <button id="post">post请求</button>
    <button id="ajax">ajax方法</button>

    <script src="script/jquery.min.js"></script>
    <script>
        $("#get").click(function(){
            $.get("/hello",{"username":"li","password":"aaa"},function(data){
                alert(data);
            })
        })
        $("#post").click(function(){
            $.post("/hello",{"username":"li","password":"aaa"},function(data){
                alert(data);
            })
        })

        $("#ajax").click(function(){

            $.ajax({
                "method":"get",
                "url":"/hello",
                "data":{"username":"li","password":"aaa"}
            }).done(function(data){
                alert(data);
            }).fail(function(){
                alert("请求失败！");
            })

        })
    </script>
```
### 提交表单：
```
    <form id="myform">
        <input type="text" name="number">
        <input type="text" name="name">
        <button>提交表单</button>
    </form>

    <script src="script/jquery.min.js"></script>
    <script>
        $("#myform").submit(function(){
            var data = $(this).serialize();  //表单序列化
            $.ajax({
                method:"post",
                url:"/insertdata",
                data:data
            }).done(function(data){
                console.log(data);
            })
            return false;
        })
    </script>
```
********
### 跨域请求:
+ jsonp协议
```
index.html:
<h1>hello</h1>
<script src="script/jquery.js"></script>
<script>
    $.ajax({
        "method":"get",
        "url":"http://127.0.0.1:2000/getdata",
        "dataType":"jsonp"(前台jquery写好的jsonp协议)
    }).done(function(data){
        console.log(data)
    }).fail(function(){
        console.log("请求失败")
    })
</script>

server.js:
var express=require("express");
var path=require("path")
var app=express();
app.use(express.static(path.join(__dirname,"public")));
app.get("/getdata",function(req,res){
    res.jsonp("跨域请求数据成功")~~~~~~(后台返回node写好的jsonp)
})
app.listen(2000,function(){
    console.log("服务器已启动");
})
```
+ 响应头
```
var express = require("express");
var path = require("path");
var app = express();
app.use(express.static(path.join(__dirname,"public")));
app.listen(3000,function () {
    console.log("服务器已经启动");
})

app.get("/getdata",function(req,res){
    res.setHeader('Access-Control-Allow-Origin', "http://127.0.0.1:2000");
    res.send("成功获取数据")
})
```
+表单提交 action="http://www.baidu.com"








