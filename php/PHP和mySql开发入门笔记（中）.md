# 面向对象
1. 关键字: 软件危机 软件工程学 、结构化方法 oop(面向对象编程);
2. 目标: 重用性、灵活性、扩展性
3. 特点: 封装、继承、多态 (打死都要记住！！)

## 面向对象程序设计
1. 什么是类: 抽象的大的概念。 比如: 人类，Person
2. 什么是对象: 具体的事物，比如: 我这个人, xiaohong

如何抽象一个类：
  - 类的声明
  - 成员属性
  - 成员方法
  代码实现
```php
    /**
 * 类的声明
 */
class Person{
  public $age;
  public function say($word){
    echo "she say {$word}";
  }
  public function info(){
    $this -> say('Hi');
   return  $this -> age;
  }
}
$xiaohong = new Person();
$xiaohong->age = 33;
$age = $xiaohong->info();
echo "<br/> $age"
```
# 构造函数与析构方法
构造函数
>当创建一个对象时,它将自动调用构造函数,也就是使用new这个关键字来实例化对象的时候

析构方法
>析构函数允许在销毁一个类之前执行的一些操作或完成一些功能,比如说关闭文件,释放结果集等.

代码实现
```php
<?php
/**
 * 本demo是为了测试构造函数和析构方法
 */
class Person
{
  public function __construct($name,$age)
  {
    // 使用这个类new的时候自动调用构造函数,
    $this -> name = $name;
    $this -> age = $age;
  }
  public function data ()
  {
    return $this -> age;
  }
  public function __destruct()
  {
    // 析构函数会在到某个对象的所有引用都被删除或者当对象被显式销毁时执行,也就是象在内存中被销毁前调用析构函数
    echo "<br/> bye bye {$this -> name} <br/>";
  }
}

$xiaowang = new Person("hh",30);
echo $xiaowang -> data(); // 30
?>
```
# 封装性
> 控制成员的访问权限
- 封装的修饰符public (公有的 默认) 、private(私有的) 、protected (受保护的)
- 设置私有成员 private(私有的)
```php
  <?php
  /**
   * 定义一个类 学习 public private protected
   */
  class Person
  {
    public $x= 0;
    private $name = "liuyeqian";
      private $age = 18;
      protected $money = 10;

      //  私有的成员方法, 不能再类外部被访问
      private function getAge($value="")
      {
        return $this -> age;
      }
      // 被保护的成员方法 不能再类的外部直接访问
      protected function getMoney()
      {
        return $this -> money;
      }
      public function userCard() 
      {
        echo "name->".$this -> name."\n age->".$this -> getAge() . " money->".$this -> getMoney();
      }
      public function __set($key,$value)
      {
        //  魔术方法的set 只针对 protected private
        if($key == "name"&& $value=="xw")
        {
          $this -> name = "xiaowang";
        }

      }
      public function __get($key)
      {
          if($key == "age"){
            return "not tell you";
          }
      }

      public function __isset($name)
      {
        // 
        if($name == "age"){
          return true;
        }
      }

      public function __unset($key){
        echo 1111;
        if($key == "age"){
          unset($this->age);
        }
      }
  }
  $ll = new Person();

  $ll ->name = "xw";
  echo $ll -> userCard();
  // unset($ll->x);
  echo $ll->x ;
  ?>
```
# 继承和多态
> php 只支持单继承, 不允许多重继承。可以有多层继承。
```php
<?php
/**
 * 父类
 */
class Person
{
    public $name;
    private $age; // 不可以被继承
    protected $money; //可以被继承
    function __construct($name,$age,$money)
    {
        $this -> name = $name;
        $this -> age = $age;
        $this -> money = $money;
    }
    public function cardInfo()
    {
      echo "name->".$this->name. " age->". $this-> age. "money->". $this-> money;
    }
}

class Yellow extends Person
{
    function __construct($name,$age,$money)
    {
          parent::__construct($name, $age, $money);
    }

    // php 重写
    public function cardInfo(){
      // php 实现重载的方法
      parent::cardInfo();
    }
    public function test(){
      echo $this -> money;
    }
}

$yy = new Yellow("张家辉",12,100);
$yy -> cardInfo(); // name->张家辉 age->12money->100
$yy -> test(); // 100
?>
```
# PHP 抽象类与接口

