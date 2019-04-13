# 你不知道的HTML 

# 知识点
1. 什么是浏览器同源同源策略？ 
2. 跨域的几种方式
3. HTML5 语义化标签
4. img标签知多少

##  一、 什么是浏览器同源同源策略？
**画重点!!**
> 同源策略<font color="red">限制</font>了从同一个<font color="green">源</font>加载的文档或脚本如何与来自另一个源的资源进行交互.这是一个用于隔离潜在恶意文件的重要安全机制。<br>

    1.1 同源的源是什么意思
    如果两个页面的协议、端口和主机(域名)都相同，那么两个页面用相同的源

    1.2 同源策略限制以下几种行为
        1.) Cookie、LocalStorage 和 IndexDB 无法读取
        2.) DOM 和 Js对象无法获得
        3.) AJAX 请求不能发送
    1.3 源的改变
```javascript
    //通过设置document.domain 为相同的值 来允许子域安全访问其父域

    //  http://store.company.com/dir/other.html 
    document.domain ="company.com"

    //  http://company.com/dir/page.html
    document.domain = "company.com"
```
##  二、  跨域的几种方式
    2.1 什么是跨域? 
    跨域是指一个域下的文档或者脚本试图去请求另一个域下的资源,这里的跨域是广义的。
    广义的跨域:
      1) 资源跳转: A链接[<a href=".."></a>]、重定向[window.location.href="..."]、表单提交
      2) 资源嵌入: <link> <script> 、<img> <frame> <iframe>，<video> 和 <audio>嵌入多媒体资源。
       <object>、<embed>、<applet>等dom标签和css中的 background:url(), @font-face()等。
    我们通常说的跨域是狭义的，是由浏览器同源策略限制的一类请求场景
    比如:
        http://www.domain1.com/a.js
        http://www.domain2.com/b.js 不同域名 不允许
        或者 端口不同，协议不同等。
    这类的跨域解决方案有：
        1. 通过jsonp 跨域 [get 请求 适合简单的请求]
        2. document.domain + iframe 跨域  
        3. location.hash + iframe 跨域 
        4. window.name + iframe 跨域 [和上面原理很像]
        5. postMessage 跨域 [未用过,可以尝试到项目中]
        6. 跨域资源共享（CORS）[常用的方法]
        7. nginx代理跨域 [待研究]
        8. nodejs中间件代理跨域 [待研究]
        9. WebSocket协议跨域 [带学习]

    2.2 jsonp跨域
    使用场景: 简单的get请求
    缺点: 只能是get请求,jsonp存在安全性问题
    优点: 简单容易实现
    实现方法:
    2.2.1. js 原生实现jsonp 原理
```javascript
  //  js 原生实现jsonp 原理
      var hander = function(data){
          alert(data);
      }
      var url = "10.0.0.103/index?code=ca001&callback=hander";
      var script = document.createElement('script');
      script.setAttribute('src',url);
      document.getElementsByTagName('head')[0].appendChild(script)
```   
    2.2.2. jsonp 简单实现
```javascript
  // jsonp 简单实现
  function jsonp(req){
    var script = document.createElement('script');
    var url = req.url + '?callback=' + req.callback.name;
    script.src = url;
    document.getElementsByTagName('head')[0].appendChild(script); 
  }  
  // 使用
  function hello (res){
      alert(res.data);
  }   
  jsonp({
      url:'',
      callback:hello
  });
``` 
    2.2.3. 可靠的jsonp实现    
[可靠的jsonp实现]("./prep-class/code/jsonp.html")

    2.3 document.domain + iframe 跨域
    ★★
    使用条件: 一级域名一致才可以相同的情况下,才能使用此方法。
    比如： yideng.com/test.html 和 a.yideng.com/a.html 这两个域名进行 在各自的页面中加上 
