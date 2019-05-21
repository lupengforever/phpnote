# php 面向对象

### 类的定义、属性、方法、实例化

!> 童子军军规----让营地比你来时更干净！

需要掌握的知识点

面向对象基础

    类和对象：声明及实例

    构造方法：自动加载对象

    析构方法：自动加载对象

    基本数据类型：

    继承：为什么要继承、如何使用继承

    可见性(visibility)：public、private、protected、var、const、static、

创建第一个类以及实例

```php
    class ClassNmae{//定义一个类
        //定义类属性 也称为成员变量 主要用来存对象之间不同的数据，关键字publi决定了属性的作用域 (public、private、protected 5引入)
        public $publicVariable = "";

        public $title = null;

        //构造方法 也叫作构造器  当实例化此类时自动调用的方法 魔术方法方法名前以两个下划线开头
        public function __construct($title = "默认值"){
            $this->title = $title;
        }
        //定义类方法 获取属性方法最好用set、get+属性名为名定义方法名
        public function functionName(string $type){//如果省去关键字默认为public
            //$this伪变量 类本身
            return $this->publicVariable;
        }
        //对象在内存中删除时自动调用
        public function __destruct(){
            if(){
                //处理逻辑
            }
        }
    }

    $obj = new ClassName();//实例化一个对象 操作符 new
    //基本使用
    $publicVariable = $obj->publicVariable;
    $publicVariable = $obj->functionName();
    unset($publicVariable);
```

!> 正确的使用作用域关键字可以使代码更健壮 懂得?

> public 任何地方都可以调用属性方法

> private 私有的 只能在当前类使用，即使在子类中也不能使用

> protected 当前类、子类中才能访问外部无权点用

PHP 虽是弱类型语言，并不是说 PHP 就没有类型概念。PHP 的每个变量都有类型下表是 PHP 的基本检查函数

| 检查函数      | 类型     | 描述          |
| ------------- | -------- | ------------- |
| is_bool()     | 布尔型   | true 或 false |
| is_integer()  | 整形     | 整数          |
| is_double()   | 双精度型 | 浮点数        |
| is_string()   | 字符串   | 字符串类型    |
| is_object()   | 对象     | 对象类型      |
| is_array()    | 数组     | 数组类型      |
| is_resource() | 资源     | 资源类型      |
| is_null()     | null     | null          |

### 类的继承

继承是从一个基类生成或者派生出多个类的机制

> 继承自另一个类的类叫做另一个类的子类，常叫做子类、父类（超类）

!> 被 final 定义的类不能有子类。final 定义的方法不能被覆写

使用场景：当需要要定义多个类时，而且这些类又有共同的功能 使用继承可以在一定程度上减少代码冗余。例：

```php
class ShopProduct
{
    public $title = '默认标题';
    public function __construct(string $title = '默认值')
    {
        $this->title = $title;
    }

    public function getTitle()
    {
        return $this->title;
    }
}
//子类
class CdProduct extends ShopProduct
{
    public $price = 15;
    public function __construct(string $title, int $price)
    {
        parent::__construct($title); //当父类有构造方法时 子类也要有  要用关键字parent::调用
        $this->price = $price;
    }

    public function getPrice()
    {
        return $this->price;
    }
    //当子类方法名誉父类方法名重名时  子类方法你会覆盖掉父类方法 不想覆盖可以用关键字parent::先掉用方法获取返回值
    //一定要注意父类方法是否调用其他成员属性或者方法
}

$obj = new CdProduct('这是商品名10块能买吗', 10);
echo $obj->getPrice(); //10
echo $obj->getTitle();//这是商品名10块能买吗
```

### 进阶 静态属性方法、常量

```php
class StaticExample
{
    //self与$this区别 self指向当前类 $this指向当前对象
    //关键词static
    static public $name = '静态属性';
    public $testName = '测试静态属性调用正常属性';
    //常量用关键字const声明 命名风格如下
    const OUT_OF_STOCK = 1;
    //在类中不能调用静态方法，因此静态属性和静态方法称为类属性和类变量，因此不能再静态方法内使用$this
    static public function sayHellow()
    {
        //静态属性在类中只能在静态方法中调用
        //在类中方法调用静态属性用self::关键字
        echo self::$name;
        echo self::$testName; //报错 -》静态方法不能调用正常属性
        return 'hellow';
    }
}
```

### 抽象类(abstract)

抽象类不能直接被实例化，只定义了（部分实现）子类的需要的方法和属性

```php
abstract class Product
{
    public $arr = array();
    abstract public function getTitle();
    public function getName()
    {
        return $this->arr;
    }
    //多数情况下抽象类至少包含一个抽象方法 同样使用abstract关键字声明其中不能有具体内容要以‘;’结尾
    //抽象类的子类必须实现抽象类的抽象方法或者把他们声明为抽象方法
}
```

