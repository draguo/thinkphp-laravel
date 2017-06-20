## 前言
作为一个 laravel 爱好者让我写 tp3.2 我本来是拒绝的，但是呢，拒绝了谁给你工资啊
那怎么办呢，既然 laravel 是基于 composer 的组件化开发的，那么能不能把 laravel 中的功能引入到 tp 中呢
### 环境
tp 3.2


### 问题
测试版和正式版的数据库一般是不同的, 那么如何方便的切换呢？  
* think php
```
$db = strpos(getcwd(), 'test') ? 'db_test' : 'db';
define('APP_STATUS',$db);
```
之前我才用的方法是看当前运行的文件夹是哪个就加载不同的文件，
这种方式的问题是你换了个文件夹名就不行了
作为 laravel 粉丝肯定是使用 env 文件

### 改造
通过看 laravel 的源码和官网的介绍开始进行改造 tp
#### step 1

```
composer require vlucas/phpdotenv
```
在 thinkphp 根目录中的 index.php 文件的头部引入

```
require 'vendor/autoload.php'
// for use .env
$dotenv = new Dotenv\Dotenv(__DIR__);
$dotenv->load();
```

#### step 2
在根目录新建 .env 文件

更多的使用 请参考 phpdotenv 的 [github](https://github.com/vlucas/phpdotenv)
### 使用更像 laravel 的 env 函数

在 composer.json 中
```
    "autoload": {
        "files": [
            "App/helpers.php"
        ]
    }
```
新建 helpers.php
```
function env($key, $default='undefined') {
    $value = getenv($key);
    if ($value === false) {
        return $default;
    }

    switch (strtolower($value)) {
        case 'true':
        case '(true)':
            return true;
        case 'false':
        case '(false)':
            return false;
        case 'empty':
        case '(empty)':
            return '';
        case 'null':
        case '(null)':
            return;
    }

    return $value;
}
```
然后执行
```
composer dump-autoload
```
然后就可以在 Conf 目录下使用了
```
env('DB_DATABASE')
```
这样配置上基本上就和 laravel 体验是一样的了
但这个东西对开发速度提升真的意义不是很大啊， laravel 中最好用的还是 [ORM](http://d.laravel-china.org/docs/5.4/eloquent) 啊， 下一篇将会带来 tp3.2 整合 laravel 的 ORM