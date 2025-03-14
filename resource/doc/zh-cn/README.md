# webman是什么

webman是一款基于[workerman](https://www.workerman.net)开发的高性能HTTP服务框架。webman用于替代传统的php-fpm架构，提供超高性能可扩展的HTTP服务。你可以用webman开发网站，也可以开发HTTP接口或者微服务。

除此之外，webman还支持自定义进程，可以做workerman能做的任何事情，例如websocket服务、物联网、游戏、TCP服务、UDP服务、unix socket服务等等。

# webman理念
**以最小内核提供最大的扩展性与最强的性能。**

webman仅提供最核心的功能(路由、中间件、session、自定义进程接口)。其余功能全部复用composer生态，这意味着你可以在webman里使用最熟悉的功能组件，例如在数据库方面开发者可以选择使用Laravel的`illuminate/database`，也可以是ThinkPHP的`ThinkORM`，还可以是其它组件如`Medoo`。在webman里集成他们是非常容易的事情。

# webman具有以下特点

1、高稳定性。webman基于workerman开发，workerman一直是业界bug极少的高稳定性socket框架。

2、超高性能。借助于workerman高性能以及对HTTP服务深入优化，webman性能高于传统php-fpm框架10-100倍左右，比go的gin echo等框架性能高一倍左右。

3、高复用。无需修改，可以复用绝大部分composer组件及类库。

4、高扩展性。支持自定义进程，可以做workerman能做的任何事情。

5、超级简单易用，学习成本极低，代码书写与传统框架没有区别。

6、使用最为宽松友好的MIT开源协议。

# 项目地址
GitHub: https://github.com/walkor/webman **不要吝啬你的小星星哦**

码云: https://gitee.com/walkor/webman **不要吝啬你的小星星哦**

# 第三方权威压测数据

## [techempower.com](https://www.techempower.com/benchmarks/#section=data-r20&hw=ph&test=db&l=zik073-sf) (带数据库业务)


![](../assets/img/benchmark1.png)

带数据库查询业务，webman单机吞吐量达到39万QPS，比传统php-fpm架构的laravel框架高出近80倍。


![](../assets/img/benchmarks-go.png)

带数据库查询业务，webman比同类型go语言的web框架性能高一倍左右。



