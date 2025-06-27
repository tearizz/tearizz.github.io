# 换行符与回车键


<!--more-->
# 换行与回车

在了解git commit文件时，发现{content}中的不同字段件使用`0a`分隔，于是查了一下`\u000a`，发现是换行符`\n`（new line）的意思。

[unicode - What escape character is \u000a](https://stackoverflow.com/questions/6116899/what-escape-character-is-u000a)指出，在ASCII中\n=newline=\u000a，但在Unicode中，LF与CR有所不同：
- Line Feed：LF = \\n = \\u000A
- Carriage Return：CR=\\r=\\u000D

深入发现**换行（LF）** 与**回车（CR）** 却有不同

## 来历
源自于计算机出现前的电传打字机（Teletype Model 33），每秒可打印10个字符，但有一个问题是打完一行换行的时候需要用去0.2秒，可以打印两个字符，若此时有两个新字符传进来，则字符会丢失。于是演职人员想的解决办法是在每行后面加两个字符，一个叫做”回车“，表示将打印头定位在左边界；另一个叫做”换行“，将打字机纸向下移一行。
## 演变
在计算机发明后，这两个概念也被迁移到计算机上。但当时存储器很贵，两个字符被认为浪费，有些科学家认为一个就足以，于是产生了分歧。
## 区别与问题
- 在unix系统中，每行结尾只有”换行“，即"\\n"；
- 在Windows系统中，每行结尾是”换行回车“，即”\\n\\r“;
- 在 Mac系统中，每行结尾是”回车“。
但问题时，Unix/Mac系统下的文件在Windows中打开时，所有文字会变成一行；而Windows里的文件在Unix/Mac下打开的花，在每行结尾可能会多出一个^M符号

且在编程中，以Windows C++为例
如果只用回车“\\r”（return），就只是回到本行行首，导致本行以前的输出被覆盖掉
```C++
int main(){
    cout<<"hahaha"<<"\r"<<"xixi";
}

> xixi
```
使用"\\n"（newline）才是回车+换行，把光标先移动到行首，再换到下一样
```c++
int main(){
    cout<<"haha"<<"\n"<<"xixi";
}
> haha
> xixi
```

所以对于回车与换行，在不同的语言、不同的环境中，对其的支持与定义也各不相同

参考资料：
>[回车"（carriage return）和"换行"（line feed）的区别和来历-阿里云开发者社区 (aliyun.com)](https://developer.aliyun.com/article/239409)


---

> 作者: tearizz  
> URL: http://localhost:1313/posts/bcf0326/  

