---
aliases: []
tags:
  - PL
  - php
  - base
created: 2025-02-25 03:33:52
modified: 2025-02-25 05:31:40
---

# PHP 基础语法

---

## 大小写敏感性

PHP 中的大小字敏感性与 [C语言](../C/C_Note.md) 或 [Java](../Java/Java_Note.md) 等不太一样。大概有以下两个规则：

* [变量](#变量) 的大小写敏感
* [类](#类)、[函数](#函数) 及「关键词」大小写不敏感

## 变量

### 变量命名规则

* 变量以 `$` 符号开头，其后是变量的名称
* 变量名必须以**字母**或**下划线**开头
* 变量名不能以数字开头，不能有空格
* 变量名只能包含**字母**、**数字**和**下划线**
* 变量名对大小写敏感

---

## 类

PHP 是一种面向对象的编程语言，支持类和对象的概念。类是 PHP 中面向对象编程（OOP）的核心概念之一。类可以看作是一个蓝图或模板，用于创建对象。对象是类的实例，具有类定义的属性和方法。

### 类的定义

在 PHP 中，类使用 `class` 关键字定义。类可以包含属性（变量）和方法（函数）。以下是一个简单的类定义示例：

```php
class Car {
    // 属性
    public $color;
    public $model;

    // 方法
    public function startEngine() {
        echo "Engine started!";
    }

    public function getDescription() {
        return "This car is a " . $this->color . " " . $this->model . ".";
    }
}
```

### 类的实例化

要使用类，首先需要创建类的实例（即对象）。使用 `new` 关键字可以实例化一个类：

```php
$myCar = new Car();
```

### 访问属性和方法

通过对象可以访问类的属性和方法：

```php
$myCar->color = "red";
$myCar->model = "Tesla Model S";

echo $myCar->getDescription(); // 输出: This car is a red Tesla Model S.
$myCar->startEngine(); // 输出: Engine started!
```

### 构造函数

构造函数是一个特殊的方法，在创建对象时自动调用。通常用于初始化对象的属性。在 PHP 中，构造函数使用 `__construct` 方法定义：

```php
class Car {
    public $color;
    public $model;

    // 构造函数
    public function __construct($color, $model) {
        $this->color = $color;
        $this->model = $model;
    }

    public function getDescription() {
        return "This car is a " . $this->color . " " . $this->model . ".";
    }
}

$myCar = new Car("blue", "Toyota Corolla");
echo $myCar->getDescription(); // 输出: This car is a blue Toyota Corolla.
```

### 访问控制

PHP 提供了三种访问控制修饰符：
* `public`：属性和方法可以在任何地方访问。
* `protected`：属性和方法只能在类内部和子类中访问。
* `private`：属性和方法只能在类内部访问。

```php
class Car {
    private $engineStatus = "off";

    public function startEngine() {
        $this->engineStatus = "on";
        echo "Engine started!";
    }

    public function getEngineStatus() {
        return $this->engineStatus;
    }
}

$myCar = new Car();
$myCar->startEngine(); // 输出: Engine started!
echo $myCar->getEngineStatus(); // 输出: on
```

### 继承

PHP 支持类的继承，允许一个类继承另一个类的属性和方法。使用 `extends` 关键字实现继承：

```php
class Vehicle {
    public $color;

    public function __construct($color) {
        $this->color = $color;
    }

    public function getColor() {
        return $this->color;
    }
}

class Car extends Vehicle {
    public $model;

    public function __construct($color, $model) {
        parent::__construct($color);
        $this->model = $model;
    }

    public function getDescription() {
        return "This car is a " . $this->color . " " . $this->model . ".";
    }
}

$myCar = new Car("black", "BMW X5");
echo $myCar->getDescription(); // 输出: This car is a black BMW X5.
```

----

## 函数

由 [DeepSeek](../AI/DeepSeek/DeepSeek_Note.md) 生成

常见的 PHP 函数及其详细说明

### 1. 字符串处理函数

* **strlen($string)**: 返回字符串的长度。
  ```php
  echo strlen("Hello World"); // 输出 11
  ```

* **strpos($haystack, $needle)**: 查找字符串中首次出现的位置。
  ```php
  echo strpos("Hello World", "World"); // 输出 6
  ```

* **substr($string, $start, $length)**: 返回字符串的子串。
  ```php
  echo substr("Hello World", 6, 5); // 输出 "World"
  ```

* **str_replace($search, $replace, $subject)**: 替换字符串中的部分内容。
  ```php
  echo str_replace("World", "PHP", "Hello World"); // 输出 "Hello PHP"
  ```

### 2. 数组处理函数

* `count($array)`： 返回数组中元素的数量。
  ```php
  $array = [1, 2, 3];
  echo count($array); // 输出 3
  ```

* **array_push($array, $value)**: 将一个或多个元素添加到数组的末尾。
  ```php
  $array = [1, 2, 3];
  array_push($array, 4);
  print_r($array); // 输出 [1, 2, 3, 4]
  ```

* **array_pop($array)**: 删除数组中的最后一个元素并返回它。
  ```php
  $array = [1, 2, 3];
  echo array_pop($array); // 输出 3
  print_r($array); // 输出 [1, 2]
  ```

* **array_merge($array1, $array2)**: 合并两个或多个数组。
  ```php
  $array1 = [1, 2];
  $array2 = [3, 4];
  print_r(array_merge($array1, $array2)); // 输出 [1, 2, 3, 4]
  ```

### 3. 文件处理函数

* **file_get_contents($filename)**: 将整个文件读入一个字符串。
  ```php
  echo file_get_contents("example.txt");
  ```

* **file_put_contents($filename, $data)**: 将字符串写入文件。
  ```php
  file_put_contents("example.txt", "Hello World");
  ```

* **fopen($filename, $mode)**: 打开文件或 URL。
  ```php
  $file = fopen("example.txt", "r");
  ```

* **fclose($handle)**: 关闭打开的文件指针。
  ```php
  fclose($file);
  ```

### 4. 数据库处理函数

* **mysqli_connect($host, $username, $password, $dbname)**: 打开一个到 MySQL 服务器的连接。
  ```php
  $conn = mysqli_connect("localhost", "user", "password", "database");
  ```

* **mysqli_query($connection, $query)**: 执行 SQL 查询。
  ```php
  $result = mysqli_query($conn, "SELECT * FROM users");
  ```

* **mysqli_fetch_assoc($result)**: 从结果集中取得一行作为关联数组。
  ```php
  while ($row = mysqli_fetch_assoc($result)) {
      echo $row['username'];
  }
  ```

* **mysqli_close($connection)**: 关闭先前打开的数据库连接。
  ```php
  mysqli_close($conn);
  ```

### 5. 其他常用函数

* **date($format, $timestamp)**: 格式化本地日期和时间。
  ```php
  echo date("Y-m-d H:i:s"); // 输出当前日期和时间
  ```

* **isset($variable)**: 检测变量是否已设置并且非 null。
  ```php
  if (isset($variable)) {
      echo "Variable is set.";
  }
  ```

* **empty($variable)**: 检查变量是否为空。
  ```php
  if (empty($variable)) {
      echo "Variable is empty.";
  }
  ```

* **header($string)**: 发送原始的 HTTP 头。
  ```php
  header("Location: http://www.example.com/");
  exit;
  ```

---

## 相关笔记

* [PHP 笔记](PHP_Note.md)
* [PHP 资料清单](PHP_Material.md)
* [PHP 视频清单](PHP_Videos.md)

