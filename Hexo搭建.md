---
title: Hexo搭建
date: 2017-06-10 16:04:59
tags: [Hexo,git]
categories: Hexo
---
## 开始安装

```bash
$ npm install -g hexo 
```

## 出错,使用淘宝镜像 

```bash
$ npm config set registry https://registry.npm.taobao.org
```

## 出现权限不允许,修改该文件的权限:/usr/local/lib/node_modules,找到文件夹右击,权限改为:读与写
 
## 创建一个文件夹名为:Blog,cd到Blog文件夹中

```bash
$ cd /Users/lbymac/Blog
```

## 初始化hex

```bash
$ hexo init
```

## 文件生成

```bash
$ hexo generate
```

## 本地服务测试

```bash
$ hexo server
```
 
## 在_config.yml文件中添加以下信息

```
type: git 
repository: https://github.com/CherishJoyBy/CherishJoyBy.github.io.git
branch: master
```

## git上传插件安装

```bash
$ npm install hexo-deployer-git —save
```

## 清空

```bash
$ hexo clean
```

## 生成

```bash
$ hexo generate
```

## 部署上传

```bash
$ hexo deploy
```

