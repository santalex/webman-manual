## 视图
webman默认使用的是php原生语法作为模版，在打开`opcache`后具有最好的性能。除了php原生模版，webman还提供了[Twig](https://twig.symfony.com/doc/3.x/)、 [Blade](https://learnku.com/docs/laravel/8.x/blade/9377)、 [think-template](https://www.kancloud.cn/manual/think-template/content) 模版引擎。

## 开启opcache
使用视图时，强烈建议开启php.ini中`opcache.enable`和`opcache.enable_cli` 两个选项，以便模版引擎达到最好性能。


## 安装Twig
1、composer安装

`composer require twig/twig`

2、修改配置`config/view.php`为
```php
<?php
use support\view\Twig;

return [
    'handler' => Twig::class
];
```
> **提示**
> 其它配置选项通过options传入，例如  

```php
return [
    'handler' => Twig::class,
    'options' => [
        'debug' => false,
        'charset' => 'utf-8'
    ]
];
```


## 安装Blade
1、composer安装

```
composer require psr/container ^1.1.1 jenssegers/blade:~1.4.0
```

2、修改配置`config/view.php`为
```php
<?php
use support\view\Blade;

return [
    'handler' => Blade::class
];
```

## 安装think-template
1、composer安装

`composer require topthink/think-template`

2、修改配置`config/view.php`为
```php
<?php
use support\view\ThinkPHP;

return [
    'handler' => ThinkPHP::class,
];
```
> **提示**
> 其它配置选项通过options传入，例如

```php
return [
    'handler' => ThinkPHP::class,
    'options' => [
        'view_suffix' => 'html',
        'tpl_begin' => '{',
        'tpl_end' => '}'
    ]
];
```

## 原生PHP模版引擎例子
创建文件 `app/controller/User.php` 如下

```php
<?php
namespace app\controller;

use support\Request;

class User
{
    public function hello(Request $request)
    {
        return view('user/hello', ['name' => 'webman']);
    }
}
```

新建文件 `app/view/user/hello.html` 如下

```html
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <title>webman</title>
</head>
<body>
hello <?=htmlspecialchars($name)?>
</body>
</html>
```

## Twig模版引擎例子

修改配置`config/view.php`为
```php
<?php
use support\view\Twig;

return [
    'handler' => Twig::class
];
```

`app/controller/User.php` 如下

```php
<?php
namespace app\controller;

use support\Request;

class User
{
    public function hello(Request $request)
    {
        return view('user/hello', ['name' => 'webman']);
    }
}
```

文件 `app/view/user/hello.html` 如下

```html
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <title>webman</title>
</head>
<body>
hello {{name}}
</body>
</html>
```

更多文档参考 [Twig](https://twig.symfony.com/doc/3.x/) 

## Blade 模版的例子
修改配置`config/view.php`为
```php
<?php
use support\view\Blade;

return [
    'handler' => Blade::class
];
```

`app/controller/User.php` 如下

```php
<?php
namespace app\controller;

use support\Request;

class User
{
    public function hello(Request $request)
    {
        return view('user/hello', ['name' => 'webman']);
    }
}
```

文件 `app/view/user/hello.blade.php` 如下

> 注意blade模版后缀名为`.blade.php`

```html
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <title>webman</title>
</head>
<body>
hello {{$name}}
</body>
</html>
```

更多文档参考 [Blade](https://learnku.com/docs/laravel/8.x/blade/9377)

## ThinkPHP 模版的例子
修改配置`config/view.php`为
```php
<?php
use support\view\ThinkPHP;

return [
    'handler' => ThinkPHP::class
];
```

`app/controller/User.php` 如下

```php
<?php
namespace app\controller;

use support\Request;

class User
{
    public function hello(Request $request)
    {
        return view('user/hello', ['name' => 'webman']);
    }
}
```

文件 `app/view/user/hello.html` 如下


```html
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <title>webman</title>
</head>
<body>
hello {$name}
</body>
</html>
```

更多文档参考 [think-template](https://www.kancloud.cn/manual/think-template/content)

## 模版赋值
除了使用`view(模版, 变量数组)`给模版赋值，我们还可以在任意位置通过调用`View::assign()`给模版赋值。例如：
```php
<?php
namespace app\controller;

use support\Request;
use support\View;

class User
{
    public function hello(Request $request)
    {
        View::assign([
            'name1' => 'value1',
            'name2'=> 'value2',
        ]);
        View::assign('name3', 'value3');
        return view('user/test', ['name' => 'webman']);
    }
}
```

`View::assign()`在某些场景下非常有用，例如某系统每个页面首部都要显示当前登录者信息，如果每个页面都将此信息通过 `view('模版', ['user_info' => '用户信息']);` 赋值将非常麻烦。解决办法就是在中间件中获得用户信息，然后通过`View::assign()`将用户信息赋值给模版。

## 关于视图文件路径

#### 控制器
当控制器调用`view('模版名',[]);`时，视图文件按照如下规则查找：

1. 非多应用时，使用 `app/view/` 下对应的视图文件
2. [多应用](multiapp.md)时，使用 `app/应用名/view/` 下对应的视图文件

总结来说就是如果 `$request->app` 为空，则使用 `app/view/`下的视图文件，否则使用 `app/{$request->app}/view/` 下的视图文件。

#### 闭包函数
闭包函数`$request->app` 为空，不属于任何应用，所以闭包函数使用`app/view/`下的视图文件，例如 `config/route.php` 里定义路由
```php
Route::any('/admin/user/get', function (Reqeust $reqeust) {
    return view('user', []);
});
```
会使用`app/view/user.html`作为模版文件(当使用blade模版时模版文件为`app/view/user.blade.php`)。

#### 指定应用
为了多应用模式下模版可以复用，view($template, $data, $app = null) 提供了第三个参数 `$app`，可以用来指定使用哪个应用目录下的模版。例如 `view('user', [], 'admin');` 会强制使用 `app/admin/view/` 下的视图文件。
