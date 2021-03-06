# hexo-blog-source
[![Build Status](https://travis-ci.org/dengtongcai/hexo-blog-source.svg?branch=master)](https://travis-ci.org/dengtongcai/hexo-blog-source)

博客源码，首次运行步骤

## 安装Git，Nodejs

[Git下载地址]: http://git-scm.com/	"Git下载"
[Nodejs]: http://nodejs.org/	"Nodejs下载"

## npm配置淘宝镜像
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
配置好使用`cnpm`

## 安装Hexo
进入hexo-blog-source目录
```
cnpm install -g hexo-cli
cnpm install hexo --save
```

## 初始化Hexo
找个空目录执行初始化，然后把node_modules复制到hexo-blog-source下
```
hexo init
```

## 安装主题和渲染器
```
git clone https://github.com/dengtongcai/maupassant-hexo.git themes/maupassant
cnpm install hexo-renderer-pug --save
cnpm install hexo-renderer-sass --save
```

## 本地图床插件

《_config.yml 》里的`post_asset_folder`这个选项设置为`true`

```
npm install hexo-asset-image --save
```

## 分享插件

```
cnpm i -S hexo-helper-qrcode
```

## 编译生成静态文件
```
hexo g
```

## 本地部署
```
hexo s
```
本地访问 http://localhost:4000
即可查看首页内容

## 远程部署到github（首次部署需要安装hexo-deployer-git）
```
cnpm install --save hexo-deployer-git
hexo deploy
```

## 自动部署

```
git push --quiet https://GH_TOKEN@github.com/dengtongcai/hexo-blog-source.git
```



