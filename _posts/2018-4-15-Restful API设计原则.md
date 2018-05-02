---
layout:     post
title:  "Restful api 设计原则"
subtitle:   ""
date:   2018-04-15 09:54:22 +0800
author:     "ONE"
header-img: "img/post-bg-2015.jpg"
tags:
    - 前端
---

##Restful api设计原则

- 在Restful API设计原则中，每个网页代表一种资源，所有网址中不能有动词，只能是名词。

<!-- more -->

- 通信协议为HTTPs协议
- API应部署在专用域名下，如果简单，可以考虑放在主域名下。
- API的版本号放入URL中

#### 资源的操作类型有以下几种（HTTP动词）

- GET（SELECT）：从服务器取出资源（一项或多项）
- POST（CREATE）：在服务器新建一个资源。
- PUT（UPDATE）：更新资源（客户端提供改变后的完整资源）
- PATCH（UPDATE）：更新资源（客户端提供改变后的属性）
- DELETE（DELETE）：删除资源。
- HEAD：获取资源元数据（不常用）
- OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的。

#### 过滤信息

- 如果记录的数据很多，服务器不可能全部返回给用户，API要提供参数过滤返回结果。

- 常见参数如下：

  ```
  ?limit=10：指定返回记录的数量 

  ?offset=10：指定返回记录的开始位置. 

  ?page=2&per_page=100：指定第几页，以及每页的记录数。 

  ?sortby=name&order=asc：指定返回结果按照哪个属性排序，以及排序顺序。 

  ?animal_type_id=1：指定筛选条件
  ```


#### 状态码

```
200 OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
202 Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
204 NO CONTENT - [DELETE]：用户删除数据成功。
400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
403 Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
422 Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。
```

- 详细列表请见[这里](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)

#### 错误处理

- 状态码是4xx

```
{
  error: "Invalid API key"
}
```

#### Hypermedia API

- 在返回结果中提供链接

```
{"link": {
  "rel":   "collection https://www.example.com/zoos",
  "href":  "https://api.example.com/zoos",
  "title": "List of zoos",
  "type":  "application/vnd.yourformat+json"
}}
```

#### 其他

API的身份认证应该使用OAuth2.0框架 
服务器返回的数据格式尽量用JSON，避免XML