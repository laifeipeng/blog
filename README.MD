# 基于VuePress构建的静态博客

## 简介

基于 [VuePress](https://v1.vuepress.vuejs.org/zh/guide/) 构建静态博客站。内置 `deploy.sh` 和 [GitHub Actions](https://github.com/features/actions) 两种自动部署脚本，一键发布到 GitHub Pages 或 国内访问速度更快的Coding Pages。

* **简洁至上**
  * 继承了[VuePress](https://v1.vuepress.vuejs.org/zh/guide/) 简洁至上的特性，以 Markdown 为中心的项目结构，让你专注于写作
* **高自动化**
  * 项目结合了各种自动化技术，[自动生成侧边栏](https://github.com/xugaoyi/blog/issues/113)，[CI持续集成部署](https://xugaoyi.com/pages/6b9d359ec5aa5019/)，[自动百度链接推送](#百度自动推送和统计)，结合GitHub Actions开发的自动化[每天定时推送](https://xugaoyi.com/pages/f44d2f9ad04ab8d3/)
* **SEO友好**
  * 让你的博客给更多人看到



## 使用

#### 1、克隆项目后进入

```bash
git clone https://github.com/xugaoyi/blog
cd blog
```

#### 2、安装依赖并运行开发服务

```bash
npm install # or yarn install
npm run dev # or yarn dev
```

> 使用yarn的可以把以下`npm run`命令替换为`yarn`

#### 3、编辑

编辑或新增 \*.md文件，以及修改 `docs/.vuepress/config.js` 中的 nav 导航栏等信息（具体可查看 [VuePress](https://v1.vuepress.vuejs.org/zh/guide/) 文档），需重新运行·`npm run dev`并刷新浏览器查看更新。

#### 4、部署

本项目包含两种部署方式：

##### 1）、使用deploy.sh脚本部署

第一步，修改`deploy.sh`脚本内的仓库地址为你的仓库，如有自定义域名则一并修改，没有则注释

```bash
# 如果是发布到自定义域名
echo 'xugaoyi.com' > CNAME

# 如果发布到 https://<USERNAME>.github.io
# git push -f git@github.com:<USERNAME>/<USERNAME>.github.io.git master

# 如果发布到 https://<USERNAME>.github.io/<REPO>
# git push -f git@github.com:<USERNAME>/<REPO>.git master:gh-pages
```

第二步，一键部署命令

```bash
npm run deploy # 运行部署脚本deploy.sh,并发布到仓库的gh-pages分支
```
> windows系统下使用bash命令窗

第三步，设置GitHub Pages为gh-pages分支。



##### 2）、使用GitHub Action自动持续集成

第一步，按照[官方文档](https://help.github.com/en/articles/creating-a-personal-access-token-for-the-command-line)，生成一个github token (令牌)。

第二步，将这个密钥储存到当前仓库的`Settings/Secrets`里面。
> `Settings/Secrets`是储存秘密的环境变量的地方。环境变量的名字可以随便起，这里用的是`ACCESS_TOKEN`。如果你不用这个名字，`.github/workflows/ci.yml`脚本里的变量名也要跟着改。

第三步，push提交代码到GitHub仓库master分支。

第四步，设置GitHub Pages为gh-pages分支。




> 注意：如果您有自定义域名，需要在`.github/workflows/ci.yml`中修改`xugaoyi.com`为你的域名。
>
> 如没有自定义域名，需要在`.github/workflows/ci.yml`中去掉`cd docs/.vuepress/dist && echo 'xugaoyi.com' > CNAME && cd -`命令
>
> 参考 [GitHub Action实现自动部署静态博客](http://xugaoyi.com/pages/6b9d359ec5aa5019/)



#### 4.1、部署升级：同时部署到github和[coding](https://dev.tencent.com/)

为了让博客站能够让百度收录，因此我把博客部署到了coding。相关[文章](https://xugaoyi.com/pages/41f87d890d0a02af/)。

在原有部署方式上做了升级，主要修改代码文件有下面这两个：

`deploy.sh`文件看 [这里](https://github.com/xugaoyi/blog/blob/master/deploy.sh)

> 看不懂代码shell代码？ 参考：[shell教程](https://ipcmen.com/)




`ci.yml`文件看 [这里](https://github.com/xugaoyi/blog/blob/master/.github/workflows/ci.yml)

> 看不懂yaml代码？ 参考：[yaml教程](https://xugaoyi.com/pages/4e8444e2d534d14f/)



使用前先将[github token (令牌) ](https://help.github.com/en/articles/creating-a-personal-access-token-for-the-command-line)和[coding token (令牌)](https://dev.tencent.com/help/doc/account/access-token) 同时设置到github仓库的`Settings/Secrets`位置。

![token设置](https://raw.githubusercontent.com/xugaoyi/image_store/master/blog/token.jpg)

##### 部署方式有两种：

##### 1) 、使用如下命令一键部署到github和coding

```sh
npm run deploy
```

##### 2）、使用GitHub Action自动部署到github和coding

只需要push提交代码到github仓库master分支即可。

>  参考 [《GitHub Action实现自动部署静态博客》](http://xugaoyi.com/pages/6b9d359ec5aa5019/)





## 自定义域名以及解析 (可选)

#### 1、申请域名

申请域名当然去哪都行。 国内的[阿里云](https://link.zhihu.com/?target=https%3A//wanwang.aliyun.com/domain/)，[腾讯云](https://cloud.tencent.com/)，国外的[freenom](https://link.zhihu.com/?target=http%3A//www.freenom.com/)，namecheap什么的 。 操作大同小异，都是

* 注册账号，登录

* 搜索并找到可用的域名，买下来。比如我在腾讯云买的一个域名：`xugaoyi.com`

#### 2、DNS解析

想要在浏览器里面输入`xugaoyi.com`就能跳到博客页面。我们首先需要做的就是通过DNS解析将`xugaoyi.com` 与某已有域名绑定起来（CNAME），或者是固定的ip地址绑定起来(A)。

这里以腾讯云操作为例，步骤如下：

* 域名列表中点击`解析`

![解析](https://raw.githubusercontent.com/xugaoyi/image_store/master/blog/20191228212829.png)

* 进入解析页面后，点击`添加记录`。填入，记录类型：`CNAME`，主机记录：`www`或`@`，解析线路：默认，记录值：你的github pages地址`<userName>.github.io`，TTL：默认的10分钟，确定。

![解析2](https://raw.githubusercontent.com/xugaoyi/image_store/master/blog/20191228213125.png)

#### 3、使github pages与域名关联

deploy.sh添加如下脚本

```bash
echo 'xugaoyi.com' > CNAME # // 域名替换成你的
```

在自动部署配置文件ci.yml的jobs.build.steps.<Build and Deploy\>.env.BUILD_SCRIPT中添加脚本

```sh
"cd docs/.vuepress/dist && echo 'xugaoyi.com' > CNAME" && cd -
# 域名替换成你的
```




## 其他


#### 百度自动推送和统计

加入了自动推送和统计插件

```js
'vuepress-plugin-baidu-autopush', // 百度自动推送，作用：加快将页面推送给百度搜索，https://ziyuan.baidu.com/

[
    'vuepress-plugin-baidu-tongji', // 百度统计，https://tongji.baidu.com
    {
        hm: '8a5**************47d607481723b7815d'
    }
]
```

> 如果你也需要百度统计功能，请把hm参数改为你自己的，请到<https://tongji.baidu.com>获取你自己的hm参数