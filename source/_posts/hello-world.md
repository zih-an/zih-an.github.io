---
title: 'Hexo: Hello World'
tags: [Hexo]
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post in /source/_posts"
$ hexo new draft "new post draft in /source/_drafts"
$ hexo new page xxx
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)

### 全局吸底音乐播放
主要问题：aplayer.pug引用的meting.js文件没有no-destory，将以下代码复制到本地，改为引用本地的meting.js
(Meting.js地址)[https://github.com/metowolf/MetingJS/blob/v1.2/src/Meting.js]