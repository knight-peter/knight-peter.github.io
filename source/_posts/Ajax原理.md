title: Ajax原理
author: 徐子玉
tags:
  - ajax
  - javascript
categories:
  - 前端
date: 2018-02-13 16:55:00
---
# Ajax
## 前端相关的技术点
- html 主要用来实现页面的排版布局
- css 主要用来实现页面的样式美化
- javaScript 主要用来实现前端功能特效

> 采用上面的这些技术开发的页面和前端特效脚本需要放到服务器才能够对外提供服务，才能够让互联网上的网友看到。
<!-- more -->
## 客户端与服务器

> 本质上都是计算机，只不过样子不同，配置不同，应用场景不同（安装的应用软件不同） 

- 客户端主要用于普通上网用户
- 服务器主要给上网用户提供后台服务

## 网络相关概念
- IP地址（唯一的确定互联网上的一台计算机）
- 域名 IP地址的别名，方便记忆
- DNS 用于维护IP地址与域名的关系
- 端口 用来确定计算机上的网络应用程序

## 通信协议理解

> 通信双方约定的规则

- http/https 超为本传输协议
- ftp 文件传输协议
- smpt/pop3 邮件收发协议
- ......

## 网站

> 网站由一系列页面组成（页面由js、css、图片、html标签。。。所有的这些文件都被称为资源）

### 静态网站

> 就是提前写好的html页面（包括图片、媒体文件。。。静态资源文件），并且部署到服务器上

- 静态网站主要存在的问题：
    - 随着网站规模的增大可维护性逐渐降低
    - 没有交互性

### 动态网站

> 动态指的是html页面是动态生成的，这里动态生成的不一定是一个完整的页面，有可能仅仅是页面的一部分，或者仅仅是数据(普通字符串、json、xml)

- 实现动态网站的技术：
    - php
    - java（jsp）
    - .net
    - Node.js
    - python
    - ......
### http 协议

    get       |     用来获取数据
    post      |     用来添加数据
    put       |     用来修改数据
    delete    |     用来删除数据
    
> http 协议的常用请求方式: **增删改查**


## PHP基础语法
    <?php PHP的所有语法都写在这个地方 ?>
- PHP文件文件格式是 `xxx.php`;

### 变量
> 变量以$开头 字母/数字/下划线 不能以数字开头，大小写敏感。

### 字符串拼接
> PHP中字符串拼接用" **.** ";

### 单引号 ###
> PHP中的单引号把包含在其中的变量当作为普通的字符串来处理;

### 双引号 ###
> PHP中的双引号把包含在其中的变量当作变量解析成变量值;

### 运算符 ###
> 与javaScript基本类似;

### 数据类型
- 字符串
- 整型
- 浮点型
- 布尔型
- 数组
- 对象
- NULL
> gettype: 是内置数据类型的判断

### 输出内容
- **echo:** 输出简单数据类型, 如字符串, 数值;
- **print_r():** 输出复杂数据类型, 如数组;
- **var_dump** 输出详细信息, 如对象, 数组;

### 分之循环
- if/switch
- while
- for
    - count()内置函数, 计算数组的长度;
- foreach
    - foreach($arr as $key => $value){ };

### 函数
- **自定义函数**
    - 语法类似与JavaScript的自定义函数;
- **系统函数**
    - 直接调用, 不需要声明;
    - `json_encode();` 将数组和对象转换为字符串的方法;
### 预定义变量
- **$_GET**
    - $_GET请求参数获取
    - $_GET是专门用来接数据用的一个全局数组;
    - form 默认请求方式就是get请求, get请求会把表单数据作为url的参数进行提交;
- **$_POST**
    - $_POST请求参数获取
    - 设置服务响应的文件类型:
       1. header("Content-Type: text/plain; charset=utf-8");
       2. header("Content-Type: text/html; charset=utf-8");
    
    - $_POST也是PHP内置好的专门用来接数据用的一个全局数组;

- **GET与POST请求方式的差异**
    
1. GET没有请求主体, 使用`xhr.send(null)`;
2. GET可以通过在请求URL上添加请求参数;
3. POST有请求主体, 可以通过;`xhr.send(name=itcast=&age=10)`;
4. POST需要设置请求头;
5. GET效率更好(应用多);
6. GET大小限制约4K, POST则没有限制;
7. 与 POST 相比, GET 个更简单也更快,并且在大部分情况下都能用。
8. 然而，在以下情况中，使用 POST 请求：
    -  无法使用缓存文件。
    -  向服务器发送大量数据（POST 没有数据量限制）
    -  发送包含位置字符的用户输入时，POST 比 GET 更稳定也更可靠。



### 案例
> HTML
    

    <div>
        <form action="./page6-data.php" method="post">
            考号：<input type="text" name="code"><input type="submit" value="查询">
        </form>
    </div>


