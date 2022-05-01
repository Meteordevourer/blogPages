---
title: '翻页标签错误'
date: 2021-08-27 21:36
categories:
- Blog
tags:
- Bugs
---
# 翻页标签显示错误
鸽了好久好久，408太难学啦，主要还是记得太多......
首先的首先，先来一张bug截图
<!-- more -->
![jpg](../tagError/tagError.png)
那么，这是怎么造成的呢？
显然，应该正常显示的是一个font Awesome字体图标，现在被显示为了HTML源码
![jpg](../tagError/fontAwesome.png)
## 解决办法:
最简单的办法就是将`<i class="fa fa-angle-right"></i>`这个不能正常显示的字体图标改成一般的字符，我这里就是改成正常的一般左右键字符`>`，`<`。
在 `themes\hexo-theme-next\layout_partials` 下找到hexo-theme-next的翻页组件，就是`pagination.swig`
将
```
{% if page.prev or page.next %}
  <nav class="pagination">
    {{
      paginator({
        prev_text: '<i class="fa fa-angle-left"></i>',
        next_text: '<i class="fa fa-angle-right"></i>',
        mid_size: 1
      })
    }}
  </nav>
{% endif %}
```
改为
```
{% if page.prev or page.next %}
  <nav class="pagination">
    {{
      paginator({
        prev_text: '<',
        next_text: '>',
        mid_size: 1
      })
    }}
  </nav>
{% endif %}
```
即可解决
![jpg](../tagError/tagRecovered.png)
思路参考： {% link 解决Hexo博客模板hexo-theme-next的翻页按钮不正常显示问题 https://www.cnblogs.com/xiejava/p/12456273.html %}
## 拓展
那么，既然可以通过HTML的形式改图标，那么可否将其更换为别的更好看的icon甚至是图片呢？
思路到了，那就动手
### 首先，查询确定HTML标签paginator属性及各value
麻了，百度之后发现天真了，这并不是简单的自有标签，这属于**Django**的组件——分页器
好吧，那么简单的看一下HTML的调用实现吧
可以看到，上述HTML是采用js的渲染标记来输出HTML，现尝试如下操作
```
{% if page.prev or page.next %}
  <nav class="pagination">
    {{
      paginator({
        prev_text: '<img src="pulpit.jpg" alt="Pulpit rock" width="304" height="228">',
        next_text: '<img src="pulpit.jpg" alt="Pulpit rock" width="304" height="228">',
        mid_size: 1
      })
    }}
  </nav>
{% endif %}
```
未完待续

