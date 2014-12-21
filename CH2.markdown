#第二章 在HTML中使用JavaScript

##2.1 &lt;script>元素

- async : 异步下载脚本，只对外部脚本有效（必须有src属性）
- charset : 指定代码字符集（多数浏览器会忽略它）
- defer : 延迟到文档完全被解析和显示之后再执行，只对外部脚本有效（必须有src属性）（IE7之前对嵌入脚本也有效）
- language : 已废弃
- src : 包含要执行代码的外部文件
- type : 脚本语言的内容类型（也称为MIME类型）

**注意事项：**

- 包含在&lt;script>中的js代码将被从上到下依次解析。
- 使用&lt;script>嵌入js代码时，代码中不要出现&lt;/script>，要使用转义字符，如：

``` js
<script type="text/javascript>
    function sayScript(){
        alert("<\/script>"); //这里不能直接写</script>
    }
</script>
```
- 使用外部js文件，和解析嵌入式js代码一样，在解析（包括下载）外部文件的时候，页面处理会暂时停止
- 按照惯例，外部js文件应有.js扩展名，如果不加.js，那么要确保服务器返回正确的MIME类型
- 带有src属性的&lt;script>元素不应在&lt;script和&lt;/script>之间包含额外js代码，如果加了，会被忽略
- &lt;script>元素可以引用外部域中的代码，只要在src属性指定外部域的完整url

###2.1.1 标签的位置

- 传统做法，将&lt;script>元素放在&lt;head>元素中。（弊端：呈现页面时可能出现明显延迟）
- 把js引用放在&lt;body>元素中页面内容的后面

###2.1.2 延迟脚本

**注意事项：**

- defer属性相当于告诉浏览器立即下载，但是延迟执行
- 原则上来说，多个添加了defer的script外部文件，也按照上下顺序执行，并且这些添加了defer的脚本会先于DOMContentLoaded事件触发前执行，但不论执行顺序和执行时间在现实中都可能并非如此，所以最好只包含一个延迟脚本。
- defer只适用于有src属性的外部脚本，而忽略嵌入脚本设置的defer。但是IE4-IE7还支持对嵌入脚本的defer
- 有些浏览器直接忽略这个属性，因此把延迟脚本放到页面底部仍然是最佳选择

###2.1.3 异步脚本

**注意事项：**

- async属性相当于告诉浏览器立即下载外部文件，但不保证按照指定它们的先后顺序执行
- async只适用于带有src的外部脚本文件
- 建议异步脚本不要在加载期间修改DOM
- 异步脚本一定会在页面的load事件前执行，但可能会在DOMContentLoaded事件触发之前或之后执行。

###2.1.4 在XHTML中的用法（略）

###2.1.5 不推荐使用的语法（略）

##2.2 嵌入代码与外部文件

- 可维护性
- 可缓存
- 适应未来

##2.3 文档模式

- 文档模式通过使用文档类型（doctype）来切换
- 混杂模式（quirks mode）vs.标准模式（standards mode）
- 标准模式：HTML 4.01 严格型、XHTML 1.0 严格型、HTML5
- 准标准模式：HTML4.01 过度型、HTML 4.01 框架集型、XHTML 1.0 过度型、XHTML 1.0 框架集型

##2.4 &lt;noscript>元素

- 下面两种情况下，&lt;noscript>元素显示：
    - 浏览器不支持脚本
    - 浏览器支持脚本，但脚本被禁用

``` js
<noscript>
    <p>本页面需要浏览器支持（启用）JavaScript。</p>
</noscript>
```