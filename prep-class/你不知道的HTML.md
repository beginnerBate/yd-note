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
        1. 通过jsonp 跨域
        2. document.domain + iframe 跨域
        3. location.hash + iframe 跨域
        4. window.name + iframe 跨域
        5. postMessage 跨域
        6. 跨域资源共享（CORS）
        7. nginx代理跨域
        8. nodejs中间件代理跨域
        9. WebSocket协议跨域

    2.2 jsonp跨域
    使用场景: 简单的get请求
    缺点: 只能是get请求,jsonp存在安全性问题
    优点: 简单容易实现
    实现方法:
    1. js 原生实现jsonp 原理
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
    2. jsonp 简单实现
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
    3. 可靠的jsonp实现    
[可靠的jsonp实现]("./prep-class/code/jsonp.html")

    2.3 document.domain + iframe 跨域
    
    前提条件：
    这两个域名必须属于同一个一级域名!而且所用的协议，端口都要一致，否则无法利用document.domain进行跨域。
    Javascript出于对安全性的考虑，而禁止两个或者多个不同域的页面进行互相操作。
    而相同域的页面在相互操作的时候不会有任何问题。
# 小结