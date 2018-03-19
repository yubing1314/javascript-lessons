# 初探

当学习一门新的编程语言的时候，应该边学边做，反复演练以加深理解。因此，你需要一个 JavaScript 解释器。幸运的是，每一个 Web 浏览器都包含一个 JavaScript 解释器。

可以通过在 HTML 文件里写一个 `<script>` 元素来嵌入 JavaScript 代码，当浏览器加载 HTML 文件的时候，它会自动执行这段代码。如果运行的是一小段 JavaScript 代码，则不必每次都这样做。我们可以利用 Chrome 浏览器的「开发者工具」来运行这些小段代码，通常按 `F12` 或者 `Ctrl+Shift+J` 快捷键可以唤醒控制台。

> 现代浏览器可以使用函数 `console.log()` 来向控制台输出消息，通过这种方式可以非常方便地调试代码。

## `<script>` 元素

使用 `<script>` 元素的方式有两种：

- 直接在页面中嵌入 JavaScript 代码。
- 包含外部 JavaScript 文件。
 
使用 `<script>` 元素嵌入 JavaScript 代码时，只需为 `<script>` 指定 `type` 属性。然后，像下面这样把 JavaScript 代码直接放在元素内部即可：

``` html
<script type="text/javascript">
    function sayHello(){
        console.log("Hello!");
    }
</script>
```

> 在 HTML5 规范中，`<script>` 的 `type` 属性默认是 `"text/javascript"`，所以可以省略；但是在 HTML 4.01 和 XHTML 1.0 规范中，`type` 属性是必须的。可以参考 Stack Overflow 上的回答：[http://stackoverflow.com/questions/4243577/which-is-better-script-type-text-javascript-script-or-script-scr](http://stackoverflow.com/questions/4243577/which-is-better-script-type-text-javascript-script-or-script-scr)

如果要通过 `<script>` 元素来包含外部 JavaScript 文件，那么 src 属性就是必需的。这个属性的值是一个指向外部 JavaScript 文件的链接，例如：

``` html
<script type="text/javascript" src="example.js"></script>
```

包含在 `<script>` 元素内部的 JavaScript 代码将被从上至下依次解释，在解释器对 `<script>` 元素内部的所有代码求值完毕之前，页面中的其余内容都不会被浏览器加载或显示。

需要注意的是，带有 `src` 属性的 `<script>` 元素不应该在其 `<script>` 和 `</script>` 元素之间再包含额外的 JavaScript 代码。如果包含了嵌入的代码，则只会下载并执行外部脚本文件，嵌入的代码会被忽略。

``` html
<script type="text/javascript" src="example.js">
    console.log("Hello!");  //永远不会执行
</script>
```

另外，通过 `<script>` 元素的 `src` 属性还可以包含来自外部域的 JavaScript 文件。例如：

``` html
<script type="text/javascript" src="https://code.jquery.com/jquery-3.0.0.min.js"></script>
```

> 在 HTML 中嵌入 JavaScript 代码虽然没有问题，但一般认为最好的做法还是尽可能使用外部文件来包含 JavaScript 代码。

## 元素的位置

按照惯例，所有的 `<script>` 元素都应该放在页面的 `<head>` 元素中，例如：

``` html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script type="text/javascript" src="example1.js"></script>
    <script type="text/javascript" src="example2.js"></script>
</head>
<body>
    <!-- 这里放内容 -->
</body>
</html>
```

可是这种做法意味着必须等到全部 JavaScript 代码都被下载、解析和执行完成之后，才能开始呈现页面的内容（浏览器在遇到 `<body>` 元素时才开始呈现内容）。对于那些需要加载很多 JavaScript 代码的页面来说，会导致页面出现明显的延迟（浏览器窗口一片空白）。为了避免这个问题，最好的做法是把 `<script>` 元素放到 HTML 文档的最后面，`</body>` 元素之前，例如：

``` html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <!-- 这里放内容 -->
    <script type="text/javascript" src="example1.js"></script>
    <script type="text/javascript" src="example2.js"></script>
</body>
</html>
```

## 小结

把 JavaScript 插入到 HTML 页面中要使用 `<script>` 元素。使用这个元素可以把 JavaScript 嵌入到 HTML 页面中，让脚本和标记混合在一起；也可以包含外部的 JavaScript 文件。

在包含外部 JavaScript 文件时，必须将 `src` 属性设置为指向相应文件的 URL。而这个文件既可以是本服务器上的文件，也可以是其他任何域中的文件。

所有 `<script>` 元素都会按照他们在页面中出现的先后顺序依次被解析。

## 关卡

现有一个网页（代码如下），引用了大量的外部 JavaScript 文件，打开该网页会一直显示空白，直至外部 JavaScript 文件全部下载完毕，才能正常显示网页内容「本页面用来测试 `<script>` 加载顺序~」，用户体验不好。请尝试修改页面中 `<script>` 元素的位置，实现以下效果：

- 挑战一：实现打开页面就能看到网页内容「本页面用来测试 `<script>` 加载顺序~」，不必等外部 JavaScript 文件全部下载完毕才显示。 
- 挑战二：实现在外部 JavaScript 文件下载之前「开启页面加载效果」，外部 JavaScript 文件全部下载完毕之后「关闭页面加载效果」。

最终效果可参考：[http://shijiajie.com/other/javascript-lesson/1.2](http://shijiajie.com/other/javascript-lesson/1.2)

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>javascript-lesson-1.2</title>
    <link rel="stylesheet" href="http://qiniu.shijiajie.com/blog/javascript-lesson/1.2/layer.css">

    <!-- 开启页面加载效果 -->
    <script src="http://qiniu.shijiajie.com/blog/javascript-lesson/1.2/layer.js"></script>
    <script>layer.open({ type: 2, shadeClose: false });</script>

    <!-- 关闭页面加载效果 -->
    <script>setTimeout(function(){ layer.closeAll(); },500);</script>

    <!-- 引入 10MB 外部 JavaScript，比较耗时 -->
    <script src="http://qiniu.shijiajie.com/blog/javascript-lesson/1.2/external.js"></script>

</head>
<body>

    本页面用来测试 &lt;script&gt; 加载顺序~

</body>
</html>
```

## 更多

> 关注微信公众号「劼哥舍」回复「答案」，获取关卡详解。  
> 关注 [https://github.com/stone0090/javascript-lessons](https://github.com/stone0090/javascript-lessons)，获取最新动态。

