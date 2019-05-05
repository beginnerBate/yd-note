# 初始php

1. php 是一种通用开源脚本语言。
2. 主要用于web开发领域。
3. php可以将程序嵌入到HTML文档中去执行。
4. php可以执行编译后的代码。

# 第一个PHP文件

环境: win10 + xampp 

a.php 测试
```html
   <!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>我的第一个php</title>
</head>
<body>

  <?php
    echo "我的第一个PHP 脚本！";
    if(1>2){
      echo "我的第一个PHP 脚本！";
    }else{
      echo "Hello PHP";
    }
  ?>

</body>
</html>
```

# 变量 $+名称 万能变量

isset  判断变量是否定义
```php
  if(isset($a)){
    echo "我是一个声明生的";
  }else{
    echo "我是一个未声明的";
  }
```
全局变量 global 

```php
  $a = "我是外面的";
  function test () {
    echo $a;
  }
  test() //  会报错

  function test1() {
    global $a;
    echo $a;
  }
  test1() // 我是外面的。
```
引入外部文件 require_once('')和include_once(''), 在引入错误文件的时候：

 1. include_once() // 不管有没有错 都会执行然后报错
 2. require_once() // 报错 不执行

 ```php
   // a.php

   <?php
      $GLOBALS['b']= "test";
   ?>

  //  index.php

  <?php
// require_once('a.php');
// 引入外面文件
include_once('a.php');

echo $GLOBALS['b'];
$a = "我是外面的";
function test(){
    global $a;
    echo $GLOBALS['b'];
    // echo $a;
}
test();
?>
 ```

 # PHP 数组
 ```php
     // 数组
    $arrTest = array('0' => "苹果",1=> "测试");
    // 输出数组
    echo json_encode(arrTest);
 ```
# php session 会话机制。
a.php 和 index.php
ap.php
```php
 session_start();
 $_SESSION['view'] = 1;
```
index.php
```php
session_start();
echo "Pageviews=". $_SESSION['views']; // Pageviews=1
```

# php处理html表单请求
## 1. get请求处理
```php
     $_GET['paramName'] 
     $_REQUEST['paramName']
```
## post 请求处理
```php
     $_POST['paramName'] 
     $_REQUEST['paramName']
```
## php结合ajax 实现页面无调转
ajax.php
```html
  <!DOCTYPE html>
  <html>
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>ajax 请求 和 php</title>
    <!-- 引入jquery -->
    <script src="https://code.jquery.com/jquery-3.4.1.min.js"></script>
  </head>
  <body>
    <form action="b.php" method="get">
      <div>
        <label for="username">用户名:</label><input type="text" name="username" id="username">
      </div>
      <div>
        <label for="password">密码:</label><input type="password" name="password" id="password">
      </div>
      <div>
        <button type="submit" id="subBtn">提交</button>
      </div>
    </form>
  </body>
  <script>
  $("#subBtn").click(function(e){
    e.preventDefault();
    $.ajax({
      url:"b.php",
      type:"get",
      data:{
        username: $("#username").val()
      }
    }).then((res)=>{
      console.log(res);
    }).fail((err)=>{
      console.log(err)
    })
  });
  </script>
  </html>
```
b.php
```php
  <?php
    header("Content-type:application/json;charset=utf-8");
    $username = $_GET['username'];
    if($username == "admin"){
      echo json_encode(array("code"=>200, "msg" => "ok"));
    }else{
      echo json_encode(array("code"=>400, "msg" => "error"));
    }
  ?>
```