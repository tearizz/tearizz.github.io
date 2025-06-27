# Blog01 - Infrastructure


<!--more-->

2024/6/30 用了 [hugo](https://gohugo.io/getting-started/quick-start/)+[FixIt](https://fixit.lruihao.cn/zh-cn/documentation/getting-started/quick-start/)+[github](https://github.com/tearizz/tearizz.github.io/tree/main) 搭建了自己的博客网站，os 为 ubuntu24.04 ，记录如下：

2025/6/27 重建了一下，增加一些配置，丰富了主页

## Hugo 
**文档：**
- [Quick start | Hugo (gohugo.io)](https://gohugo.io/getting-started/quick-start/)
- [Linux | Hugo (gohugo.io)](https://gohugo.io/installation/linux/)
- [Fix configuration](https://fixit.lruihao.cn/zh-cn/documentation/getting-started/configuration/)



**安装 Hugo :**  

- 安装Hugo时，使用apt安装的版本偏低，建议使用[snap](https://gohugo.io/installation/linux/#snap)
- [Go编译Hugo源码](https://gohugo.io/installation/linux/#build-from-source)是另一种方式，保证最新版本

**创建项目及主题：**

   - 创建hugo项目 ` hugo new site <path>` ，如果path非空，需要加上flag `--force`
   - 在<project/themes/>中下载主题（目前选择的是FixIt），在频繁更新的条件下建议使用git submodule，目前做法是fork了一份到个人仓库，不知道不fork的效果。
   - 在`<project/hugo.toml>`中配置主题、标题、中英文（在FixIt中配置项更多更负责，不如在project中覆盖）
```toml
        baseURL = 'http://localhost:1313' # 本地测试时候的URL，后续挂载到github上
        theme = 'FixIt'  # ==> 测试时也可以灵活指定主题 `hugo serve --theme="FixIt" -D`
        title = '捶打石头的101次'

        defaultContentLanguage = "zh-cn"
        languageCode = "zh-CN"
        languageName = "简体中文"
```

**添加文章：**    

-  `hugo new content content/posts/<file.md>` 在content/posts/ 下新建文件<file.md>
- 默认`draft = true`， 建议不要更改draft值，在启动服务时附加参数`--buildDrafts` / `-D` ==> `hugo serve -D`


## FixIt

**主题配置：**
1. 在我之前提的问题 -- [Archives页面不存在](https://github.com/orgs/hugo-fixit/discussions/455) 中，FixIt的作者回复到：

  > `<project>/hugo.toml` 的优先级始终大于 `FixIt/hugo.toml`当 project 配置缺省时，主题的配置会当作默认值。但是 Hugo 设定主题的配置有一些限制，默认只有 `params`, `menu`, `outputformats` 和 `mediatypes` 会存在这种合并行为。其他的配置需要用户手动使用 `_merge` 字段指定合并行为

​	因此，用户需要手动“合并项目与主题配置”（作者的说法是从主题中继承配置）：

  ```
  [mediaTypes]
    _merge = "shallow"

  [outputFormats]
    _merge = "shallow"

  [outputs]
    _merge = "shallow"
```

- `_merge` 参数用于控制配置项的合并策略，在使用多层级配置（如模块或主题继承）时非常重要

- 这里的 `shallow` 表示浅合并策略，Hugo 的三种配置策略：

  | 策略      | 行为                                                         | 适用场景               |
  | :-------- | :----------------------------------------------------------- | :--------------------- |
  | `none`    | 完全忽略上级配置                                             | 需要完全覆盖默认配置时 |
  | `shallow` | 仅合并顶级键，嵌套结构会被覆盖                               | 大部分场景（最常用）   |
  | `deep`    | 递归合并所有层级（类似 JavaScript 的 `Object.assign` 深度合并） | 需要保留嵌套结构时     |

- 综上：Hugo 的配置加载顺序为**默认配置 → 主题配置 → 用户配置**，而 _merge 策略决定了策略如何组合；

  - 90% 的场景用 shallow 就满足需求
  - 需要保留嵌套配置时使用 deep
  - 完全自定义即彻底重写配置块时使用 none 

2. 内置的搜索功能[params.search] 配置如下：

   - 在[params.search]中开启搜索功能

     ```
     [params.search]
         enable = true
     ```

   - 在[outputs]的home中添加"json"

     ```toml
     [outputs]
       home = ["html", "rss", "archives", "offline", "search","json"]
     ```




## Github
在 Github 中保留Hugo项目；将网站托管到.github.io中，无需购买域名

**Hugo Repo**
  - 新建`.gitignore`文件，其中主要添加`public`文件
  - 主题子模块可以在github中同步，目前还未同步过，等后续同步会更新同步以及冲突情况
  - 需要子模块单独add-commit-push，虽然有选项可以提交所有子模块，但我还是建议一个一个提交，（1）`git status` 可以检查每个子模块，仔细看很难遗漏 （2）子模块的commit肯定不同

**website Repo**
  - 注意事项：
    - 仓库名称一定要是："用户名.github.io"
    - 只需要把 `<project>/public/*`拷贝到该仓库
  - 上传方法（ github 每次新建仓库都会介绍 ）：
    - 新文件夹
    ```bash
      git init
      git add README.md
      git commit -m "<commit message>"
      git branch -M main
      git remote add origin git@github.com:tearizz/personalWebsite.git
      git push -u origin main 
    ```
    - 已经存在的仓库
    ```
      git remote add origin git@github.com:tearizz/personalWebsite.git
      git branch -M main
      git push -u origin main
    ```


> First Modify in 2026-06-27

---

> 作者: tearizz  
> URL: http://localhost:1313/posts/96e3e7f/  