```javascript
    // 在test.html中和a.html中 把domain 设置成一样的域名[主域名]
    document.domain = "yideng.com"
```
    2.4 location.hash + iframe 跨域
    实现原理: 利用location.hash传值,根据需求监听hash 变化，执行相应的操作。
    优点:可以解决完全跨域问题,实现双向通信
    缺点: 安全性不高，支持传递的数据量小
    实现核心代码:
 ```javascript
    // yideng.com/yd.html 和 dengyi.com/dy.html 通信
    // yd.html
    function startRequest(){
        var ifr = document.craeteElement('iframe');
        ifr.style.display = 'none';
        ifr.src = 'http://dengyi.com/dy.html#test';
        document.body.appendChild(ifr);
    }   
    // 监听 hash 变化
    function chechHash() {
        try{
            var data = location.hash? location.hash.substring(1):"";
            console.log('Now the data is' + data);
            // 根据data做一些处理
            // .....
        } catch (e) { }
    }
    // 定时监听
  setInterval(chechHash,2000)  
 
 //-----------degngyi.com/dy.html 页面--------------
 function callBack(){
     try{
         parent.location.hash = 'testdata';
     }catch(e){}
     var ifrproxy = document.createElement('iframe');
     ifrproxy.style.display = 'none';
     ifrproxy.src = 'http://yideng.com/yd.html#testdata';
     document.body.appendChild(ifrproxy)
 }   
 switch (location.hash){
     case: '#test':
       callBack();
       break;
     case:"#other":
        //  ....
        break;  
 }
 ```
    2.5 window.name + iframe 跨域
    实现原理: window.name + iframe 跨域和 window.hash + iframe 跨域原理很像 ，利用window.name 属性的特点：
        1) window.name = string 设置窗口的名称，
        2) name 值在不同的页面（甚至不同域名）加载后依旧存在，并且可以支持非常长的 name 值（2MB）  
    ★★
    2.6 postMessage 跨域
    window.postMessage() 方法可以安全的实现跨源通信。
    语法：
 ```javascript
      otherWindow.postMessage(message,targetOrigin,[transfer]);
    //   the dispatched event
    window.addEventListener("message", receiveMessage,false);
    function receiveMessage(event){
        var origin = event.origin || event.originalEvent.origin;
        if (origin !== "http://example.org:8080")return;
    }
 ```   
     跨域代码实现
 ```javascript
      //  A: 窗口: http://example.com:8080  中js
      var popup = window.open('....');

      popup.postMessage('hello here', 'http://example.org');
      function receiveMessage(event){
          if(event.origin !== "http://example.org") return
      }
      window.addEventListener("message", receiveMessage, false);

    //  popup: 窗口：  http://example.org  中的js
    //  当A页面postMessage被调用后，这个function被addEventListenner调用
    function receiveMessage(event)
    {
    if (event.origin !== "http://example.com:8080")
        return;
    event.source.postMessage("hi there yourself!  the secret response " +
                            "is: rheeeeet!",
                            event.origin);
    }
    window.addEventListener("message", receiveMessage, false);
 ```
    2.7 跨域资源共享（CORS） 
    常用的跨域方法，主要在后端中设置。
    2.8 nginx代理跨域、nodejs中间件代理跨域、WebSocket协议跨域
    这三种跨域方法等我技术上个等级再来好好研究研究。 
## 三、 HTML5 语义化标签
> 为了seo 为了代码好理解 为了代码好维护 为了好写css 所以使用语义化的标签，让人你看就知道， 哦 这个是header 这个是footer
* 标准H5的代码结构 
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>h5语义化</title>
</head>
<body>
    <!-- header 网站的页眉 头部 -->
  <header>
    <h1>header</h1>
  </header>
  <!-- 网站的导航 可以在 header里面 -->
  <nav>nav</nav>
  <!-- main 文档主要的内容 一般一个网站里面只有一个 -->
  <main>
      <!-- 文档 应用 独立页面 -->
    <article>
        <!-- 区域  -->
      <section></section>
    </article>
    <!-- 侧边栏 广告 -->
    <aside> </aside>
  </main>
  <!-- 底部 页脚 -->
  <footer>
      <address>联系信息</address>
  </footer>
</body>
</html>
```
## 四、 img标签知道多
> img 标签除了 正常显示图片的使用功能之外，还可以实现：
1)  简单的测下载速度,代码如下:
```js
  var Img = new Image();
  Img.src = 'xxx.png' 
  fileSize = 100;

  var st = new Date();
  Img.onLoad = function(){
   var et = new Date();
   var speed = Math.round(filesize*1000)/(et - st);
   console.log(speed)
  }
```
[img测速](./code/img.html)
# 小结
> 主要了解同源策略，跨域的常用方法，h5 语义化， 研究跨域的时候发现跨域的水很深，需要掌握的知识点还挺多，最后三个跨域的方式在网上看了一下 感觉自己要提高技术水平之后 回过来在深入研究。