# php学习

## 目标
1. PHP基本语法

## 一、PHP基本语法

### 1.1 变量和类型
php 变量是一个美元符号后面跟变量名来表示，变量名区分大小写;

一个有效的变量名由字母或者下划线开头，后面跟上任意数量的字母，数字，或者下划线。

☆ $this是一个特殊的变量，它不能被赋值。
```php
<?php
   $ var1 = "99";
   $ Var1 = "55";
   echo "$var,$Var"; // 输出 "99, 55"
   // var1 和 Var1 是两个不同的变量。 
   
   $4site = "not yet"; //非法变量名;以数字开头
   $_4site = "not yet"; //合法变量名
   $i站戴安is = "mansikka"; // 变量名合法可以用中文
?>
```
PHP提供了两种赋值方式，传值赋值和引用赋值 默认是传值赋值。引用赋值，简单的将一个&符号加到将要赋值的变量前（源变量）。
```php
    <?php
        $foo = "Bob";
        $bar = &$foo; // 通过$bar 引用 $foo
        $bar = "MY name is $bar";
        echo $bar; // "My name is Bob"
        echo $foo; // "My name is Bob"
    ?>
```
☆ 只有有名字的变量才可以引用赋值
```php
  <?php
    $foo = 25;
    $bar = &$foo;
    $bar = &(24 * 7); // 非法；引用没有表达名字的表达式。

    function test (){
        return 25;
    }
    $bar = &test(); // 非法；
  ?>
```
isset() 用来检测一个变量是否已经被初始化。
```php
  print isset($a); // false
  $b = 0;
  isset($b); // true
```
可变变量
一个变量的变量名可以动态的设置和使用。一个普通的变量通过声明来设置。
```php
  $a = "hello";
  $$a = "world";
  // 两个变量都被定义了: $a 的内容是"hello" 并且 $hello 的内容是"world".
  echo "$a ${$a}";  // hello world
  echo "$a $hello"; // hello world   
  //  !!! 好神奇哦  
```
PHP 支持9种原始数据类型。
四种标量类型:
* boolean (布尔型)
* integer (整型)
* float (浮点型，也称作 double)
* string (字符串)

三种复合类型
* array (素组)
* object (对象)
* callable (可调用)

两种特殊类型
* resource (资源)  