抽象类的定义和特点
>定义为抽象的类不能被实例化.任何一个类，如果它里面至少有一个方法是被声明为抽象的，那么这个类就必须被声明为抽象的。被定义为抽象的方法只是声明了其调用方式（参数），不能定义其具体的功能实现。
- 继承一个抽象类的时候，子类必须定义父类中的所有抽象方法
- 调用方式必须匹配，即类型和所需参数数量必须一致
```php
    <?php
    /**
     * 1.含有抽象方法的类必须是抽象类
     * 2. 抽象类不一定非得含有抽象方法
     * 3. 抽象类可以存在普通的方法
     * 4. 抽象类不被实例化 必须由一个子类去继承 并且把抽象类的方法都实现了
     */
    abstract class  Person
    {
        // 抽象方法 没有方法体 
          public abstract function eat();
    }
    class Man extends Person
    {
        public function eat(){
        echo "eat";
      }
    }
    $man = new Man();
    $man -> eat();
    ?>
```
接口的定义和特点
> 一个实现了该接口的类必须实现的一系列函数
```php
  <?php
  /**
   * 1. 接口声明的关键字是interface
   * 2. 接口可以声明常量也可以是抽象方法
   * 3. 接口中的方法都是抽象方法 不用abstract去人肉的定义
   * 4. 接口不能被实例化 需要一个类去继承它
   * 5. 一个类可以实现多个接口
   */
  interface Person{
    const NAME = "xiaowang";
    public function run();
    public function eat();
    }
  interface Study {
    public function study();
  }
  class Student implements Person, Study
  {
    public function run (){
      echo "run";
    }
    public function eat (){
      echo "eat";
    }
    public function study (){
      echo "study";
    }
  }
  $xiaoliu = new Student();
  $xiaoliu -> eat();
  $xiaoliu -> run();
  $xiaoliu -> study();
  echo $xiaoliu::NAME;
  ?>
```
抽象类和接口的区别
> 当你关注一个事物的本质的时候 用抽象类, 当你关注一个操作的时候，用接口。 接口是对动作的抽象,  对类的局部行为进行抽象。
抽象类是对根源的抽象，对类整体进行抽象。
- 接口是抽象类的变体, 接口中所有的方法都是抽象的. 而抽象类是声明方法存在而不去实现它的类。
- 接口可以多继承, 抽象类不行。
- 接口定义方法， 不能实现， 而抽象类可以实现部分方法。
- 接口中基本数据类型为static 而抽象类不是的。
- 接口中不能含有静态代码块以及静态方法，而抽象类可以含有静态方法和静态代码块。

对象的多态应用
> 在父类中定义的属性或者行为被子类继承之后，可以具有不同的数据类型或者表现出不同的行为. 这使得同一个属性或者行为在父类以及其各个子类中具有不同的语义。

# 常见关键字
- final 关键字 【为了安全，没有必要被重写或继承】
> 只能用来修饰类和方法，不能使用final这个关键字来修饰成员属性。

注意啦：
1. 使用final 关键字标识的类不能被继承
2. 使用final关键字标识的方法不能被子类覆盖（重写）， 是最终版本。
- static 关键字
> static表示静态的意思。 用于修饰类的成员属性和成员方法（即静态属性和静态方法）
1. 类中的静态属性和方法不用实例化就可以使用类名直接访问 类::$静态属性 self::静态方法
2. 静态方法中不可以使用非静态的内容 就是不让使用$this
3. 静态属性是共享的。
- 单例设计模式
> 一个类只能有一个实例对象存在。
- const关键字
> const 是一个在类中定义常量的关键字
- instanceof 关键字
> 用来检测当前对象实例是否属于某一个类或这个子类的子类。

# 魔术方法
- 克隆对象
- 类中通用的方法 __toString()
- __call() 方法的应用
- 自动加载类
- 对象串行化

# 常用函数


# php面向对象异常处理
- 系统自带的异常处理

- 自定义异常处理
- 捕捉多个异常处理
