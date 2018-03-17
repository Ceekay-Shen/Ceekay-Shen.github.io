---
layout:     post
title:      "Markedown 语法"
subtitle:   " \"markdown, 笔记, blog\""
date:       2018-03-17 13:00:00
author:     "Shen"
header-img: "img/markdown-bg-new.jpg"
catalog: true
tags:
    - markdown
    - 笔记
    - 语法
---

> “You need learning markdown. ”  


# Markdown begin!!!

## 前言

这几天开始尝试使用markdown来写blog，发现这个语法还是非常简单明了，所以觉得有必要学一学顺便推荐给大家。

## What is Markdown

Markdown是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。Markdown的目标是实现[易读易写]。

具体的详细介绍可以上[wiki看看](https://en.wikipedia.org/wiki/Markdown)。

## let`s learning markdown

### [一]  标题
标题使用 "#" 来标注，共六级。  
[H1 ~  H6]  
[#  ~  ######]  
一般来说#作为文章的大标题，只有一个，###作为段落标题。

*代码*  

```  
# h1  
## h2  
### h3  
#### h4  
##### h5  
###### h6  
####### h7  
######## h8  

```  

**演示**  

# h1  
## h2  
### h3  
#### h4  
##### h5  
###### h6  
####### h7  
######## h8 

### [二]  分级标题
分级标题使用 "=" "-"，最少只可以写一个，兼容性一般。  

**代码**  

```  
 一级标题  
====================
 二级标题  
--------------------

```

**演示**  

 标题  
====================  
 标题  
--------------------  


### [三]  目录   
根据标题生成目录，兼容性一般

**代码**  
  
```
[TOC]
```

### [四]  引用  

代码1（单行式）

```
> hello world!
```

**演示**
>hello world!

代码2（多行式）  

```
>hello world!
hello world!
hello world!

或者
>hello world!
>hello world!
>hello world!

```
**演示**

相同的结果

>hello world!  
>hello world!  
>hello world!  

代码3（多层嵌套）

**代码**  

```
> hello world!  
>> hello country!  
>>> hello city!  
```

**演示**

> hello world!  
>> hello country!  
>>> hello city!  

### [五] 行内标记

用'`'标记**代码块将变成一行**

**代码**  
```
标记之外`hello world`标记之外
```  

**演示**  

标记之外`hello world`标记之外

***错误代码***  
不同平台错误略有差异  

```
 标记之外 ` 
< div>   
    < div></div>
    < div></div>
    < div></div>
< /div>
`标记之外
```

**演示**

 标记之外 ` 
< div>   
    < div></div>
    < div></div>
    < div></div>
< /div>
`标记之外

### [六] 代码块
与上行距离空一行

**代码1（```）**   

使用```生成块
```
    ```
    <div>   
        <div></div>
        <div></div>
        <div></div>
    </div>

    ```
```
**演示**

<div>   
    <div></div>
    <div></div>
    <div></div>
</div>


**代码2（Tab）**  

Tab缩进

```
我是文字.....
    <div>   
        <div></div>
        <div></div>
        <div></div>
    </div>
```

**演示**

我是文字.....
    <div>   
        <div></div>
        <div></div>
        <div></div>
    </div>

**代码3(自定义语法)**  
注：根据不同的语言配置不同的代码着色
```
    ```javascript
    var num = 0;
    for (var i = 0; i < 5; i++) {
        num+=i;
    }
    console.log(num);
    ```
```

**演示**

```javascript
var num = 0;
for (var i = 0; i < 5; i++) {
    num+=i;
}
console.log(num);
```


## 后续

今天先写到这，有时间在继续整理。