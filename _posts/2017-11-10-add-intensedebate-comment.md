---
title: jekyll���IntenseDebate����֧��
categories:
- tech
tags:
- jekyll
---

## ��ữ����ϵͳ

����������ϵͳ��[Disqus](http://disqus.com/), [IntenseDebate](https://intensedebate.com), [Livefyre](http://livefyre.com/)��[����](http://changyan.kuaizhan.com/)��[����](http://www.uyan.cc/)�ȡ����Թ�Disqus���������վ������ɵĶ���������ôҲ���ʲ��ˣ����ҷ����ٶ�Ҳ���������������������[IntenseDebate](https://intensedebate.com)���������濴������̫�ÿ���

## ��װIntenseDebate

+ ע��IntenseDebate�˺�
  [https://intensedebate.com/signup](https://intensedebate.com/signup)

+ ���ղ��贴��վ�㣬���ջ��һ��js�����ƴ��뱣����_includes/intensedebate.html��
```
<script>
      var idcomments_acct = 'xxxxx132e69';
      var idcomments_post_id;
      var idcomments_post_url;
      </script>
      <span id="IDCommentsPostTitle" style="display:none"></span>
<script type='text/javascript' src='https://www.intensedebate.com/js/genericCommentWrapperV2.js'></script>
```

+ �������ļ�_config.yml������������ò����������л����۹��ܡ�

```
     intensedebate_comments: true
```
+ ��_layouts/post.html�ļ�������Ӵ�������ʾIntenseDebate���ۿ�
```html
<div id="posts" class="posts-expand">
  {% if site.intensedebate_comments %}
    {% include intensedebate.html %}
  {% endif %}
</div>
```