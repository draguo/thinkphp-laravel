## 简介
orm Object Relational Mapping  
对象关系映射

laravel 中我认为最好用的莫过于 orm tp3.2 中也有类似的东西，但是使用起来总是感觉很费力，可能是先入为主的原因吧。

但是为了提升开发速度和减少使用 tp3.2 的不适感，现在来把 laravel 中的 orm 接入到 tp3.2 中

为什么说是接入呢？  
因为 laravel 的 orm 是可以单独使用的详见

[illuminate/database](https://github.com/illuminate/database)  
感谢组件化开发，感谢 composer

### step 1

到 [这里](https://packagist.org/packages/illuminate/database) 选择合适的版本
```
composer require illuminate/database
// or for php 5.5.9
composer require illuminate/database:5.2.*
```

依然保持 index.php
```
require 'vendor/autoload.php';
```

### step 2
```
Thinkphp/Library/Think/Think.class.php 中的 start() 方法中的 App::run() 之前添加
// ORM
$capsule = new \Illuminate\Database\Capsule\Manager;
$capsule->addConnection([
    'driver'    => C('DB_TYPE'),
    'host'      => C('DB_HOST'),
    'database'  => C('DB_NAME'),
    'username'  => C('DB_USER'),
    'password'  => C('DB_PWD'),
    'charset'   => C('DB_CHARSET'),
    'collation' => C('DB_COLLATION'),
    'prefix'    => C('DB_PREFIX'),
]);
$capsule->setAsGlobal();
$capsule->bootEloquent();
```

### step 3
为了可以加载 App/Models 下的文件在 composer.json 中
```
"autoload": {
    "files": [
        "App/helpers.php"
    ],
    "psr-4": {
        "App\\": "App"
    }
}
```

然后就可以想 laravel 中的 [orm](http://d.laravel-china.org/docs/5.4/eloquent) 一样使用了

### tip
 这时是不能使用 paginate 的要想使用
 ```
 composer require illuminate/pagination
 ```
 同时注意选择对应的版本