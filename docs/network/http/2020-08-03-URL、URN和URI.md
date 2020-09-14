---
layout:     post
title:      "URL、URN和URI"
subtitle:   "指点迷津～"
date:       2020-08-03
author:     "Rain"
header-img: "img/post-bg-2020-08-03.jpg"
tags:
    - http
    - uri


---

## 什么是URI

URI(Uniform Resource Identifier,统一资源标识符)使用一连串的字符串来标识一个唯一的资源。

- **统一（Uniform）**：需要事先设定一系列语法规则来保证，但是需要保证可扩展性；
  - 允许不同种类的资源在同一上下文中出现
  - 对不同种类的资源标识符可以使用同一种语义进行解读
  - 引入新的标识符时，不会对已有的标识符产生影响
  - 允许同一资源标识符在不同的、Internet规模下的上下文中出现
- **资源（Resource）**：可以是图片、文档等
- **标识符（Identifier）**：将当前资源与其他资源区分开的名称

> 通常包括两种，**URL（统一资源定位符）和URN（统一资源名称）**

## 什么是URN

URN是一种URI，用来在一个特定的命名空间中用名称来标识一个资源。

例如ISBN，`*urn:isbn:0-486-27557-4*`可以标识一个唯一的书名号

## 什么是URL

URL也是URI的一种，标识了一种资源所在的唯一位置，在网络上能找到。在http和https的schema情况下，通常URL和URI是指的同一个东西。

## URI的语法规则

> ```
> URI = scheme:[//authority]path[?query][#fragment]
> ```
> ```
> authority = [userinfo@]host[:port]
> ```

- **scheme**:大小写不敏感，包括http、https、ftp、mailto,file, data等
- **authority**:认证信息，例如admin:pass@localhost:8080
- **path**:路径信息
- **query:**参数信息
- **fragment**:`#`锚点

## 为什么要进行URI编码

### 需要进行URI编码的情况

- 传递数据中，存在用作分隔符的保留字符
- 对可能产生歧义性的数据要进行编码
  - 不在ASCII码范围中的字符
  - ASCII码中不可显示的字符
  - URI中规定的保留字符
  - 不安全字符（传输环节中可能会被不正确处理），如空格、引号、尖括号等。

### URI百分号编码

- 百分号编码方式
- 非ASCII码字符（例如中文）：建议先UTF8编码，在US-ASCII编码
- 对于URI合法字符，编码与不编码是等价的