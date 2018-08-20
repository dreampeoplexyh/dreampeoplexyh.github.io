---
title: HEXO+NEXT 搭建及其备份
date: 2018-08-09 11:36:45
tags:
---

**hexo 官网地址：https://hexo.io/  是一个基于Node.js的静态博客程序，可以方便的生成静态网页托管在github和Heroku上**</br>
**next 官网地址：http://theme-next.iissnan.com 是hexo的一种主题，以精于心，简于形的思想而获得极多博客爱好者追求。**</br>
<!--more-->
### 简介
    安装hexo+next
    安装插件
    备份博客

## **0x00 安装hexo+next**   </b>
####  **安装前提** </br>
- [Node.js ](https://nodejs.org/en/) <br>
- [git](https://git-scm.com/)       </br>

####   **安装HEXO**
在windows的DOS下的**某个文件**夹下输入</br>

```hexo
npm install -g hexo-cli
hexo init
npm install
```


</br>
<div align=center>
![文件目录](hexo-next搭建及其备份\文件目录.png)
</div>


   
#####  **安装NEXT**
```hexo
git clone https://github.com/iissan/hexo-theme-next themes-next
```


在站点文件下将主题转移到next上
在dos下移动到站点安装hexo的文件下，执行
```hexo
hexo s
```


在本地的浏览器中输入[http://localhost:4000][1]查看本地博客</br>

----------

##  **0x01 上传博客github**   </b>
###   **上传前提** </br>

github账号
上传本地git秘钥到github
安装hexo上传插件
###  github创建仓库</br>
github直接官网注册就行</br>
创建一个public账户 账户名是yourname.github.io  | usname就是你创建github的名字</br></br>
####  **git中创建ssh**</br>
配置gihtub账户信息：
```hexo
git config --global user.name "yournanme"
git config --global user.email "yourEmail"
```
在git--bash中输入：
```hexo
ssh-keygen -t rsa -C "youremail@example.com
```

查看公钥</br>
```hexo
cd ~/.ssh</br>
cat id_rsa.pub</br>
```
将公钥内容复制到github上的 SSH Keys
修改站点文件_config.yml</br>
```hexo
deploy:
   type: git@github.com:xxx.github.io.git
   repo: YourgithubName.github.io.git
   branch: master
```

安装github上传插件
npm install hexo-deployer-git --save


#### **上传流程**
hexo clean
hexo g
hexo d



----------


##  **0x02扩展插件**

- 设置博文内链接为蓝色
- 设置文章末尾“本文结束”标记
- 显示每篇文章的字数
- 实现点击出现的桃心效果
- 修改字体颜色/大小/背景色
- 添加RSS
- 添加菜单选项
####  **设置博文内链接为蓝色**

修改方法：
```hexo
 F:\blog\themes\next\source\css\_common\components\post
```



打开post.styl文件，在文件中末尾添加，如下字段：

```hexo
     .post-body p a{
      color: #0593d3;
       border-bottom: none;
       &:hover {
         color: #0477ab;
         text-decoration: underline;
       }
     }
```



####  **设置文章末尾"本文结束"标记**
修改方法:
F:\blog\themes\next\layout\_macro新建一个文件passage-end-tag.swig,文件内容中添加以下代码：
```
     {% if theme.passage_end_tag.enabled %}
       <div style="text-align:center;color: #ccc;font-size:14px;">
        ------ 本文结束感谢您的阅读 ------</div>
       {% endif %}
```
 


这里可以更改字体显示的颜色，大小，以及内容，例如可将本文结束用The Happy Ending代替，并将字体颜色设置为了自己喜欢的#CDBA96，你可以去这里选择自己喜欢的颜色对应的RGB值。

然后在路径\themes\next\layout\_macro\下找到并打开post.swig文件，在 post-body 之后， post-footer 之前添加如下代码（post-footer之前两个DIV）：

```
     {% if not is_index %}
     <div>
     {% include 'passage-end-tag.swig' %}
     </div>
     {% endif %}
```


最后在主题配置文件 中，在末尾添加如下语句：
```
passage_end_tag:
enabled: true
```



#### **显示每篇文章的字数**
实现方法
安装插件
```
npm install hexo-wordcount --save
```


然后修改主题配置文件 ，定位到post_wordcount，将wordcount由false改为true即可。

####  **点击出现桃心效果**
打开浏览器，输入如下网址
[http://7u2ss1.com1.z0.glb.clouddn.com/love.js](http://7u2ss1.com1.z0.glb.clouddn.com/love.js)

然后将里面的代码copy一下，新建love.js文件并且将代码复制进去，然后保存。将love.js文件放到路径/themes/next/source/js/src里面，然后打开\themes\next\layout\_layout.swig文件,在末尾（在前面引用会出现找不到的bug）添加以下代码：

```
<!-- 页面点击小红心 -->
<script type="text/javascript" src="/js/src/love.js"></script>
```




####  **修改字体颜色/大小/背景色**
比如说，想在文章中对某一部分的文字进行强调（改变大小，颜色），该操作具体说明如下：

如果想自定义字体大小以及颜色，可以直接在 Markdown 文档中使用 html 语法：
```hexo
    <font size=4 > 这里输入文字，自定义字体大小 </font>
    <font color="#FF0000"> 这里输入文字，自定义字体颜色</font> 
    <span style="background-color: #ff6600;">这里输入文字，自定义字体背景色</span>
    <font color="#000000" size=4><span style="background-color: #ADFF2F;">这是综合起来的效果 </span></font>
    <font color="#FFFFFF" size=4><span style="background-color: #68228B;">这是综合起来的效果2 </span></font>
```



####  **添加RSS**
操作具体说明如下
 npm install hexo-generator-feed --save

在站点配置文件 中追加如下代码：
```hexo
feed:
type: rss2
path: rss2.xml
limit: 10
hub:
content: 'true'
```


----------


####  **添加菜单选项**
 实现方法

默认情况下，菜单导航栏有首页、归档、关于三个选项，除此之外笔者还添加了分类、标签和关于。在主题配置文件 中，找到menu属性，并去掉categories、 tags、about的的注释，如下图所示：
```hexo
    // 添加分类页面
    hexo new page "categories"
    // 添加标签页面
    hexo new page "tags"
    // 添加关于页面
    hexo new page "about"
```


执行完上述指令后，在Hexo根目录/source/文件夹下创建三个文件夹，命名分别为：categories、tags、about文件夹，在这些文件夹中分别会创建一个以index命名的Markdown文件，对这三个Markdown文件内容进行修改，使之分别为：
```hexo
  ---
    title: categories
    date: 2017-03-12 22:06:24
    type: "categories"
    ---

    ---
    title: 标签
    date: 2017-03-12 17:27:16
    type: "tags"
    ---

    ---
    title: about
    date: 2017-03-12 22:07:26
    type: "about"
    ---
```



----------

## **0x03 博客备份**
实现方法1

只需要把HEXO根目录下的文件打包，发到qq某个自己的群里
即完成打包，下次写博客的时候只需要将博客的源文件发到qq群里就行

电脑换了的话只需要将里面的文件下载下来，安装完HEXO，配置完git秘钥到github上即可完成更新

实现方法二2

将github上的博客仓库分成两个分支master---hexo</br>
master分支用来存储hexo静态博客发布的文件</br>
hexo 用来存储源文件及其配置文件</br>
跟换电脑的时候只需要下载github上的hexo文件就可以了。然后就是和方法一一样完成配置跟新

参考下其他方法  [https://www.zhihu.com/question/21193762](https://www.zhihu.com/question/21193762)


  [1]: http://localhost:4000