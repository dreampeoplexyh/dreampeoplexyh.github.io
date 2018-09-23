---
title: bootstrap基础知识(二)
date: 2018-09-15 13:51:23
tags:
categories: Bootstrap实战五部曲
---

bootstrap：来自twiter,是目前最受欢迎的前端开发框架之一，具体文档可以参考<a href="http://www.bootcss.com/">bootstrap 中文网</a>
<!--more-->
### bootstrap基本知识
    bootstrap 容器
    bootstrap栅格系统
    bootstrap 响应式查询区间
    bootstrap 表单
    bootstrap 按钮
    bootstrap 图片
    bootstrap 隐藏类
    bootstrap 字体图标
    bootstrap 下拉菜单
    bootstrap 导航条

### bootstrap 容器
```
containner-fluid 流体

containner
    * 1170px;
    * 970px;
    * 750px;
    * 100%;
```
### bootstrap 栅格系统
bootstrap 将页面横向分成12等分，按照12等分定义了不同宽度等分的样式，这些样式类组成了一套响应式，移动设备优先的流式栅格系统
![](bootstrap基础知识/栅格系统.png)

     * col-lg-1
     * col-md-1
     * col-sm-1
     * col-xs-1

     列偏移
     * col-lg-offset-1
     * col-md-offset-1
     * col-sm-offset-1
     * col-xs-offset-1

### bootstrap 表单
    * form 声明一个表单域
    * from-inline 内联表单域
    * from-horizontal 水平排列表单域
    * form-group 表单组，包括表单文字和表单窗体
    * from-contral 文本输入框，下拉列表控件样式
    * checkbox checkbox-inline 多选框样式
    * radio    radio-inline 单选框样式
    * input-group    表单控件组
    * input-groupaddon 表单控件组物件样式
    * form-group-lg   大尺寸表单
    * from-group-sm   小尺寸表单


### bootstrap 按钮
    * btn  声明按钮
    * btn-default 默认按钮样式
    * btn-primay
    * btn-success
    * btn-info
    * btn-warning
    * btn-danger
    * btn-link
    * btn-lg
    * btn-md
    * btn-xs
    * btn-block  宽度是父级宽100%的按钮
    * btn-group 定义按钮组
### bootstrap 图片
    * img-reposive 声明响应式图片
       * center-block  图片居中


### bootstrap 隐藏类
    * hidden-xs
    * hidden-sm
    * hidden-md
    * hidden-lg
### bootstrap 字体图标
<a href="https://v3.bootcss.com/components/">Glyphicons 字体图标</a>使用时直接将字体类名加上即可

### bootstrap 下拉菜单
    * dropdown-toggle
    * dropdown-menu


### bootstrap 选项卡
    * nav
    * nav-tabs
    * nav-pills
    * tab-content'


### bootstrap 导航条
    * navbar 声明导航条
    * navbar 声明默认的导航样式
    * navbar-defalult 声明反白的导航条样式
    * navbar-inverse  声明反白的导航条样式
    * navbar-static-top 去掉导航条的圆角
    * navba-fixed-top 固定到顶部的导航条
    * navbar-fixed-bottom 固定到底部的导航条
    * navbar-header 申明logo的容器
    * navbar-brand 针对logo等固定内容的样式
    * navbar-nav    定义导航条中的菜单
    * navbar-form 定义导航条中的表单
    * navbar-btn 定义导航条中的按钮
    * navbar-text 定义导航条中的文本
    * navbar-left 菜单靠左
    * navbar-right 菜单靠右
