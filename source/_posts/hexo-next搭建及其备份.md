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
**实现方法1**

只需要把HEXO根目录下的文件打包，发到qq某个自己的群里
即完成打包，下次写博客的时候只需要将博客的源文件发到qq群里就行

电脑换了的话只需要将里面的文件下载下来，安装完HEXO，配置完git秘钥到github上即可完成更新

**实现方法二2**

将github上的博客仓库分成两个分支master---hexo</br>
master分支用来存储hexo静态博客发布的文件</br>
hexo 用来存储源文件及其配置文件</br>
跟换电脑的时候只需要下载github上的hexo文件就可以了。然后就是和方法一一样完成配置跟新

**实现方法3**
我们之前说过hexo文件夹中文件的作用，那么我说下其实对我们源文件备份，更新最重要的文件主要有这么几个文件
```hexo
source           源文件
themes           主题文件
_config.yml      站点配置文件
```
我们备份的话可以选择这样，本地创建一个分支上传分支到github,这样我们就在github上就有两个分支，一个用来存静态博客，一个用来存源代码的。在存放hexo的文件夹下
```  
git init                             //将这个文件夹创建成git仓库
git checkout -b hexo                //hexo为分支名字，创建并进到hexo分支
git add .                          //将所有文件添加到缓存区
git commit -m "你觉得更新要注意的点"  //将缓存区的数据添加到git终极区
git remote add origin git@github.com:dreampeoplexyh/dreampeoplexyh.github.io.git                       //添加远程仓库
git push origin hexo                //推送hexo分支到github上对应的hexo分支上，github上如果么有hexo，github会自动创建爱你一个，或报错
```



那么日常跟新博客怎么办呢。直接
```
git chechout hexo           //切换到到hexo分区
git add .      //将跟新的东西放到缓存区里面
git commit -m "跟新说明"   //将缓存区的文件添加到git终极仓库中
git push origin hexo      //直接推送到hexo中 

删除远程文件的话可以删除本地文件，或更改，照着上面的再提交一遍就行了
```
换了电脑之后呢？怎么备份跟新呢？这是个好问题，我们可以这样做

```
git clone -b hexo git@github.com:dreampeoplexyh/dreampeoplexyh.github.io.git    //下载有源代码的hexo分支下的文件

npm install hexo-cli      //下载hexo
npm install               //下载跟新
npm install hexo-deployer-git   //下载上传到服务器端的

```
补充一些备份需要注意的点吧。首先满足的条件  
```
node.js
git
```
node.js 就是官网下载点下一步下步安装，当然这只是在windows下安装咯，还有其他操作系统以后再说。</br>
git默认一直点确定的话需要点确定的话需要配置环境变量</br>
就是直接进环境变量里将path里添加
```
E:\c盘的一些安装文件\Git\cmd    //看安装在那个盘，比如我就是安装在E盘下的c盘的一些安装文件这个文件下
```
git 的话还需要配置与github链接的秘钥，这样才好上传
配置的话其实也好办</br>
首先配置用户名和邮箱，运行git bash
```
git config --global user.name "你要写的名字，最好英文"
git config --global user.email "邮箱地址"
```
先看看有没有秘钥咯，基本没配的话基本没有，ssh公钥一般存在~/.ssh里面，看一看咯
```
cd ~/.ssh
ls
```
看看执行命令之后的ls有没有些文件，比如id_rsa和id_rsa.pub，这样的，有的话那好办，这个.pub的文件就是公钥，
执行
```
cat xxx.pub
```
把文件下的东西全都复制下来，粘贴到github上的setting 里面的ssh and GPG keys,添加进去就ok。</br>
假如没有这个秘钥的话也不要慌，慢慢的来</br>
```
ssh-keygen -t rsa -C "your_email@youremail.com"
Creates a new ssh key using the provided email # Generating public/private rsa key pair.

Enter file in which to save the key (/home/you/.ssh/id_rsa):
```
直接按Enter就行。然后，会提示你输入密码，如下(建议输一个，安全一点，当然不输也行，应该不会有人闲的无聊冒充你去修改你的代码)：
```
Enter same passphrase again: [Type passphrase again]
```
完了之后，大概是这样：
```
Your public key has been saved in /home/you/.ssh/id_rsa.pub.
The key fingerprint is: # 01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@youremail.com
```
1、查看你生成的公钥：
```
$ cat ~/.ssh/id_rsa.pub

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC0X6L1zLL4VHuvGb8aJH3ippTozmReSUzgntvk434aJ/v7kOdJ/MTyBlWXFCR+HAo3FXRitBqxiX1nKhXpHAZsMciLq8vR3c8E7CjZN733f5AL8uEYJA+YZevY5UCvEg+umT7PHghKYaJwaCxV7sjYP7Z6V79OMCEAGDNXC26IBMdMgOluQjp6o6j2KAdtRBdCDS/QIU5THQDxJ9lBXjk1fiq9tITo/aXBvjZeD+gH/Apkh/0GbO8VQLiYYmNfqqAHHeXdltORn8N7C9lOa/UW3KM7QdXo6J0GFlBVQeTE/IGqhMS5PMln3 admin@admin-PC
```
本地秘钥就有了，把公钥放进github的设置里面，然后测试看通不通咯
```
$ ssh -T git@github.com

Attempts to ssh to github
```
如果，看到：
```
Hi xxx! You've successfully authenticated, but GitHub does not # provide shell access
```
那么就连通了github了，
以上我觉得环境是配置好了，所有的部署都是在配置好环境之后才操作的

