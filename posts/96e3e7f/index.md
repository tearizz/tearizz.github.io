# 博客搭建01-Hugo&amp;FixIt


&lt;!--more--&gt;

2024/6/30 用了[hugo](https://gohugo.io/getting-started/quick-start/)&#43;[FixIt](https://fixit.lruihao.cn/zh-cn/documentation/getting-started/quick-start/)&#43;[github](https://github.com/tearizz/tearizz.github.io/tree/main)搭建了自己的博客网站，记录如下：

## Hugo&#43;FixIt (Linux)

### 资料 &amp; 方法 &amp; 步骤

- [Quick start | Hugo (gohugo.io)](https://gohugo.io/getting-started/quick-start/)
- [Linux | Hugo (gohugo.io)](https://gohugo.io/installation/linux/)


### 流程 &amp; 注意事项

&gt; *安装 Hugo &amp; Git -- 创建Site -- 添加内容 -- 配置Site -- 发布*  

**安装 Hugo :**  

- 安装Hugo时，使用apt安装的版本偏低，建议使用snap

**创建 Site：**

   - 创建hugo项目 ` hugo new site &lt;site&gt;`
     hemes文件夹中下载主题，虽然建议用git submodule 但目前实操时发现在上传public文件时module也会上传，暂时不知如何处理，故现用git clone
   - 配置主题 `echo &#34;theme=&#39;&lt;theme&gt;&#39; &gt;&gt; hugo.toml &#34;`（项目的hugo.toml，而非主题的hugo.toml）

**添加内容：**    

-  `hugo new content content/posts/&lt;file.md&gt;` 在content/posts/ 下新建文件&lt;file.md&gt;
- 默认`draft = true`， 建议不要更改draft值，在启动服务时附加参数`--buildDrafts` / `-D`

**配置 Site：**
   - 在项目根目录中的`hugo.toml` 配置 `baseURL`,`languageCode`,`title`,`theme` 
   - 同时主题配置中若包含URL、语言等信息也需配置,路径为`themes/hugo.toml` 
   - 服务`hugo serve -D` / `hugo --theme=&lt;Theme&gt; --baseURL=&lt;URL&gt; -D`
   - 项目启动后在根目录生成 `public` 文件夹，将public上传到github仓库



## Github

### 流程 &amp; 实现 &amp; 注意事项

&gt; *新建github仓库 -- 上传public文件夹*

**新建github仓库：** 

- 项目名一定要是：&#34;用户名.github.io&#34;
  上传public文件夹：
- 只上传public文件夹内容，即先`cd public/`

**上传public文件夹：**

- 与上传Github代码步骤基本相同
  - `git remote add origin git@github.com:tearizz/tearizz.github.io.git` 
  - `git branch -M main` (文档中是-M，但我用-m才可以)
  - `git push -u origin main`  (push不上加`--force`)

---

> 作者: teariz  
> URL: http://localhost:1313/posts/96e3e7f/  

