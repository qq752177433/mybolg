---
title: flex和grid实现网页圣杯布局
date: 2018-03-12 08:53:49
tags:
  - css3
  - 布局
categories:
  - css
  - layout
toc: true
---
### 前言
随着前端技术的越来越发达，一些新的布局渐渐的被大家广泛认知和认可，并且跟着趋势，人们已经开始广泛的运用起来了，作为前端开发人员，认识这些新的布局还是很有必要，这里主要记录了两种网页常需布局-**圣杯布局**用新的布局方式**flex**,**frid**布局实现。

参考文档: [flex布局语法](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html),[学习grid布局](https://www.w3cplus.com/css/learncssgrid.html)
<!-- more -->
### 需求
所谓圣杯布局，实现的是三栏布局，随着页面的宽度的变化，这三栏布局是中间盒子优先渲染，两边的盒子框子固定不变，即使页面宽度变小，也不影响我们的浏览
![](t7.png)
### flex布局实现方式
#### flex定义
Flex 是 Flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。采用 Flex 布局的元素，称为 Flex 容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称"项目"。它已经得到了所有浏览器的支持。
#### 初始布局
**html**
```
<div id="flex-container">
  <div class="header">Header</div>
  <div class="container">
    <div class="flex-items main">Container</div>
    <div class="flex-items nav">Nav</div>
    <div class="flex-items aside">Aside</div>
  </div>
  <div class="footer">Footer</div>
</div>
```
**css**
```
#flex-container{
  display: flex;
}
#flex-container .header,#flex-container .footer{
  padding: 25px;
}
#flex-container .header{
  background: #b03532;
}
#flex-container .footer{
  background: #da6f2b;
}
#flex-container .container{
  display: flex;
}
#flex-container .container div{
  padding: 25px
}
#flex-container .container .nav{
  background: #33a8a5;
}
#flex-container .container .main{
  background: #30997a;
}
#flex-container .container .aside{
  background: #6a478f;
}
```
**说明：**这里使用两个flex容器盒子，一个是最外面那层的，用于从上到下排列（具体我们还没添加样式，由于要全部用flex布局，所以干脆就全部flex布局了，当然这里也可以不用flex布局），另一个适用于主题页面的弹性布局（这个才是重点）。
效果图如下：

![](t9.png)

#### 具体布局
我们先把从上到下的布局实现，由于flex布局的布局方向默认是从左到右的，这里我们通过`flex-direction`属性改变方向就行
```
#flex-container{
  display: flex;
  flex-direction: row;
}
```
效果如下：

![](t10.png)

主题部分分配宽度
```
#flex-container .container .nav{
  background: #33a8a5;
  width: 300px;
}
#flex-container .container .main{
  background: #30997a;
  flex:1;
}
#flex-container .container .aside{
  background: #6a478f;
  width: 300px;
}
```
**说明：**这里的`flex:1`实际上是`flex-grow`,`flex-shrink`和`flex-basis`的缩写默认值为`0 1 auto`
对应的是**放大比例**，**缩小比例**，**剩余空间分配**
这里简写为`flex:`1他会根据剩余宽度来计算，因为该容器下没有其他用`flex`属性来定义的宽，所以这里就会自动填满，详情请看参考文档[flex布局语法](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)。

接下来改变main和nav的位置，使用order属性，用于项目排列。
```
#flex-container .container .nav{
  background: #33a8a5;
  width: 300px;
  order:-1;
}
```
**说明：**flex容器下每个子项目的order默认为0,设置负数可以使其排列最靠前

最终效果如下：

![](t11.png)

这样就基本完成了布局，我们可以给container里面多加点内容测试一下。

![](t12.png)
### gird布局实现方式
#### grid定义
CSS Grid布局 （又名"网格"），是一个基于二维网格布局的系统，主要目的是改变我们基于网格设计的用户接口方式。你只需要定义一个容器元素并设置display：grid，使用grid-template-columns 和 grid-template-rows属性设置网格的列与 行的大小，然后使用grid-column 和 grid-row属性将其子元素放入网格之中。目前已被大多数主流浏览器支持。
#### 初始布局
**html**
```
<div id="grid-container">
    <div class="grid-items header">Header</div>
    <div class="grid-items container">Container</div>
    <div class="grid-items nav">Nav</div>
    <div class="grid-items aside">Aside</div>
    <div class="grid-items footer">Footer</div>
</div>
```
**css**
```
#grid-container{
  display: grid; //主容器定义为grid布局
}
#grid-container .grid-items{
  padding:25px;
  text-align:center;
}
#grid-container .header{
  background: #b03532;
}
#grid-container .nav{
  background: #33a8a5;
}
#grid-container .container{
  background: #30997a;
}
#grid-container .aside{
  background: #6a478f;
}
#grid-container .footer{
  background: #da6f2b;
}
```
效果如下：

![](t2.png)

#### 具体布局
接下来定义列数，由于是三栏布局所以三列足矣
```
#grid-container{
  display: grid;
  grid-template-columns: 300px 1fr 300px; //colmns是列数左右固定300px，fr可以自动根据网格容器的宽度来计算列的宽度
}
```
效果如下：

![](t3.png)

header,footer要求填满一行，可以使用 grid-colmn来定义
```
#grid-container .header{
  background: #b03532;
  grid-column: 1 / 4;
}
#grid-container .footer{
  background: #da6f2b;
  grid-column: 1 / 4;
}
```
效果如下：

![](t4.png)

**这里要说明一下：**`grid-column`属性是`grid-column-start`和`grid-column-end`的简写，可以写成`grid-column-start:1;grid-column-end:4`,表示当前网格元素所占的位置，关于这里的1和4其实就是线，我们之前定义了3列所以一共有4条线，请看下图，同样要定义他的行线可以使用`grid-row`属性

![](t5.png)

上图所示的有两列3行的网格，所以列线为3，行线为4。
由于我们要求的是container优先加载，所以这里的container的确优先加载了，但是位置并不是在中间，我们要通过`grid-column`和`grid-row`改变网格所属线的位置来改变网格位置。我们先改变container的位置,让他占当行的第二格的位置。
```
#grid-container .container{
  background: #30997a;
  grid-column: 2 / 3;
}
```
效果如下：

![](t6.png)

可以发现container虽然到位了，但是之前的那个位置空了，剩下的网格就会继续往下排列，所以我们要把nav填充回第一个网格中
```
#grid-container .nav{
  background: #33a8a5;
  grid-row:2/3;
}
```
**说明一下：**这里使用`grid-row:2/3;`而不是`grid-column: 1 / 2;`，由于之前我们把container调用到了第二格，所以之前那一格是空的，网格的排列顺序是一直往下排列，就算这里给nav设置`grid-column: 1 / 2;`那么他也只会出现在第三行的开头，设置`grid-row`可以让他重新在第二行进行排列，先看第一格有没有，没有就填充，有就继续往下找。  ----个人理解，事实好像也差不多啥

最终效果图：

![](t7.png)

这样就基本完成了布局，我们可以给container里面多加点内容测试一下。

![](t8.png)