> PHP 文件名:page6-data.php
    
    <?php 
        / 服务器端渲染页面
        arr = array();
        arr['123'] = array("username"=>"张三","chinese"=>"130","math"=>"149","english"=>"146","summary"=>"298");
        arr['124'] = array("username"=>"李四","chinese"=>"100","math"=>"140","english"=>"136","summary"=>"298");
        arr['125'] = array("username"=>"王五","chinese"=>"90","math"=>"139","english"=>"126","summary"=>"298");
        arr['126'] = array("username"=>"赵六","chinese"=>"30","math"=>"50","english"=>"80","summary"=>"100");
        code = $_POST['code'];
        f($code == 'admin'){
            foreach($arr as $value){
                echo "<ul>
                    <li>姓名：$value[username]</li>
                    <li>语文：$value[chinese]</li>
                    <li>数学：$value[math]</li>
                    <li>英语：$value[english]</li>
                    <li>综合：$value[summary]</li>
                </ul>";
           }
        else{
            $score = $arr[$code];
            echo "<ul>
                <li>姓名：$score[username]</li>
                <li>语文：$score[chinese]</li>
                <li>数学：$score[math]</li>
                <li>英语：$score[english]</li>
                <li>综合：$score[summary]</li>
            </ul>";
        }
    ?>
### 后台接口
1. 将数组和对象转换为字符串的方法 --- json_encode();
2. 将字符串转换为对象的方式 --- json_decod();
3. 接口说白了就是后台返回特定格式数据, 而不是一个完整的页面, 就是从后台到前台返回一些数据;

### 请求参数分析
>  **open();**

    xhr.open("get","nemo.php?username="+uname+"&password="+pwd,true);
    
- 调用open方法并不会真正发送请求, 而是启动一个请求以备发送.
- 它接受三个参数: 
    - 参数一: 请求方式(get获取数据, post提交数据);
    - 参数二: 请求地址;
    - 参数三: 同步或者异步标识位, 默认是 true, true表示异步, false 表示同步;
- **注意：** 如果是get请求， 那么请求参数必须在url中传递， 那么就用`encodeURI（）` 进行编码，防止乱码。

> **send();**

- 如果要发送请求， 用send()方法。
- 要发送特定的请求，需要调用send（）方法。它接收一个参数，即要作为请求主体发送的数据。
- get请求，则必须传入null。post请求传入字符串。
- get请求是只有头部，没有主体的，而post请求有请求主体。
- **注意：**post请求参数通过send传递，不需要通过`encodeURI()`转码,但是必须需要设置请求参数.

### 创建 xhr  对象
    1. 标准: xhr = new XMLHttprequest();
    2. IE6: xhr = newActiveXObject('Microsoft.XMLHTTP");

- xhr 对象有一个重要的属性, 就是 readyState 属性,表示"就绪状态", 就是 `xhr.readyState`。
- readyState 取值有5种值： 0、1、2、3、4

         0  -----  未初始化。
         1  -----  XMLHttpRequest对象正在加载。
         2  -----  xmlHttp对象加载完毕。
         3  -----  正在传输数据。
         4  -----  全部完成。

- 只要 readyState改变后，就会触发一个事件 `onreadystatechange` 事件.

            xhr.onreadyStatechange = function(){};

- Ajax-但用senp方法发出HTTP请求之后,判断ceadyState得到的时候,就会有一个属性`xhr.status`表示的是请求的文件的状态码.
        
        1. 1**  -----   消息.
        2. 200  -----   代码请求成功.
        3. 3**  -----   重定向.
        4. 404  -----   请求错误.
        5. 500  -----   服务器错误.
        6. 6**  -----   其他.

### 案例
> HTML

    <script type="text/javascript">
        window.onload = function(){
            var btn = document.getElementById('btn');
            btn.onclick = function(){
                var uname = document.getElementById('username').value;
                var pw = document.getElementById('password').value;
                // 1、创建XMLHttpRequest对象
                var xhr = null;
                if(window.XMLHttpRequest){
                    xhr = new XMLHttpRequest();//标准
                }else{
                    xhr = new ActiveXObject("Microsoft");//IE6
                }
                // 2、准备发送
                /*
                    参数一：请求方式（get获取数据；post提交数据）
                    参数二：请求地址
                    参数三：同步或者异步标志位，默认是true表示异步，false表示同步            
                    post请求参数通过send传递，不需要通过encodeURI()转码
                    必须设置请求头信息
                */
                var param = 'username='+uname+'&password='+pw;
                ------------------------------------------------
                get方法:
                xhr.open('get','03get.php?'+encodeURI(param),true); 
                // 3、执行发送动作                                 
                xhr.send(null);//get请求这里需要添加null参数  
                ------------------------------------------------
                post方法:
                xhr.open('post','04post.php',false);
                // 3、执行发送动作
                xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
                xhr.send(param);//post请求参数在这里传递，并且不需要转码
                ------------------------------------------------
                // 4、指定回调函数
                xhr.onreadystatechange = function(){
                    if(xhr.readyState == 4){
                        if(xhr.status == 200){
                            var data = xhr.responseText;
                            var info = document.getElementById('info');
                        if(data == '1'){
                            info.innerHTML = '登录成功';
                        }else if(data == '2'){
                            info.innerHTML = '用户名或者密码错误';
                        }
                    }
                }
            }
        }
    </script>

    <body>
        <form>
            用户名：<input type="text" name="username" id="username">
            <span id="info"></span>
            <br> 
            密码：<input type="text" name="password" id="password">
            <input type="button" value="登录" id="btn">
        </form>
    </body>

> PHP

    <?php
        ----------------------------
        $_GET方法:
        $uname = $_GET['username'];
        $pw = $_GET['password'];
        ----------------------------
        $_POST方法:
        $uname = $_POST['username'];
        $pw = $_POST['password'];
        ----------------------------
        if($uname == 'admin' && $pw == '123'){
            echo 1;
        }else{
            echo $uname;
        }
    ?>


## XML
### XML 数据格式
#### 什么是 XML
- XML 指可扩展的标记语言
- 主要用来输出和存储数据
    - 设置宗旨是(传输数据)，而非显示数据
- XML 标签没有语义化，需要自行定义标签

#### XML 数据格式的缺点
- 原数据占用的数据量比较大，不利于大量数据的网络传输
- 解析不太方便

#### XML 和 HTML 的区别
- XML 是用来传输和存储数据的，而HTML被设计是用来显示数据的
- XML 只在传输数据,HTML 只在显示信息

#### XML 的树结构
- XML 文档行程了的也是一种"树结构"
- __XML文档必须包含根元素__. 该元素是所有其他元素的父元素, 树结构从根开始,扩展到最低端

#### XML 的语法
- 所有 XML 元素都必须是闭合标签
- XML 标签大小写明感,因此必须使用相同的大小写来编写打开标签和关闭标签
- XML 必须正确的嵌套
- XML 文档必须有根元素
- XML 的属性值必须加引号
- 在 XML 中,空格会被保留

## JSON 
### json 数据结构
#### 什么是 JSON 
- JavaScript 对象表示法
- 是存储和交换文本信息的语法
- 轻量级的文本数据交换格式

#### JSON 数据和普通数据的 JS 兑现的区别
- json 对象没有变量
- json 形式的数据结尾没有分号
- json 数据中的键必须用双引号包括

#### JSON 和 XML 对比
- JSON 比 XML 更小, 更快, 更易解析

### json 数据解析
- 把 json 文本转换为 JavaScript 对象

    > JSON 最常见的用法之一, 是从 Web 服务器上读取 JSON 数据(作为文件或作为HttpRequest), 将 JSON 数据转换为 JavaScript 对象, 然后在网页中使用该数据.
    
-  为什么要转换

    > 在数据传输过程中, json 是以文本, 即字符串的形式传递的, 而 JS 操作的是 JSON 对象. 所以, JSON 对象和 JSON 字符串之间的相互转换是关键.

- 转换的方法
    - `JSON_parse();` 把 json 形式的字符串转成对象
    - `JSON_stringify();` 把对象转成字符串
    - `eval()` 把字符串解析成 js 代码并执行

### JSON 数据接口
- `json_encode();` 把数组转换成 JSON 形式的字符串

### 同步与异步

- 同步
    - 重新加载页面
    - 彼此等待
- 异步
    - 局部加载页面
    - 各做各的

- 面试题：为什么JavaScript是单线程的却能让AJAX异步发送和回调请求
    
    > JS运行在浏览器中，是单线程的，每个window一个JS线程，既然是单线程的，在某个特定的时刻只有特定的代码能够被执行，并阻塞其它的代码。而浏览器是事件驱动的（Event driven），浏览器中很多行为是异步（Asynchronized）的，会创建事件并放入执行队列中。javascript引擎是单线程处理它的任务队列，你可以理解成就是普通函数和回调函数构成的队列。当异步事件发生时，如mouse click, a timer firing, or an XMLHttpRequest completing（鼠标点击事件发生、定时器触发事件发生、XMLHttpRequest完成回调触发等），将他们放入执行队列，等待当前代码执行完成。

## Ajax跨域
- 什么是跨域

    > 跨域是指从一个域名的网页去请求另一个域名的资源。只要 __协议__、__域名__、__端口__有任何一个的不同，就被当作是跨域。

- 跨域解决方案
    - jsonp,
    - document.domain + iframe,
    - location.hash + iframe,
    - window.name + iframe,
    - window.postMessage,
    - flash 等第三方方插件.
    
    > 大部分都是使用 jsonp.
    
### JSONP 原理

- 静态 script 标签的 src 实现跨域

    - script标签里面的 async 属性表示异步加载资源, 默认是同步加载.

- 动态创建 script 标签发出去的请求是异步请求.

### jQuery 基本使用 ($.ajax)

    $.ajax({
        url: '地址',
        type: '请求数据方法 ( get post )',
        dataType: '返回的数据类型 (json jsonp )',
        data: {后台接受的数据 : 获取某个数据},
        jsonpCallback:'为jsonp请求指定一个回调函数名'
    })
     success:function(返回的数据){
        请求成功后的回调函数;
     };
