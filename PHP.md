# PHP

HTML

- Hypertext markup language

超文本标记语言



##### 标签

```html
<!--align  #对其 -->
<b align=left/right/center></b>
```

### 注释

```php
<?php
  // PHP代码
  #单行注释
    echo "hello"
    print "world"
  /*
  多行
  注释
  */
?>
```

### PHP 是一门弱类型语言

> 强类型: 不同数据类型进行运算,会报类型错误
>
> 动态：不使用显示数据类型声明，且确定一个变量的类型是在第一次给它赋值的时候。
>
> 脚本语言：一般是解释性语言，运行代码只需要一个解释器，不需要编辑。

### 作用域

```php
local 
global 
static //局部变量不被删除
parameter
```

需要==注意==的是:函数中的变量为局部变量,当你调用全局变量时,一定要声明**global**,不然只是新建一个局部变量

### echo/print/var_dump

print  有返回值 为1

echo  无返回值

**var_dump**() 返回数据类型和值 

### 类

```php
<!DOCTYPE html>
<html>
<body>

<?php
class Car
{
    var $color;
    function __construct($color="green") {
      $this->color = $color;
    }
    function what_color() {
      return $this->color;
    }
}

function print_vars($obj) {
   foreach (get_object_vars($obj) as $prop => $val) {
     echo "\t$prop = $val\n";
   }
}

// 实例一个对象
$herbie = new Car("white");

// 显示 herbie 属性
echo "\therbie: Properties\n";
print_vars($herbie);

```

### 比较

#### 类型比较

```php
==   //值比较值
===  //比较值也比较类型
```

#### 三元运算符

```php
(expr1) ? (expr2) : (expr3) 
true? 1:3
>>>1
//PHP 5.3+ 写法
(expr1) ?: (expr3) 
>>true 返回expre1
>>false 返回expre2
```

#### 组合比较符(PHP7+)

```php
$c = $a <=> $b;
解析如下：
	如果 $a > $b, 则 $c 的值为 1。
	如果 $a == $b, 则 $c 的值为 0。
	如果 $a < $b, 则 $c 的值为 -1。
```



### 常量定义

```php
<?PHP
define('log','OPen',true);//true 表示常量大小写不敏感
echo log; //定义常量不能加$
echo log;	//如:echo $log
?>
```

### string

strlen() 返回str长度

strpos(str, pos) 返回第一个匹配字符(pos)索引,没有返回false

```php
<?php
echo strpos("hello world", "world");
?>
6
```

### for

```php
1. for($i=1;$i<5;$i++){ }
2. foreach($array as $value){ }
3. foreach($array as $key => $value){ }
```

### 魔术常量

```php
__LINE__   //文件当中的行号
__FILE__   //文件的完整路径和文件名
__DIR__		//文件所在的目录 等价于dirname(__FILE__)
__FUNCTION__//函数名字
__CLASS__   //类名字
__TRAIT__   //代码复用
__METHOD__  //方法定义时的名字
__NAMESPACE__//命名空间的名称
```

### 命名空间(namespace)

查资料

