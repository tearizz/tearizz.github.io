# 博客搭建02-代码块&amp;中英文


&lt;!--more--&gt;

## 代码颜色
**变量位置：**

  FixIt对变量的控制都在 `FixIt/css/_variables.scss`，想对变量进行改变，可以在`FixIt/css/_variables.scss`更改，或在`FixIt/css/_override.scss`重写，控制代码背景与代码颜色的变量是`$code-background-color`，`$code-background-color-color-dark`，`$code-color`,`code-color-dark`。

**代码位置：**

  但是对代码块的应用都在文件`FixIt/assests/css/_partials/_single/_code.css`中，行内代码在`//inline`注释下，代码块在`//indented code` 注释下，经过多次实验，代码块的背景色为黑色是目前看起来最舒服的。

**注意事项：**

  此时产生了一个问题，由于行内代码与代码块中代码均为`$code-color`；且代码块背景色为黑色，导致在代码块中的普通代码看不出来，这边在代码块`pre:not(.....{`中的对代码`code{`进行修改，在第40行处增加了`color:#ffebe9 !important;`，这里最重要的是`!important`，一定要加，因为不加的话可能会因为多属性重名而被覆盖。

## 中英文
将博客主要改为英文，在`&lt;site&gt;/hugo.toml`中添加`defaultContentLanguage=&#34;zh-cn&#34;`，若想对字段进行修改，则在`FuxIt/i18n`中进行修改。

而顶端的“Archives”，“Categories”，“Tags”则在`FixIt/hugo.toml`中的菜单系统 `[[menu.main]] name=&#34;&lt; &gt;&#34;` 中进行更改

---

> 作者:   
> URL: http://localhost:1313/posts/dbd3dea/  

