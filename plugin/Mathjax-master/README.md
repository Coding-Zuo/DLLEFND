# Mathjax
## 数学表达式的转译

配置参考链接：
https://segmentfault.com/a/1190000008317350


##  解决MathJax的CDN过期问题，实现本地化

如果以cdn形式使用MathJax，网速慢的时候可能会出现数学表达式解析失败的问题，今天就花点时间弄了下本地化。项目非常大，因为依赖太多，如果有更好的方式，请指点。

将GitHub上的项目clone到本地
[https://github.com/mathjax/MathJax](https://github.com/mathjax/MathJax)

在本地开启一个服务，在刚刚down下来的项目test文件夹中，打开index.html可以查看此时MathJax是否可用，正常情况下不会出现问题。
### 文件MathJax.js及文件夹config、extensions、fonts/HTML-CSS、jax、localization是项目实现本地化的基础
将包含以上文件的文件夹放到项目根目录即可。
### 引用方式：
 ```bash
<script src="${path}/MathJax/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
```
"${path}"是你的项目路径，如果出现问题，请将MathJax文件夹放在根目录下。
"?"后面是可配置的参数，具体查看[官网相关介绍](http://docs.mathjax.org/en/latest/config-files.html)
### 我们要在网页加载时初始化MathJax,并进行一些配置，以下是我在项目中使用的配置，仅供参考。
```bash
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
        extensions: ["tex2jax.js"],
        jax: ["input/TeX", "output/HTML-CSS"],
        tex2jax: {
            inlineMath:  [ ["$", "$"] ],
            displayMath: [ ["$$","$$"] ],
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre','code','a'],
            ignoreClass:"class1"
        },
        "HTML-CSS": {
            showMathMenu: false
        },
        showProcessingMessages: false,
        messageStyle: "none"
    });
</script>
```
通常我们是通过接口取得数学表达式的相关数据，要在获取到数据后，调用我们之前注册的方法
```bash
  MathJax.Hub.Queue(['Typeset', MathJax.Hub, "topicStat"]);
```
此时查看网页会发现，数学表达式已经被成功转译。

部署到自己的服务上 可能会出现跨域问题，在服务端做跨域处理

具体配置及含义，请参考：
官网：[https://mathjax-chinese-doc.readthedocs.io/en/latest/configuration.html](https://mathjax-chinese-doc.readthedocs.io/en/latest/configuration.html)

民间大神：[https://www.cnblogs.com/tianshifu/p/6388391.html](https://www.cnblogs.com/tianshifu/p/6388391.html)