### 接口(interface)

> 关键字 interface(声明接口类)、implements(实现接口类,如果有 extends，extends 应在 implements 之前)

```php
interface Chargeble
{
    public function getPrice();
}
class ShopProduct implements Chargeble
{
    //可以实现多个接口 多个以‘;’分割
    public function getPrice()
    {
        return 10;
    }
}
```

### 异常处理（TODO）

### 拦截器

其实就是类的一些魔术方法，在适合的条件下被调用

> **get() **set() **isset() **unset() \_\_claa()

[详情请点击](https://php.net/manual/zh/language.oop5.magic.php)

## 对象工具

> PHP 和包 PHP 包和命名空间

命名空间 声明关键字 namespace 引用关键字 use（可以用关键字 as 起别名） `命名要规范 要规范 要规范` 重要的事儿说三遍

```text
在这里先了解一下 include_once 与 require_once 的区别 ,require()调用文件出错时会停止整个程序，而include()出错会生成警告，跳出引用代码然后继续执行 自动加载一般使用require这样的加载文件
```

```php
public function __autoload($className){
    if( preg_math('/\\\\/', $className)){
        $path = str_replace('\\',DIRECTORY_SEPARATOR, $className);
    } else {
        $path = str_replace('_',DIRECTORY_SEPARATOR, $className);
    }
    require_once($path . ".php");
    //spl_autoload_register() 可以看看
}
```

| 相关方法、操作符   | 类型             |
| ------------------ | ---------------- |
| class_exists()     | 类存在否         |
| instanceof         | 是否属于         |
| method_exists()    | 方法是否存在     |
| get_class_vars()   | 获取属性返回数组 |
| get_parent_class() | 是否存在父类     |

> 方法调用 call_user_function()
> 调用函数时 参数一 方法名（字符串）
> 调用类方法时 参数位数组 数组的第一个元素参数是对象，剩下的是对象的方法名

## 常见设计模式

### 观察者模式

```php
    //主题接口
    interface Theme
    {
        public function register(Observer $observer);
        public function notify();
    }

    //观察者接口
    interface Observer
    {
        public function watch();
    }

    //主题
    class Action implements Theme
    {
        public $_observer = [];
        public function register(Observer $observer)
        {
            $this->_observer[] = $observer;
        }
        public function notify()
        {
            foreach ($this->_observer as $observer) {
                $observer->watch();
            }
        }
    }

    //观察者
    class People implements Observer
    {
        public function watch()
        {
            echo " this is people <br/>";
        }
    }
    //实例化
    $obj = new Action();
    $obj->register(new People());
    $obj->notify();

```

### 适配器模式

```php
    interface MediaPlayer
    {
        public function play($audioType, $fileName);
    }

    interface AdvancedMediaPlayer
    {
        public function playVlc($fileName);
        public function playMp4($fileName);
    }

    class MediaAdapter implements MediaPlayer
    {
        private $media;

        public function __construct($audioType)
        {
            switch ($audioType) {
                case 'vlc':
                    $this->media = new VlcPlayer();
                    break;
                case 'mp4':
                    $this->media = new Mp4Player();
                    break;
                default:
                    var_dump("没有" . $audioType . "文件格式");
                    echo '<br/>';
            }
        }

        public function play($audioType, $fileName)
        {
            switch ($audioType) {
                case 'vlc':
                    $this->media->playVlc($fileName);
                    break;
                case 'mp4':
                    $this->media->playMp4($fileName);
                    break;
                default:
                    var_dump("没有" . $audioType . "文件格式");
                    echo '<br/>';
            }
        }
    }

    class Mp4Player implements AdvancedMediaPlayer
    {
        public function playVlc($fileName)
        { }

        public function playMp4($fileName)
        {
            var_dump("播放MP4格式的文件" . $fileName);
            echo '<br/>';
        }
    }

    class VlcPlayer implements AdvancedMediaPlayer
    {
        public function playVlc($fileName)
        {
            var_dump("播放vlc格式的文件" . $fileName);
            echo '<br/>';
        }

        public function playMp4($fileName)
        { }
    }

    class AudioPlayer implements MediaPlayer
    {
        public function play($audioType, $fileName)
        {
            if ($audioType == 'mp3') {
                var_dump("播放mp3格式的文件" . $fileName);
                echo '<br/>';
            } elseif ($audioType == 'vlc' || $audioType == 'mp4') {
                $media = new MediaAdapter($audioType);
                $media->play($audioType, $fileName);
            } else {
                var_dump("非法格式的文件" . $fileName);
                echo '<br/>';
            }
        }
    }


    $media = new AudioPlayer();

    $media->play("mp3", "beyond the horizon.mp3");
    $media->play("mp4", "alone.mp4");
    $media->play("vlc", "far far away.vlc");
    $media->play("avi", "mind me.avi");
```
