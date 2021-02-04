---
title: Hello World
abbrlink: 4a17b156
keywords:
  - web
  - java
categories:
  - 编程语言
  - 计算机
tags:
  - 标签
  - 算法
---

Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

```bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

```bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

```bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

```bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)

/_ primary _/

<div
  class="note icon custom iconfont icon-QQ2"
  style="background: #f5f0fa;border-left-color: #6f42c1;"
>
  <p>primary</p>
</div>

{% blockquote [author[, source]] [link] [source_link_title] %}
content
{% endblockquote %}

<div class="gallery-group-main">
  {% galleryGroup '壁纸' '收藏的一些壁纸' '/Gallery/wallpaper'
  https://i.loli.net/2019/11/10/T7Mu8Aod3egmC4Q.png %} {% galleryGroup '漫威'
  '关于漫威的图片' '/Gallery/marvel'
  https://i.loli.net/2019/12/25/8t97aVlp4hgyBGu.jpg %} {% galleryGroup 'OH MY
  GIRL' '关于OH MY GIRL的图片' '/Gallery/ohmygirl'
  https://i.loli.net/2019/12/25/hOqbQ3BIwa6KWpo.jpg %}
</div>

{% note info modern %}
info 提示块标签
{% endnote %}

<div class="checkbox yellow checked">
  <input type="checkbox" checked />
  <p>黄色 + 默认选中</p>
</div>
