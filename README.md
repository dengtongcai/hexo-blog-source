# hexo-blog-source
博客源码，运行步骤

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
```
hexo init
```

## 安装主题和渲染器
```
git clone https://github.com/tufu9441/maupassant-hexo.git themes/maupassant
cnpm install hexo-renderer-pug --save
cnpm install hexo-renderer-sass --save
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

