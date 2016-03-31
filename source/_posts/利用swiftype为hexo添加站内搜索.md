title: 利用swiftype为hexo添加站内搜索
categories: Java
tags:
  - Hexo
  - Blog
date: 2016-01-28 09:57:45
---

使用swiftype来实现hexo等静态博客的站内搜索，参见：
[静态博客如何实现站内搜索](http://blog.moyizhou.cn/web/search-engine-for-static-pages/#swiftype)

演示：[swiftype搜索演示](http://www.jerryfu.net/search/index.html#stq=sokoban&stp=1)

顺利的搜索出了结果。

实现步骤请参考：
[利用swiftype为hexo添加站内搜索](http://www.jerryfu.net/post/search-engine-for-hexo-with-swiftype.html)

主要参考下面的链接，这个是升级版的教程：
[利用swiftype为hexo添加站内搜索v2.0](http://www.jerryfu.net/post/search-engine-for-hexo-with-swiftype-v2.html)

# 我的配置
## hexo的Pacman主题配置
首先打开`pacman`主题下的`_config.yml`文件在末尾添加如下代码：
```markdown
    swift_search:
    enable: true
```
我这是添了一个`swift_search`的功能，你们也可以直接改掉自带的`google search`。

　　然后到`hexo`的`source`目录（注意不是pacman主题的source目录）下建立一个`search`文件夹，再在其下建立一个index.md，index.md中写入如下代码：
　　
　　然后再切换的到`pacman\layout_partial`目录下，最后需要做的收尾工作全部都在这个目录下。
　　先打开header.ejs，在
```JavaScript
　　<% if	(theme.google_cse&&theme.google_cse.enable){ %>
					<form class="search" action="<%- config.root %>search/index.html" method="get" accept-charset="utf-8">
						<label>Search</label>
						<input type="text" id="search" autocomplete="off" name="q" maxlength="20" placeholder="<%= __('search') %>" />
					</form>
```

和

```JavaScript
    <% } else { %>  //注意这个else前面有一个}是我后加的，为了和后加的代码完成闭包
					<form class="search" action="//baidu.com/search" method="get" accept-charset="utf-8">
						<label>Search</label>
						<input type="text" id="search" name="q" autocomplete="off" maxlength="20" placeholder="<%= __('search') %>" />
						<input type="hidden" name="q" value="site:<%- config.url.replace(/^https?:\/\//, '') %>">
					</form>
					<% } %>
```

之间添加如下一段代码：

```JavaScript
<% }else if	(theme.swift_search&&theme.swift_search.enable){ %>
						<form class="search" action="<%- config.root %>search/index.html" method="get" accept-charset="utf-8">
						<label>Search</label>
						<input type="text" id="search" class="st-default-search-input" maxlength="20" placeholder="Search" />
						</form>
```

注：这里和1.0版本不同的地方在于input里添加了一个class=”st-default-search-input” 的标签，这个和step2中默认的那个input class是一致的，你如果前面步骤有改过input class这里也要做相应的改动。

　　Header.ejs的处理完成
　　具体Header.ejs配置如下：

```JavaScript
<%
function root_for(path){
	path = path || '/'
	var root = config.root;
	if (path.substring(0, root.length) !== root){
    if (path.substring(0, 1) === '/'){
      path = root.substring(0, root.length - 1) + path;
    } else {
      path = root + path;
    }
  }
  return path;
}
%>
<div>
		<% if	(theme.imglogo.enable&&theme.imglogo.src){ %>
			<div id="imglogo">
				<a href="<%- config.root %>"><img src="<%- config.root %><%= theme.imglogo.src %>" alt="<%= config.title %>" title="<%= config.title %>"/></a>
			</div>
			<% } %>
			<div id="textlogo">
				<h1 class="site-name"><a href="<%- config.root %>" title="<%= config.title %>"><%= config.title %></a></h1>
				<h2 class="blog-motto"><% if (config.subtitle){ %><%= config.subtitle %><% } %></h2>
			</div>
			<div class="navbar"><a class="navbutton navmobile" href="#" title="<%= __('menu') %>">
			</a></div>
			<nav class="animated">
				<ul>
					<ul>
					 <% for (var i in theme.menu){ %>
						<li><a href="<%- root_for(theme.menu[i]) %>"><%= i %></a></li>
					<% } %>
					<li>
 					<% if	(theme.google_cse.enable){ %>
					<form class="search" action="<%- config.root %>search/index.html" method="get" accept-charset="utf-8">
						<label>Search</label>
						<input type="search" id="search" autocomplete="off" name="q" maxlength="20" placeholder="<%= __('search') %>" />
					</form>
					<% }else if	(theme.swift_search.enable){ %>
						<form class="search" action="<%- config.root %>search/index.html" method="get" accept-charset="utf-8">
						<label>Search</label>
						<input type="text" id="search" class="st-default-search-input" maxlength="20" placeholder="Search" />
						</form>
					<% } else if(theme.baidu_search.enable){ %>
						<form class="search" action="<%- theme.baidu_search.site %>" target="_blank">
							<label>Search</label>
						<input name="s" type="hidden" value= <%= theme.baidu_search.id %> ><input type="text" name="q" size="30" placeholder="<%= __('search') %>"><br>
						</form>
					<% } else if(theme.tinysou_search.enable){ %>
						<form class="search">
							<label>Search</label>
						<input type="text" id="ts-search-input" name="q" size="30" placeholder="<%= __('search') %>"><br>
						</form>
					<% } else { %>
					<form class="search" action="//google.com/search" method="get" accept-charset="utf-8">
						<label>Search</label>
						<input type="search" id="search" name="q" autocomplete="off" maxlength="20" placeholder="<%= __('search') %>" />
						<input type="hidden" name="q" value="site:<%- config.url.replace(/^https?:\/\//, '') %>">
					</form>
					<% } %>
					</li>
				</ul>
			</nav>
</div>

```

接下来处理search.ejs。

　　将原来的search.ejs中的代码清空，替换为如下的代码：

　　其实主要就是为了控制结果的显示样式。

```javascript
<% if(theme.swift_search.enable) { %>
<div  id="container" class="page">
  <div id="st-results-container" class="st-search-container" style="width:80%">正在加载搜索结果，请稍等。</div>
  <style>.st-result-text {
  background: #fafafa;
  display: block;
  border-left: 0.5em solid #ccc;
  -webkit-transition: border-left 0.45s;
  -moz-transition: border-left 0.45s;
  -o-transition: border-left 0.45s;
  -ms-transition: border-left 0.45s;
  transition: border-left 0.45s;
  padding: 0.5em;
}
@media only screen and (min-width: 768px) {
  .st-result-text {
    padding: 1em;
  }
}
.st-result-text:hover {
  border-left: 0.5em solid #ea6753;
}
.st-result-text h3 a{
  color: #2ca6cb;
  line-height: 1.5;
  font-size: 22px;
}
.st-snippet em {
  font-weight: bold;
  color: #ea6753;
}</style>

<script type="text/javascript">
  (function(w,d,t,u,n,s,e){w['SwiftypeObject']=n;w[n]=w[n]||function(){
  (w[n].q=w[n].q||[]).push(arguments);};s=d.createElement(t);
  e=d.getElementsByTagName(t)[0];s.async=1;s.src=u;e.parentNode.insertBefore(s,e);
  })(window,document,'script','//s.swiftypecdn.com/install/v2/st.js','_st');

  _st('install','NqEsrzxhg1oBfYygK5q5','2.0.0');
</script>

<% } %>
```

注：这里和1.0版本不同的地方在于显示内容的div里添加了一个class=”st-search-container”的标签，这个和step2中默认的那个container标签是一致的，你如果前面步骤有改过这里也要做相应的改动。

　　最后打开footer.ejs（其实header也行，随便你），在最后一个标签之前添加一开始拷贝的那段js代码，我的是：

```JavaScript
<script type="text/javascript">
  (function(w,d,t,u,n,s,e){w['SwiftypeObject']=n;w[n]=w[n]||function(){
  (w[n].q=w[n].q||[]).push(arguments);};s=d.createElement(t);
  e=d.getElementsByTagName(t)[0];s.async=1;s.src=u;e.parentNode.insertBefore(s,e);
  })(window,document,'script','//s.swiftypecdn.com/install/v2/st.js','_st');

  _st('install','NqEsrzxhg1oBfYygK5q5','2.0.0');
</script>
```

  具体footer.ejs配置如下：

```JavaScript
<div id="footer" >
	<% if(theme.author_img){ %>
	<div class="line">
		<span></span>
		<div class="author"></div>
	</div>
	<% } %>
	<% if(theme.author.intro_line1 || theme.author.intro_line2){ %>
	<section class="info">
		<p> <%= theme.author.intro_line1 %> <br/>
			<%= theme.author.intro_line2 %></p>
	</section>
	 <% } %>
	<div class="social-font" class="clearfix">
		<% if(theme.author.weibo){ %>
		<a href="http://weibo.com/<%= theme.author.weibo %>" target="_blank" class="icon-weibo" title="微博"></a>
		<% } %>
		<% if(theme.author.github){ %>
		<a href="https://github.com/<%=theme.author.github %>" target="_blank" class="icon-github" title="github"></a>
		<% } %>
		<% if(theme.author.stackoverflow){ %>
		<a href="http://stackoverflow.com/users/<%=theme.author.stackoverflow %>" target="_blank" class="icon-stack-overflow" title="stackoverflow"></a>
		<% } %>
		<% if(theme.author.twitter){ %>
		<a href="https://twitter.com/<%=theme.author.twitter %>" target="_blank" class="icon-twitter" title="twitter"></a>
		<% } %>
		<% if(theme.author.facebook){ %>
		<a href="https://www.facebook.com/<%=theme.author.facebook %>" target="_blank" class="icon-facebook" title="facebook"></a>
		<% } %>
		<% if(theme.author.linkedin){ %>
		<a href="https://www.linkedin.com/in/<%=theme.author.linkedin %>" target="_blank" class="icon-linkedin" title="linkedin"></a>
		<% } %>
		<% if(theme.author.douban){ %>
		<a href="https://www.douban.com/people/<%=theme.author.douban %>" target="_blank" class="icon-douban" title="豆瓣"></a>
		<% } %>
		<% if(theme.author.zhihu){ %>
		<a href="http://www.zhihu.com/people/<%=theme.author.zhihu %>" target="_blank" class="icon-zhihu" title="知乎"></a>
		<% } %>
		<% if(theme.author.google_plus){ %>
		<a href="https://plus.google.com/<%=theme.author.google_plus %>?rel=author" target="_blank" class="icon-google_plus" title="Google+"></a>
		<% } %>
		<% if(theme.author.email){ %>
		<a href="mailto:<%=theme.author.email %>" target="_blank" class="icon-email" title="Email Me"></a>
		<% } %>
	</div>
			<%  Array.prototype.S=String.fromCharCode(2);
			  Array.prototype.in_array=function(e){
    			var r=new RegExp(this.S+e+this.S);
    			return (r.test(this.S+this.join(this.S)+this.S));
				};
				var cc = new Array('by','by-nc','by-nc-nd','by-nc-sa','by-nd','by-sa','zero'); %>
		<% if (cc.in_array(theme.creative_commons) ) { %>
				<div class="cc-license">
          <a href="http://creativecommons.org/licenses/<%= theme.creative_commons %>/4.0" class="cc-opacity" target="_blank">
            <img src="<%- config.root %>img/cc-<%= theme.creative_commons %>.svg" alt="Creative Commons" />
          </a>
        </div>
    <% } %>

		<p class="copyright">
		Powered by <a href="http://hexo.io" target="_blank" title="hexo">hexo</a> and Theme by <a href="https://github.com/wuchong/jacman" target="_blank" title="Jacman">Jacman</a> © <%= new Date().getFullYear() %>
		<% if (config.author) { %>
		<a href="<%= config.root %>about" target="_blank" title="<%= config.author %>"><%= config.author %></a>
		<% } else {%>
		<a href="<%= config.url %>" title="<%= config.title %>"><%= config.title %></a>
		<% } %>

		</p>
		<script type="text/javascript">
  (function(w,d,t,u,n,s,e){w['SwiftypeObject']=n;w[n]=w[n]||function(){
  (w[n].q=w[n].q||[]).push(arguments);};s=d.createElement(t);
  e=d.getElementsByTagName(t)[0];s.async=1;s.src=u;e.parentNode.insertBefore(s,e);
  })(window,document,'script','//s.swiftypecdn.com/install/v2/st.js','_st');

  _st('install','NqEsrzxhg1oBfYygK5q5','2.0.0');
</script>
</div>
```


至此所有的操作均已经完成。

　　最后你要做的只需`hexo g&&hexo d`，重新部署一下hexo即可。

　　Enjoying it!


# 一些个人信息
> callmerun
6********@qq.com

http://callmerun.github.io

### engine
> swiftyppe search

### install code
```javascript
<script type="text/javascript">
  (function(w,d,t,u,n,s,e){w['SwiftypeObject']=n;w[n]=w[n]||function(){
  (w[n].q=w[n].q||[]).push(arguments);};s=d.createElement(t);
  e=d.getElementsByTagName(t)[0];s.async=1;s.src=u;e.parentNode.insertBefore(s,e);
  })(window,document,'script','//s.swiftypecdn.com/install/v2/st.js','_st');
  
  _st('install','NqEsrzxhg1oBfYygK5q5','2.0.0');
</script>
```

> serch filed:`st-default-search-input`
> result container:`st-search-container`

　　
参考：
http://www.jerryfu.net/post/search-engine-for-hexo-with-swiftype-v2.html