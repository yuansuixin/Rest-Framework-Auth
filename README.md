# Rest-Framework源码解析----登录认证


### 认证
- 问题，有些API需要用户登录成功之后，才能访问，有些无需登录就能访问。
- 基本使用认证组件
解决：
    - 创建两张表
    - 用户登录认证（返回token并保存到数据库）


- 认证流程原理：

![Untitled-1-2018418201518](http://p693ase25.bkt.clouddn.com/Untitled-1-2018418201518.png)

- 再看一遍源码（看配置）
    - 认证的配置可以局部视图使用还可以全局使用
    - 全局使用认证只需要在settings里配置好就可以，默认就是调用settings里的配置
    - 匿名时request.user = None
    `` authentication_classes = api_settings.DEFAULT_AUTHENTICATION_CLASSES``


# 对token使用md5摘要


- 内置的认证类
    - 基于Django做的，本质上都是把你的用户信息给我，我去数据库检测一下你是否认证成功，一般用不到，更趋向于使用自定义的
    - 认证类，必须继承，``from rest_framework.authentication import BaseAuthentication``
    - 其他认证类，BaseAuthentication，流程其实就是浏览器对你的用户名秘密（浏览器做的那个对话框里面的用户名密码）使用base64加密，加密之后放到请求头里面发过去
![](index_files/509953468.png)


###### 梳理
1. 使用
    - 创建类，继承BaseAuthentication，实现authenticate()，authenticate_header()方法
    - 返回值
        - None，下一次认证来执行，
        - raise exceptions.AuthenticationFailed('用户认证失败')  # from rest_framework import exceptions
        - 返回元组，（元素1，元素2），元素1赋值给request.user 元素2赋值给request.auth
    - 局部使用
        - 在视图类中写个单独的字段 `` # authentication_classes = [Authtication, ]``
        - 全局使用,在配置文件settings中设置


2. 源码流程


[用户登录认证详细解析](https://yuansuixin.github.io/2017/03/20/rest-framework-auth/ "用户登录认证详细解析")









