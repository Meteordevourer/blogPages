---
title: '为亲爱的02头像换上圆形并旋转'
date: 2021-04-11 14:45:16
categories:
- hexo
tags:
---
找到主题配置文件
> /themes/next/source/css/_common/components/sidebar/sidebar-author.styl

修改为如下代码即可
{% fold 点我折叠 %}
{% codeblock lang:CSS %}
.site-author-image {
  display: block;
  margin: 0 auto;
  padding: $site-author-image-padding;
  max-width: $site-author-image-width;
  height: $site-author-image-height;
  border: $site-author-image-border-width solid $site-author-image-border-color;
  border-radius: 60%;
  transition: 2.5s all;
}

.site-author-image:hover {
    transform: rotate(360deg);
}


.site-author-name {
  margin: $site-author-name-margin;
  text-align: $site-author-name-align;
  color: $site-author-name-color;
  font-weight: $site-author-name-weight;
}

.site-description {
  margin-top: $site-description-margin-top;
  text-align: $site-description-align;
  font-size: $site-description-font-size;
  color: $site-description-color;
}
{% endcodeblock %}
{% endfold %}

思路参考： {% link HexoNexT主题设置圆形头像并旋转 https://crayonnew.github.io/2018/11/05/Hexo-NexT%E4%B8%BB%E9%A2%98-%E8%AE%BE%E7%BD%AE%E5%9C%86%E5%BD%A2%E5%A4%B4%E5%83%8F%E5%B9%B6%E6%97%8B%E8%BD%AC/ %}
