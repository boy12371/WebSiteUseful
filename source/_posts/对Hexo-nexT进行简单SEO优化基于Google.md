---
title: 对Hexo-nexT进行简单SEO优化基于Google收录站点
tags:
  - seo
  - hexo
  - next
categories:
  - seo
copyright: ture
keywords:
  - seo
  - Google收录站点
abbrlink: 5e7d6b37
date: 2018-02-26 21:52:25
---

#### 设置站点地图
1. 安装`sitemap`站点地图自动生成插件
    ```
    npm install hexo-generator-sitemap --save
    ```
2. 在站点配置文件中末尾添加
    ```
    sitemap:
     path: sitemap.xml
    ```
3. 站点配置文件url修改为你的网址域名，`hexo g`之后，在站点根目录的`/public`生成`sitemap.xml`文件  

<!--more-->
4. 在`/source`中新建文件`robots.txt`如下（allow,disallow看需求）：
  ```
  User-agent: *
  Allow: /
  Allow: /about/

  Disallow: /archives/
  Disallow: /categories/
  Disallow: /tags/
  Disallow: /photo/

  Disallow: /vendors/
  Disallow: /js/
  Disallow: /css/
  Disallow: /fonts/
  Disallow: /vendors/
  Disallow: /fancybox/

  Sitemap: https://yourWebSite/sitemap.xml
  ```
`hexo g -d`生成并部署  
说明：
* robots.txt（统一小写）是一种存放于网站根目录下的ASCII编码的文本文件，它的作用是告诉搜索引擎此网站中哪些内容是可以被爬取的，哪些是禁止爬取的。
* Allow字段的值即为允许搜索引擎爬区的内容，可以对应到主题配置文件中的menu目录配置，如果菜单栏还有其他选项都可以按照格式自行添加。

#### 将站点提交至Google
1. [google search console](https://www.google.com/webmasters/),添加站点域名
2. 站点验证选择备用验证,将给出的标签复制到`\themes\hexo-theme-next\layout\_partials\head.swig`中
3. `robots.txt`测试工具并测试
4. 站点地图提交域名中的`sitemap.xml`文件
5. google抓取工具，将需要给谷歌收录的url网页进行`抓取`操作，并请求编入索引

#### 首页title优化（可选但不推荐）
`hexo/themes/next/layout/index.swig`中的
 ```
 {% block title %}
 ...
 {% endblock %}
 ```
 包含的内容进行如下修改：
 ```
{% block title %}
{{ config.title }}
{% if theme.index_with_subtitle and config.subtitle %} - {{config.subtitle }}
{% endif %}
{{ theme.description }} 
{% endblock %}
 ```

#### 修改文章链接（推荐）
HEXO默认的文章链接形式为domain/year/month/day/postname，默认就是一个四级url，并且可能造成url过长，对搜索引擎是十分不友好的，推荐安装hexo-abbrlink
```
npm install hexo-abbrlink --save
```
然后配置`_config.yml`
```
permalink: note/:year/:month_:day/:abbrlink.html
abbrlink:
  alg: crc32  # 算法：crc16(default) and crc32
  rep: hex    # 进制：dec(default) and hex
```
之后部署一下好了
#### 添加`nofollow`标签(推荐)
nofollow标签是由谷歌领头创新的一个反垃圾链接的标签，并被ecosia.org、bing(US)等各大搜索引擎广泛支持，引用nofollow标签的目的是：用于指示搜索引擎不要追踪（即抓取）网页上的带有nofollow属性的任何出站链接，以减少垃圾链接的分散网站权重。
__对`\themes\next\layout_partials\footer.swig`中的如下部分：__
``` 
{{ __('footer.powered', 
'<a class="theme-link" href="http://hexo.io">Hexo</a>') }}
```
修改为
```
{{ __('footer.powered', 
'<a class="theme-link" 
    href="http://hexo.io" rel="external nofollow">
    Hexo</a>') }}
```
以及将这部分
```
<a class="theme-link" href="https://github.com/iissnan/hexo-theme-next">
```
修改为
 ```
<a class="theme-link" 
    href="https://github.com/iissnan/hexo-theme-next"
    rel="external nofollow">
```
 __对`\themes\next\layout_macro\sidebar.swig`中的如下部分：__
```
<a href="{{ link }}" target="_blank">{{ name }}</a>
```
修改为
```
<a href="{{ link }}" target="_blank" rel="external nofollow">{{ name }}</a>
```
以及将下面代码
```
<a href="http://creativecommons.org/licenses/{{ theme.creative_commons }}/4.0"
    class="cc-opacity" 
    target="_blank">
 ```
 修改成
 ```
<a href="http://creativecommons.org/licenses/{{ theme.creative_commons }}/4.0"
    class="cc-opacity" 
    target="_blank" 
    rel="external nofollow">
 ```
 #### 相关阅读（推荐）
 [SEO优化实战](http://imweb.io/topic/5682938b57d7a6c47914fc00)