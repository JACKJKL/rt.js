# rt.js

一款基于 JavaScript 语法的模板引擎.


## 快速上手.

1. 引入 rt.js 文件.
2. 使用 rt.template( template, data ); 生成 HTML 字符串.

```
var template = '<%= it %>'
var html = rt.template( template, 'I\' am rt.js!' );

// 或
var func = rt.template( template );
var html = func( 'I\' am rt.js!' );
```


## 特性

* 支持注释
* 重定义 tag - [[, ]]
* 在模板中书写 JavaScript 代码.
* 内置转义文本字符, 防止 xss - 同时支持扩展转义字符.
* 支持子模板 - 支持自定义策略.
* 在模板中使用扩展方法. `rt.helper( 'name', method );`
* 支持调试 - chrome develop tool 直接支持.


## 语法说明

* 默认 tag: `<% %>`
* 切换 tag: `<%@ {{ }} @%>`
* 注释: `<%# %>`
* 代码: `<% script_code %>`
* 转义输出: `<%= it.property %>`
* 不转义输出: `<%& it.property %>`
* 使用 unicode 编码输出: `<%:= it.property %>
* 子模板: `<%> tag %>` 默认支持浏览器端 dom#id. 可使用 rt.helper 注册 'include' 方法重写它.


## 实例

### 使用 rt.template 生成模板函数.
```
// html:
<script type="text" id="tmpl">
  <ul>
  <% for( var i = 0, l = it.length; i < l; ++i ) { %>
    <li><%= it[i] %></li>
  <%}%>
  <ul>
</script>

// JavaScript:
var template = document.getElementById( 'tmpl' ).innerHTML;
var render = rt.template( template, 'tmpl'/* 可选 id, 建议添加 */ );
var html = render( [ 'github', 'yahoo', 'google'] );
```


### 使用 rt.template 生成 html 字符串.

```
var template = '<%= it %>'
var html = rt.template( template, 'template string data' );
```


### 使用子模板.
```
<script type="text" id="tmpl-subtmpl">
  <% $.each( it, function( index, item ) { %>
    <%> tmpl-sub %>
  <% }); %>
</script>

<!-- 
  rt.js 中内置 helper 变量. 
  可使用 rt.helper 注册的方法.
  helper.name( value );
-->
<script type="text" id="tmpl-sub">
<li><%= index %>:  <%= helper.prefix(item) %></li>
</script>

<script>
  // 注册 prefix 方法.
  rt.helper( 'prefix', function( val ) {
    return '@' + val;
  });
  // 渲染.
  var template = $( '#tmpl-subtmpl' ).html();
  var html = rt.template( template, { 's': 'sogou.com' } ); 
</script>
```

[更多(more)](http://zhanhongtao.github.io/blog/rt)


## 补充说明

* 连续的 JavaScript 脚本放在一个 `<%` 脚本 `%>` 中间. 
```
  <% for ( var i = 0, l = it.length; i < l; ++i ) {
    var item = it[i]; %>
    // .....
  <% } %>
```

* 在 & 或者 = 中, 使用 JavaScript 表达式(不加结束分号).
```
  <%= it.sex %>
  <%& it.style %>
```

* 推荐使用模板中注释语法 - HTML 注释也可使用, 但会被模板引擎渲染.
```
  <%# 推荐写法 %>
  <!-- 不推荐写法 -->
```

## 其它
* compile 参考 Mustache 和 underscore.js 的 template 函数. 
* 扫描字符代码来自 Mustache.
* Mustach.js 的 compile -> [Esprima](http://esprima.org/)
