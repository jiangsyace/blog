---
title: jekyll添加IntenseDebate评论支持
categories:
- tech
tags:
- jekyll
---

## 社会化评论系统

主流的评论系统有[Disqus](http://disqus.com/), [IntenseDebate](https://intensedebate.com), [Livefyre](http://livefyre.com/)，[畅言](http://changyan.kuaizhan.com/)，[友言](http://www.uyan.cc/)等。尝试过Disqus，但是添加站点后，生成的二级域名怎么也访问不了，而且访问速度也很慢，遂放弃。后来改用[IntenseDebate](https://intensedebate.com)，不过界面看起来不太好看。

## 安装IntenseDebate

+ 注册IntenseDebate账号
  [https://intensedebate.com/signup](https://intensedebate.com/signup)

+ 按照步骤创建站点，最终获得一段js，复制代码保存在_includes/intensedebate.html中
```
<script>
      var idcomments_acct = 'xxxxx132e69';
      var idcomments_post_id;
      var idcomments_post_url;
      </script>
      <span id="IDCommentsPostTitle" style="display:none"></span>
<script type='text/javascript' src='https://www.intensedebate.com/js/genericCommentWrapperV2.js'></script>
```

+ 在配置文件_config.yml中添加评论配置参数，方便切换评论功能。

```
     intensedebate_comments: true
```
+ 在_layouts/post.html文件后面添加代码来显示IntenseDebate评论框
```html
<div id="posts" class="posts-expand">
  {% if site.intensedebate_comments %}
    {% include intensedebate.html %}
  {% endif %}
</div>
```