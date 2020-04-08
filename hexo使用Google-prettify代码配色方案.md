---
title: hexo使用Google-prettify代码配色方案
date: 2020-01-19 12:27:57
tags: 
    - hexo
    - 配色
categories:
    - hexo
---
## 前言
hexo 自带的代码配色方案太简单，于是换了种Google-prettify的方案，在这里简单写一下配置方法

## 下载相关文件
1.下载[prettify-small.zip](https://raw.githubusercontent.com/google/code-prettify/master/distrib/prettify-small.zip)解压到主题的source/lib目录下重命名为prettify

2.下载[themes.zip](https://jmblog.github.io/color-themes-for-google-code-prettify/themes.zip)解压到prettify的skins目录下

[prettify-small.zip备用地址](/lib/highlight_themes/prettify-small.zip)

[themes.zip备用地址](/lib/highlight_themes/themes.zip)

<!-- more -->
## 修改相关文件
1.在主题的layout/_third-party目录下新建prettify.swig文件，内容如下：
``` html
<link rel="stylesheet" href="/lib/prettify/skins/{{ theme.custom_highlight_theme }}.css" type="text/css">
<script src="/lib/prettify/prettify.js" type="text/javascript"></script>
<script type="text/javascript">
  $(document).ready(function() {
      $('pre').addClass('prettyprint linenums').attr('style', 'overflow:auto;');
      prettyPrint();
  });
</script>
```
***注意href和src的路径是否正确***

2. 修改主题的layout目录下的_layout.swig文件，在最后面`</body>`前面新加一行
```
{% include '_third-party/prettify.swig' %}
```

## 配置
1. 关闭自带的配色方案
修改blog目录下的_config.yml，找到highlight关键字，将enable设置为false
``` yaml
highlight:
  enable: false
  line_number: true
  auto_detect: true
  tab_replace: ''
  wrap: true
  hljs: false
```
2.启用配色方案
修改主题目录下的_config.yml，找到highlight关键字，在highlight_theme下另起一行改为以下形式
``` yaml
# Code Highlight theme
# Available value:
#    normal | night | night eighties | night blue | night bright
# https://github.com/chriskempson/tomorrow-theme
highlight_theme: normal
custom_highlight_theme: skins下的配色方案名 #后缀不要写
```

## 使用
``` bash
hexo clean
hexo g
hexo s
```
>参考https://www.drunkdream.com/2017/06/03/hexo-set-code-highlight/