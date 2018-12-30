layout: '[layout]'
title: Travis-CI自动化部署github上的hexo博客源码
date: 2018-12-30 20:59:58
categories: deploy
tags:[Travis-CI,Hexo,git]
---
之前一直使用hexo静态博客，每次写完东西都需要手动deploy，而且经常环境还出问题，比较恶心。但是！！！今天偶然发现了Travis-CI这个服务，以前只觉得它是用来给代码build验证跑单元测试晕(((φ(◎ロ◎;)φ)))，并没有实际使用过。我们使用github账号授权Travis-CI后，可以配置Travis-CI监听我们的repository动态，当有push或pull request过来的时候，Travis-CI会自动帮我们按照`.travis.yml`的配置进行build和deploy。下面我们来看看是怎么配置的。

##使用Travis-CI自动发布

#### 第一步：登录github生成access token

进入Github个人主页，找到：Settings -> Developer settings -> Personal access tokens，然后取`Generate new token`，参照下图配置即可。

注意：这里生成的token最好先记下来，不然关闭页面后找不到又得`regenerate token`

![](E:\hexo-blog-source\source\_posts\deploy\Travis-CI自动化部署github上的hexo博客源码\1.png)

#### 第二步：注册并开启Travis-CI项目构建

使用 GitHub账户登录 [Travis-CI官网](https://travis-ci.org/) 进去后能看到已经自动关联了 GitHub 上的仓库。这里我们选择需要启用的项目，即 `blog-source`。然后点击后面的`Settings`进入设置界面。

![](E:\hexo-blog-source\source\_posts\deploy\Travis-CI自动化部署github上的hexo博客源码\2.png)

####第三步：配置Travis-CI自动构建

进入设置界面后可以参考以下配置：

配置主要注意一下两点即可：

- Build pushed branches

当分支收到新的push之后构建

- Environment Variables -> GH_TOKEN

GH_TOKEN，是我们第一步在github中生成的access token，因为要从github上将代码拉到travis-ci机器上进行构建，所以需要该token授权。

![](E:\hexo-blog-source\source\_posts\deploy\Travis-CI自动化部署github上的hexo博客源码\3.png)

#### 第四步：配置hexo的_config.yml

因为我们的博客托管在github pages，所以我们是以git的方式进行deploy的，hexo如何配置使用git方式进行deploy，请自行Google。下面截取了我的`_config.yml`文件中关于git deploy配置的片段。

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
- type: git
  # 下方的GH_TOKEN会被.travis.yml中sed命令替换
  repo: https://GH_TOKEN@github.com/gaoyoubo/blog.mspring.org.git
  branch: master
```



#### 第五步：配置构建脚本`.travis.yml`

在hexo项目的根目录创建`.travis.yml`文件，该文件就是travis的构建脚本，下面是我的脚本配置，我会在脚本中详细注释每一步的作用。

```
# 指定语言为node_js，nodejs版本stable
language: node_js
node_js: stable

# 指定构建的分支
branches:
  only:
    - master

# 指定node_modules缓存
cache:
  directories:
    - node_modules

# 构建之前安装hexo-cli，因为接下来会用到
before_install:
  - npm install -g hexo-cli

# 安装依赖
install:
  - npm install

# 执行脚本，先hexo clean 再 hexo generate，会使用hexo的同学应该不陌生。
script:
  - hexo clean
  - hexo generate

# 上面的脚本执行成功之后执行以下脚本进行deploy
after_success:
  - git init
  - git config --global user.name "GaoYoubo"
  - git config --global user.email "gaoyoubo@foxmail.com"
  # 替换同目录下的_config.yml文件中GH_TOKEN字符串为travis后台配置的变量
  - sed -i "s/GH_TOKEN/${GH_TOKEN}/g" ./_config.yml
  - hexo deploy
```

#### 注意

最后要注意这个repository下的文件的时候使用以下命令即可，不然会出现deploy失败

```
git push --quiet https://GH_TOKEN@github.com/gaoyoubo/blog.mspring.org.git
```



原文：https://www.mspring.org/2018/11/29/HexoClient%E4%BD%BF%E7%94%A8%E5%B8%AE%E5%8A%A9/