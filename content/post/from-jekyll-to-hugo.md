+++
author = "h4fan"
title = "使用github actions自动deploy hugo博客"
date = "2019-03-11"
description = "使用github actions自动deploy hugo博客"
tags = [
    "hugo",
    "etc",
]
categories = [
    "etc",
]
series = ["etc"]
aliases = ["github-actions-auto-deploy-hugo-blog"]
+++

使用github actions自动deploy hugo博客。甚至都不用本地安装主题。
<!--more-->

# github actions自动deploy hugo博客

## jekyll博客
之前使用jekyll，因为github对jekyll集成好。可以直接写markdown，主题可以使用remote theme，这样就只有纯的markdown，十分简洁。不需要跟html混在一起。  
用了一段时间jekyll，使用的主题总感觉不满意。换了几个主题，还是感觉不符合。再者有些主题不兼容，还得修改老文章，改动比较大。所以想换个博客。  
搜索发现，现在可以使用github actions来进行自动部署，所以不局限于jekyll。

## hugo blog
发现hugo使用的人不少，决定尝试一下。搜索到一个使用github actions[^1]自动部署的方法。  
使用这个方法，可以自动部署，但是我不想在本地安装主题，有没有办法呢？

## hugo主题
hugo的主题，要么下载放到themes里面，要么用git submodule安装。刚开始直接使用`.gitmodules`文件，发现不成功。后来发现github action里面run可以写多条语句，那这样就可以直接在action里面git下载主题，配置里面添加相应记录即可。
```yml
      - name: Build
        run: |
          #ls -la themes
          git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
          #ls -la themes
          #cat .gitmodules
          hugo -D
          hugo --minify	# 使用hugo构建静态网页
```

## 碰到的问题
1. Q: you need the extended version to build SCSS/SASS  
A: 将extended改为true。[^2]
```yml
      - name: Setup Hugo	# 步骤名自取
        uses: peaceiris/actions-hugo@v2	# hugo官方提供的action，用于在任务环境中获取hugo
        with:
          hugo-version: 'latest'	# 获取最新版本的hugo
          extended: true
```

2. Q: action build时报错“The following paths are ignored by one of your .gitignore files: themes/ananke”  
A: 将hugoBasicExample[^3]中的.gitignore里面的themes注释掉。



[^1]: https://tomial.github.io/posts/hugo%E4%BD%BF%E7%94%A8github-action%E8%87%AA%E5%8A%A8%E9%83%A8%E7%BD%B2%E5%8D%9A%E5%AE%A2%E5%88%B0github-pages
[^2]: https://stackoverflow.com/questions/54057291/how-to-setup-scss-with-hugo
[^3]: https://github.com/gohugoio/hugoBasicExample
