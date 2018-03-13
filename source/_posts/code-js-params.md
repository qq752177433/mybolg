---
title: JS字符串数组操作之url参数操作
date: 2018-03-12 22:19:35
tags:
  - code
categories:
  - javascript
  - 笔记
toc: true
---
### 前言
字符串的操作和数组的操作是js的必修课，熟练掌握是必修课，省的自己去百度，而获取url上的参数也是项目中必不可少的功能之一，当然网上这些代码很多，需要的时候自己去拷贝就行了，但是还是决定自己用js写一下，一是为了巩固字符串和数组的操作，二也是为了加固自己的基础.

**文档参考：**[js常见字符串操作方法](http://www.jb51.net/article/65358.htm),[js常见数组操作方法](http://www.jb51.net/article/49896.htm)
<!-- more -->
### 用js数组和字符串操作的方式实现
**这里不考虑正则，虽然正则会更简单一点，为了练习嘛。**（当然自己正则用的也很不熟练，以后还得补点课，这样就不用一些简单的匹配规则都要百度了）
下面进入正题：
#### 获取url中的参数
```
//获取参数方法
function getParam(url,key){
  //判断是否带参没带直接返回空
  if(url.indexOf("?") == -1)
  return "";
  //带参且带有该参数 返回该参数的值
  let url_split = url.split("?");
  let url_params = url_split[1].split("&");
  for(let i = 0;i < url_params.length;i++){
    let url_param = url_params[i].split("=");
    if(url_param[0] == key){
      return url_param[1];
    }
  }
  //带参但未带有该参数，返回空
  return "";
}
```
#### 修改url中的参数
```
//设置参数方法
function setParam(url,key,value){
  //判断是否带参没带直接加上key=value
  if(url.indexOf("?") == -1)
  return url + "?" + key + "=" + value;
  //下面是带了该参数替换value值
  let url_split = url.split("?");
  let url_params = url_split[1].split("&");
  for(let i = 0;i < url_params.length;i++){
    let url_param = url_params[i].split("=");
    if(url_param[0] == key){
      url_param[1] = value;
      url_params[i] = url_param.join("=");
      return url_split[0] + "?" + url_params.join("&");
    }
  }
  //这里是没带该参数添加key=value值
  return url + "&" + key + "=" + value;
}
```
#### 删除url中的参数
```
//删除参数方法
function delParam(url,key){
  //判断是否带参，没带不做处理
  if(url.indexOf("?") == -1)return url;
  //带参数并且存在该key值就删除该参数
  let url_split = url.split("?");
  let url_params = url_split[1].split("&");
  for(let i = 0;i < url_params.length;i++){
    let url_param = url_params[i].split("=");
    if(url_param[0] == key){
      let splice_param = url_params.splice(i,1);
      if(splice_param!=[]){
        return url_split[0] + "?" + url_params.join("&");
      }
    }
  }
  //带参数并且不存在该key值就不做任何处理
  return url;
}
```
算是一个自己写的小方法吧，主要用了一些字符串和数组处理的方法，用的不多但是勉强可以完成功能
