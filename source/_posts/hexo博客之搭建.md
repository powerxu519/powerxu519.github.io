---
title: hexo博客之搭建
date: 2020-04-29 23:15:14
tags: hexo
---
# hexo博客搭建-无坑版

>> **本文基于ubantu16.04下操作**

## 准备阶段
### git安装
<!--more-->
* 通过包管理器安装,这里附上[git官网](https://git-scm.com/)</br>
通过如下命令安装</br>
` sudo apt-get install git `</br>
**注：通过这种⽅式安装的 Git 可能不是较新版的 Git，不过⼀般来说是够⽤的**

* 查看git是否安装成功</br>
`git --version`</br>
![查看git版本](./查看git版本.jpg)

### nodejs安装
1. 进入[node官网](https://nodejs.org/)进行下载，默认保存在Downloads文件夹下。</br>
2. 在家目录下新建node文件夹并进入</br>
```
cd ~
mkdir node
cd node
```
3. 将下载的node-v12.18.2-linux-x64.tar.xz安装包解压到~/node 中</br>
`tar -xJvf ~/Downloads/node-v12.18.2-linux-x64.tar.xz`</br>
**解压完成后，在~/node 目录下会出现一个node-v12.18.2-linux-x64的目录**
4. 配置node系统环境变量</br>
编辑~/.bashrc 文件，在文件末尾追加如下信息</br>
`export PATH=/home/python/node/node-v12.18.2-linux-x64/bin:$PATH`</br>
![node](./node环境配置.jpg)
5. 刷新环境变量，使其生效即可</br>
`source ~/.bashrc`</br>
6. 查看安装版本
```
node -v
npm -v
```
![node](./node版本查看.jpg)

**至此准备工作就完成了，接下来就是hexo博客的安装啦！**</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![nice](./nice.jpg)

## hexo博客的安装
1. 使用如下命令安装</br>
```
npm install -g hexo-cli
hexo -v #查看hexo版本
```
2. 新建一个blog文件夹（以后hexo博客的目录），并进入
```
mkdir blog
cd blog
```
3. 初始化博客，启动本地博客，可在浏览器中查看
```
sudo hexo init  #初始化博客
hexo s  #启动本地博客
```
4. 启动之后就可以在本地的4000端口查看了[localhost](http://localhost:4000/)
5. hexo的基本操作可以去官方文档查询（(hexo)[https://hexo.io/]）,下面创建一片新博文，并生成查看
```
hexo n 第一篇博文   # 创建一篇博文
hexo clean    # 清理hexo缓存
hexo g     # hexo生成
hexo s     # 本地查看
```
**此时就可以看到刚才新建的博文了**
6. 部署到GitHub
**现在博客只能本地查看，可以利用GitHub部署到远端，外网就可以查看了**</br>
* 如果没有GitHub账号的话，先注册一哈（[github](https://github.com/)）
* 在GitHub上创建一个新的仓库，名为 yourname.github.io (**必须为这个名字**)
* 下载Git部署插件
`npm install --save hexo-deployer-git`
* 在blog文件夹下找到_config.yml配置文件，在文件的最后修改成如下：</br>
```
deploy:
type: git
repo: https://github.com/YourGithubName/YourGithubName.github.io.git
branch: master
```
**其中repo为GitHub新建仓库的链接，并且：后有一个空格**
* 将本地的hexo博客部署到远端
`hexo d`</br>
* https://YourGithubName.github.io/ #访问这个地址可以查看博客*

如此操作成功之后，博客就搭建完成了，至于美化博客和文章写作就各显神通啦。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![加油](./加油.jpg)</br>
（配置时如有问题可以联系博主，也可以在下方留言。）