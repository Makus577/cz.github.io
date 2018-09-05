---
title: travis
date: 2018-09-04 18:01:14
tags:
---
费了一天时间终于把Travis搞明白了，生命在于折腾
## hexo的弊端
   hexo每次更新文章，都特别的繁琐，必须经历以下命令
    ```bash
    hexo clean
    hexo generate
    hexo deploy
    ```
   看起来特别麻烦，如果当我们更改好文章，push到github上之后，能自动执行上面代码，这样会优化很多不必要的时间
 
## Travis CI工具
   针对于以上的弊端，Travis CI工具可以很好的解决。首先Travis CI提供了持续集成服务，其实这里并没有用到它的CI服务，而是通过监听分支提交的动态，在集成成功后执行自定义的部署逻辑。
    
   > 持续集成是一种软件开发实践，即团队开发成员经常集成他们的工作，通过每个成员每天至少集成一次，也就意味着每天可能会发生多次集成。每次集成都通过自动化的构建（包括编译，发布，自动化测试）来验证，从而尽早地发现集成错误。
   
   网上教程很多，首先，你需要两个分支，一是主开发分支（dev），二是生成环境分支（gh-pages）
   首先，我们从0开始搭建hexo
   1. 安装git
   2. 安装node（npm）或者yarn
   3. 全局安装hexo
   4. 新建文件夹并cd到文件夹，git init创建git环境
   5. 新建分支dev，在dev分支上初始化hexo
   6. 生成public文件
   7. 新建分支gh-pages，把public的内容放到该分支下
   8. 在github上面新建你的仓库
   9. 在setting选项中，选择你的github pages分支
   10. 现在就好了，其中可能注意几个地方如下
       > * 文件夹根目录下的`_config.yml`，需要修改repo里面的信息
       > * github Pages会存在二级目录，所以需要设置
   
这里就搭建好hexo了，现在开始搞Travis
首先在dev分支的根目录下新建一个`.travis.yml`
这里有一个样本
```
language: node_js
node_js: stable

addons: # Travis CI建议加的，自动更新api
  apt:
    update: true

cache:
  directories: 
  - node_modules # 缓存 node_modules

install:
- npm install # 初次安装，在CI环境中，执行安装npm依赖

# before_script: 

script:
- hexo g # 执行 hexo generate，把文章编译到public中

after_success: # 执行script成功后，进入到public，把里面的代码提交到博客的gh-pages分支
- cd ./public
- git init
- git config user.name "Yuying Wu"
- git config user.email "wuyuying1128@gmail.com"
- git add .
- git commit -m "Update site"
- git push --force --quiet "https://${GH_TOKEN}@${GH_REF}" master:gh-pages

branches:
  only:
  - dev # CI 只针对分支 dev

env:
  global: # 全局变量，上面的提交到github的命令有用到
  - GH_REF: github.com/YuyingWu/blog.git
  - secure: 
# secure是自动生成的，执行`travis encrypt 'GH_TOKEN=${your_github_personal_access_token}' --add`
```
其中解释一下最后一段注释，github提交需要github access token的生成和加密
这里有另外一个简便方法，但都需要github access token
1. 首先生成github的[Personal Access Tokens](https://github.com/settings/tokens)
记住你的token码
2. 安装Travis CI 
    ```
    gem install travis
    ```
3. 加密
   登陆一下（`travis login`），输入github的用户名密码，然后再执行```
   travis encrypt 'GH_TOKEN=${your_github_personal_access_token}' --add```
  这里执行之后，会发现.travis.yml文件会刷新，并生成env.global.secure的值

登陆Travis CI的官网，登陆github，找到对应的项目仓库，打开它
然后每次当你git push dev分支的内容，Travis就会自动化执行，并刷新到github仓库的gh-pages分支。
神奇！！！！
   