title: "使用Hexo在Github上搭建自己的博客"
date: 2015-03-10 22:30:04
tags:
- Hexo
- Github
categories:
- Hexo
---

以前的博客是使用Jekyll托管在github上，后来用着越来越不方便，比如没有自动生成post，不能一键部署，文件结构和配置也是比较繁琐，更重要的是有时候用markdown写一篇文章，生成的静态文件很乱，应该是有些字符解析的问题。现在下决心换成了hexo

# Nodejs安装

因为hexo是基于nodejs的应用，所以要先安装nodejs才可以。我这里以ubuntu为例，因为我自己一直在使用ubuntu。使用如下命令即可：

```bash
sudo apt-get install nodejs
sudo apt-get install npm
```

# Hexo安装

[Hexo](https://github.com/hexojs/hexo/ "Hexo") 是一个功能强大的静态网站生成系统，快速、简洁、高效。Hexo 支持 GitHub Flavored Markdown 的所有功能，甚至可以整合 Octopress 的大多数插件。可以一键部署到github，还有丰富的插件和主题,还支持热部署哦。执行如下命令进行安装。

```bash
sudo npm install hexo-cli -g
```

# 快速开始

安装Hexo安成后，使用如下命令快速新建一个博客系统，然后运行它

```bash
hexo init blog
cd blog
npm install
hexo server
```

如果npm安装失败，请使用sudo安装。运行成功后，访问http://0.0.0.0:4000/就可以看到博客的样子的，对就是我现在的博客这个样子，o(∩∩)o...哈哈。

现在我们来看看Hexo 生成博客的目录结构

    .
    ├── _config.yml
    ├── db.json
    ├── node_modules
    ├── package.json
    ├── public
    ├── scaffolds
    ├── source
    └── themes
    
其中_config.yml是配置站点的文件，public是hexo生成的静态站点文件夹，scaffolds是模板文件夹，source是存在用户资源的文件夹，themes是主题文件夹。

# 站点配置

找到title，subtitle，author参数配置，分别配置站点的标题、副标题和作者,比如我这里是：

    title: 飞雪无情的博客
    subtitle: 专注于Android、Java、移动互联网、项目管理、软件架构
    description:
    author: 飞雪无情
    language: zh-CN
    
然后配置站点的url和permalink，这两个分别是你的站点的url host地址以及文章的永久连接，我这里是：

    url: http://www.flysnow.org
    root: /
    permalink: :year/:month/:day/:title.html
    permalink_defaults:
    
permalink 我配置的是年月日以及title，后缀是html，便于搜索收录。permalink详情参见： [http://hexo.io/zh-cn/docs/permalinks.html](http://hexo.io/zh-cn/docs/permalinks.html "")

# 新增一篇文章

使用如下命令即可新增一篇md格式的文章

```bash
hexo new 'github-page-with-hexo'
```

然后就会在sources/_posts生成一篇文件名为github-page-with-hexo.md的markdown文件。编辑该文件就可以写博客了。这里有一些Front-matter需要介绍，可以配置文章使用的模板、所属的分类和tag等。

Front-matter 是文件最上方以 --- 分隔的区域，用于指定个别文件的变量，举例来说：

    title: "使用Hexo在Github上搭建自己的博客"
    date: 2015-03-10 22:30:04
    tags:
    - Hexo
    - Github
    categories:
    - Hexo
    ---
    
请注意，目前的categories只能有一个一级分类，如果填写多个，第二个会被解析为二级分类，以及类推。tags可以允许有多个。更多关于Front-matter请参考 [http://hexo.io/zh-cn/docs/front-matter.html](http://hexo.io/zh-cn/docs/front-matter.html "")

# 发布到github page

首先你已经创建好了你的github page对应的git库，没有创建的可以google相关博客。然后新建一个hexo分支，存放你现在hexo的所有文件。然后执行如下命令清理并生成发布的静态站点文件。

```bash
hexo clean
hexo generate
```

然后把生成的public目录下的文件放到你的master分支下即可。git commit后把这两个分支推送到你的github上。git库结构可以参见我的github page库 [https://github.com/rujews/rujews.github.io](https://github.com/rujews/rujews.github.io "https://github.com/rujews/rujews.github.io")

# 最后

然后等个几分钟，访问你的域名就可以看到你的网站了。如[http://www.flysnow.org/](http://www.flysnow.org/ "http://www.flysnow.org/") 。关于更多的Hexo请参考官方文档，以后有时间也会介绍 [http://hexo.io/zh-cn/docs/](http://hexo.io/zh-cn/docs/)