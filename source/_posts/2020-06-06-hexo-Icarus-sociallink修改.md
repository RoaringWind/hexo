---
title: hexo Icarus sociallink修改
date: 2020-06-06 19:26:08
tags:
---
本文目的在于修改hexo的主题icarus,version 3.0.0版本下的sociallink图标，弃用icarus本身提供的[fontawesome](fontawesome.com "fontawesome") 图标。个人原因是该网站图标颜色太单调，想鲜明一点。
操作系统:win10 18363 64位,使用git进行`hexo clean;hexo g;hexo deploy`，具体环境如下。


```git check versions in git
$ git --version
git version 2.27.0.windows.1

$ hexo -v
hexo: 4.2.1
hexo-cli: 3.1.0
os: Windows_NT 10.0.18363 win32 x64
node: 12.18.0
v8: 7.8.279.23-node.37
uv: 1.37.0
zlib: 1.2.11
brotli: 1.0.7
ares: 1.16.0
modules: 72
nghttp2: 1.41.0
napi: 6
llhttp: 2.0.4
http_parser: 2.9.3
openssl: 1.1.1g
cldr: 37.0
icu: 67.1
tz: 2019c
unicode: 13.0
```
首先打开`hexo\themes\icarus`目录下的_config.yml文件，在前几行里可以找到icarus的版本是3.0.0。
用编辑器打开 `hexo\themes\icarus\layout\widget\profile.jsx`文件，定位到14行，把

```jsx hexo\themes\icarus\layout\widget\profile.jsx
{'icon' in link ? <i class={link.icon}></i> : link.name}
```
修改成
```jsx hexo\themes\icarus\layout\widget\profile.jsx
{'icon' in link ? <img src={link.icon} style="height: 20px;width: 20px;"></img> : link.name}
```
此句的目的在于，将图标的来源改成本地图片，而不是官方钦定的fontawesome格式。其中height和width表示图片的宽度和高度（仅测试长宽都是20px）。
修改完之后保存，这样我们就能使用本地的图片了。请注意，不要添加任何别的语句，以双斜杠开头的语句并不是注释！不要用C/Java的想法！我就以为双斜杠是注释，debug了好久。
我选择阿里巴巴矢量图标库 [iconfont](https://www.iconfont.cn/collections/index "iconfont") 作为我的图标来源。进入后搜索weibo,facebook,github的图标，选择自己喜欢的，下载`.svg`格式到本地（仅测试`.svg`格式）。
请注意，使用Chromium核心的Edge浏览器下载图标的时候，请一定等他病毒扫描完毕，下载完全结束后再对图标文件进行移动操作，不然可能会导致后续的bug。
下载完成后，务必把图标移动到`hexo\themes\icarus\source`目录下的任意位置，因为在`_config.yml`里引用的文件都是以source为基目录开始的。可以新建文件夹放图标，也可以放到img文件夹里。
我本人把所有图标放置在`hexo\themes\icarus\source\icons`目录下
再次打开`hexo\themes\icarus`目录下的`_config.yml`文件
首先使用编辑器的查找功能，查找 social ，定位到social_links关键字。此时我们可以把icon: 后面的语句写成自己的刚刚的图片位置了。我的图片放置在`hexo\themes\icarus\source\icons`下，那么我就需要把原来的`icon`行写成`icon: \icons\github.svg` 此处也要注意，icon的冒号后面需要加一个空格，不然会导致错误。
保存好之后，就可以在hexo目录下使用`hexo clean;hexo g;hexo s`三连啦，浏览器打开`localhost:4000`看看效果吧~