# Blog 03 - Disappear of Archives Page


<!--more-->


## 问题
在写好文章后，“菜单系统”中的Categories与Tags均能正常显示，唯独archives页显示
不出来，报的是“找不到页面”的问题。


## 定位问题与失败原因

在 `public/` 文件中，可以找到已经生成的archives页 `index.html`，说明页面照常生成，但是没有**配置**出来。所以我将解决问题的重心放在了寻找配置项上面，认为是某些配置项未打开（false/true）的问题。

## 解决问题

最后在github中提问了作者才解决问题[Archives 页面不存在 · hugo-fixit · Discussion #455 (github.com)](https://github.com/orgs/hugo-fixit/discussions/455)，目前导致问题的真正原因是：hugo项目的配置文件`<project>/hugo.toml`会屏蔽掉主题内的配置文件`FixIt/hugo.toml`，即只有项目中的配置文件`hugo.html`才产生作用。

## 反思

在设置项目主题，设置项目中英文时，我都是在项目的配置文件中进行的更改，同时也发现了，“改过项目配置文件后就不用再改主题内的配置文件了”，但是没有往**深入**去思考，二者的关系如何，会导致什么影响。试了一次全部copy，但是因为与已经设置的字段冲突就没再继续尝试了，以后做每一步都应该再仔细点、多思考些。


---

> 作者: tearizz  
> URL: http://localhost:1313/posts/720df59/  