说实话，这玩意事是真的多，我也是无奈。哈哈



**实现方法四**</br>
我们之前说过，hexo最重要的三个文件就是
```hexo
source           源文件
themes           主题文件
_config.yml      站点配置文件
```
我们可以如上把上传到github上的文件拷贝下俩，直接提取这三个文件，其他文件可以删除了，然后怎么样呢，我们可以这样做</br>
在一个文件夹下</br>
```
npm install -g hexo-cli     //使用 npm 即可完成 Hexo 的安装。
hexo init                   //初始化hexo
npm install                 //安装完npm
```
ok了，直接将这三个文件替换就行了，后面的操作如上，下载上传插件，git上传秘钥配置。

## **0x04 博客自动化和图片改名**
> 每次都得hexo clean,hexo g,hexo s,hexo d,git等等，能不能一键自动化？好的
```
# os.chdir(path)改变当前工作目录到指定目录
# path -- 要切换到的新路径。如果允许访问返回 True , 否则返回False。
import os
import time
import sys
path = "E:/text1111111111/dreampeoplexyh.github.io"
# 查看当前工作目录
retval = os.getcwd()
print("当前工作目录为 %s" % retval)
# 修改当前工作目录
os.chdir(path)
# 查看修改后的工作目录
retval = os.getcwd()

def infor():
    print("输入1就是本地查看")
    print("输入2就是上传到github")
    print("输入到3就是备份到github上")
    print("输入到4就是正常退出")
def See():
    print("当前工作目录%s" %retval)
    os.system("hexo clean&&hexo g&&hexo s")
    time.sleep(3)
def NewSee():
    print("当前工作目录%s" %retval)
    os.system("hexo clean&&hexo g&&hexo d")
    time.sleep(3)
def git():
    str = input("你跟新了什么内容")
    time.sleep(3)
    os.system("git add .&&git commit -m'str'&&git push origin hexo")


if __name__ == '__main__':
    infor()
    while True:
        num = int(input("请输入你需要的功能提交的功能"))
        if num == 1:
            See()
        elif num == 2:
            NewSee()
        elif num == 3:
            git()
        elif num == 4:
            sys.exit()
```
缺点也有很多，以后慢慢完善吧，现在用着还挺爽的

> 每次做的图和截的图名字都好长啊，我还得一个个去改名字，能不能一键批量改啊，ok,一键批量从1到末尾改好名字

```
import os
str1 = input("请输入地址")
for i in str1:
    if i=="\\":
        str1 = str1.replace(i, "/")
    else:
        pass
path = str1
f = os.listdir(path)
n = 0
for i in f:
    old_name = path+"/"+f[n]
    new_name = path+"/"+str(n+1)+".jpg"
    os.rename(old_name, new_name)
    n+=1
    if n == len(f):
        break
```




参考下其他方法  [https://www.zhihu.com/question/21193762](https://www.zhihu.com/question/21193762)


