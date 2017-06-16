### 问题
测试版和正式版的数据库一般是不同的, 那么如何方便的切换呢？  
* think php
```
$db = strpos(getcwd(), 'test') ? 'db_test' : 'db';
define('APP_STATUS',$db);
```
这种方式的问题是你换了个文件夹名就不行了
作为 laravel 粉丝肯定是使用 env 文件

#### step 1

```
composer require vlucas/phpdotenv
```
在 index.php 文件的头部引入

```
require 'vendor/autoload.php'
// for use .env
$dotenv = new Dotenv\Dotenv(__DIR__);
$dotenv->load();
```

#### step 2
在根目录新建 .env 文件

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
    return $value;
}
```
然后就可以在 Conf 目录下使用了
```
env('DB_DATABASE')
```
