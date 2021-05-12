---
title: "Blog Setup"
date: 2021-05-12T22:15:28+08:00
comments: true
categories: [other]
tags: [other]
draft: false
---

> 简要记录本博客的搭建流程和步骤。

## 前提

- hugo
- git
- github

## 具体步骤

1. github创建公开的repo: **blog**

2. hugo设置

```shell
hugo new site blog
cd blog
git init

# theme
git submodule add https://github.com/xianmin/hugo-theme-jane.git themes/jane
cp -r themes/jane/exampleSite/content ./
cp themes/jane/exampleSite/config.toml ./

# test: http://localhost:1313/
hugo server
```

3. git设置

```shell
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/fuzhy/blog.git
git push -u origin main
```

4. github actions的workflow设置

```shell
mkdir -p .github/workflows
touch .github/workflows/gh-pages.yaml
# gh-pages.yaml reference: https://github.com/peaceiris/actions-gh-pages

# commit & push
git add .
git commit -m "add gh-pages workflow"
git push
```

5. pages设置。

- 首先可以看到actions工作流执行成功的记录，并且自动创建了git的gh-pages分支
- 打开Settings设置，选择Pages，Source中选择上面创建的gh-pages分支，Save保存。（folder选择默认的/root）
- 过一段时间后访问https://fuzhy.github.io/blog/即可生效。但是此时好像没有css等文件（网页检查可发现）

6. 修改配置

- 没有样式的原因是config的baseURL配置错误，需要改成https://fuzhy.github.io/blog/
- 同时改一下其他的配置并提交。

7. debug

改完之后github action直接deploy出错，根据错误日志，查询大概需要extended版本的hugo：

```shell
Error: Error building site: TOCSS: failed to transform "sass/jane.scss" (text/x-scss). Check your Hugo installation; you need the extended version to build SCSS/SASS
```

于是修改action的配置，新增extended模式。==>action部署不再报错，搞定。

https://github.com/marketplace/actions/hugo-setup#%EF%B8%8F-use-hugo-extended

8. 初版搞定

功能：提交源文件到main分支，action自动build/deloy到gh-pages分支，网站自动更新

## 迁移

后期需要迁移，或者在其他电脑更新blog时，只需：

```shell
git clone --recursive https://github.com/fuzhy/blog.git
```


