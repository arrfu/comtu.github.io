---
layout : post
title : "免费搭建个人博客的几种高效方案"
category : Jekyll
duoshuo: true
date : 2016-12-01
tags : [Jekyll , hexo, wordpress,blog]
---

******

**一、博客搭建可行性方案：** 

就我目前所了解到的，较多人采用的博客搭建方案有如下几种：   
* 1.Git+Github+Markdown+jekyll （免费）  
* 2.Git+Github+Markdown+hexo   （免费）
* 3.虚拟主机＋插件＋Wordpress  （付费）

个人有个不成熟的小建议：
  * 如果你不想付费，也不想备案，那你基本上就已经确定了前面两种方案了，免费版走起。
  * 如果你是高富帅，或者要求较高又不想浪费多余精力在搭建博客上面，那强力推荐采用第三个方案。轻松加愉快，爽的飞起。   
  
**二、博客主题选择：**

***1)、jekyll***

[官网：http://jekyllrb.com](http://jekyllrb.com)   
[jekyll主题：http://www.zhanxin.info/themes.html](http://www.zhanxin.info/themes.html)      
[搭建教程：http://www.arrfu.com/windows_configuration_Jekyll.html](http://www.arrfu.com/windows_configuration_Jekyll.html)   

####优点：
* 1、jekyll是一个静态文件生成器，网站不需要数据库，只要把自己的博客放到对应的目录即可。
* 2、能部署到github或者gitcafe上，不需要自己的vps，因为是静态的，迁移起来非常方便。
* 3、原生支持markdown。现在github默认支持jekyll, 所以原生的文件如果放到github上，它会自动帮你生成静态网站。
* 4、相对hexo而言，可以直接在github网页版上编辑和发布博客，PC间切换和同步非常方便。（这点本人非常喜欢）
    
####缺点：
* 1、jekyll用的liquid语法确实不是对程序员友好的，。不过jekyll功能比hexo强大很多，有时间折腾的可以选它。
* 2、相对Wordpress而言，没有强大的后台和插件支持，学习成本较高，需要一些网页基础。

***2)、hexo主题***
 
[hexo主题：https://hexo.io/themes/](https://hexo.io/themes/)      
[搭建教程：http://baixin.io/](http://baixin.io/2015/08/HEXO%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)  

    Hexo是一个基于node.js的静态博客生成系统，它使用markdown语法来写作，同时支持丰富的自定义标签系统。用户在本地安装Hexo系统并进行写作，通过一条命令，Hexo可以自动生成静态页面，并发布到多个平台上。与传统的博客相比，Hexo可以说是一个本地运行远程发布的博客程序。   
####优点：
* 1、搭建的博客平台，速度快，免费，可以搭建在 Github 上。
* 2、操作比 Jekyll 简单，命令少，易于记忆。 3.支持markdown，Hexo最终生成的是一个静态博客，这就意味着它拥有其他博客系统无法比拟的低负载与高速度的特性。
    
####缺点：
* 1、每次在一台新电脑或者别人电脑首次使用时，都要重新安装和配置编译环境，不适合随时随地愉快的写博客。（不能优雅的装逼，略微不爽）
* 2、相对Wordpress而言，没有强大的后台和插件支持，学习成本较高，需要一些网页基础。

***3)、wordpress***

####优点：   
* 1、安装简单方便，甚至很多虚拟主机供应商都提供了Wordpress的一键式安装工具。用户连上传文件的步骤都省了。   
* 2、功能强大，可扩展性高，丰富的插件使用起来更加方便。   
* 3、wordpress搭建的博客对seo搜索引擎友好，收录也快，排名靠前。
       
####缺点：
* 1、对域名空间要求，wp需要自己购买虚拟主机，低配版大概两百多块。
    
* 2、迁移成本高，且插件装多了会变慢。
    
* 3、Wordpress对于中小型网站应该是不错的选择，但对于大型的门户网站，数据库、用户管理、内容的分类管理等方面的限制，还是会让Wordpress会有些力不从心的吧。  

[安装搭建教程：http://ztmao.com/jiaocheng/2352.html](http://ztmao.com/jiaocheng/2352.html)

**三、部署博客的主机选择**

* 1、github

* 2、coding

* 3、国内付费主机(阿里云，西部数码等)

***github:***

####优点：
* 1、对于喜欢经常逛github的童鞋来说，把博客部署在github上或许是再合适不过了。可以直接在github上编辑和发布博客。
* 2、免费，稳定，使用人多，且对google引擎友好度高。

####缺点：
* 1、国内访问速度慢。
* 2、百度收录几乎没有，原因是github禁止百度爬虫抓取数据。(这点无疑是巨大的缺点，毕竟在天朝混，使用百度的用户数量还是相当大的)
    
***为什么 Github Pages 禁用了百度爬虫？***   
官方给出的回复如下：

```
Sorry for the trouble with this. We are currently blocking the Baidu user agent from crawling GitHub Pages sites in response to this user agent being responsible for an excessive amount of requests, which was causing availability issues for other GitHub customers.

This is unlikely to change any time soon, so if you need the Baidu user agent to be able to crawl your site you will need to host it elsewhere.

Apologies again for the inconvenience.

```

大致意思就是百度爬虫爬得太猛烈，已经对很多 Github 用户造成了可用性的问题了，而禁用百度爬虫这一举措可能会一直持续下去。

***coding***

####优点：
* 1、解决了github上的博客不能被百度爬虫抓取和收录的问题。
* 2、国内访问速度较快，不需要翻墙。
    
***国内付费主机***

####优点：
* 1、访问速度快，稳定，安全。
* 2、对搜索引擎友好，便于网站对推广和使用。


**总结：**

    几种方案，各有各的好处。如果你有点网页基础，或者有意向想要了解网站搭建和建设的一些知识，那无疑就是选择前面两种方案了。   
    如果自己对网站要求较高又不想浪费多余精力在搭建博客上面，那强力推荐采用第三个方案。   
    总的来说，根据个人的需求，采用适合自己的方案，才是正解。   
    
***持续更新中。。。***


 
