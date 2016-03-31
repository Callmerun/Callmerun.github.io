---
title: 重新配置GitHub + Hexo
date: 2016-03-31 10:38:56
categories: Blog
tags:
  - GitHub
  - Hexo
---
Hexo出现错误：切换分支失败
```
D:\快盘\GitHub\Callmerun.github.io [master +21 ~0 -0 !]>  git checkout hexo
error: The following untracked working tree files would be overwritten by checko
ut:
        .gitignore
        .npmignore
        README.md
        _config.yml
        about/index.html
        archives/2015/01/index.html
        archives/2015/index.html
        archives/2016/01/index.html
        archives/2016/index.html
        archives/index.html
        atom.xml
        categories/blog/index.html
        css/style.css
        fancybox/blank.gif
        fancybox/fancybox_loading.gif
        fancybox/fancybox_loading@2x.gif
        fancybox/fancybox_overlay.png
        fancybox/fancybox_sprite.png
        fancybox/fancybox_sprite@2x.png
        fancybox/helpers/fancybox_buttons.png
        fancybox/helpers/jquery.fancybox-buttons.css
        fancybox/helpers/jquery.fancybox-buttons.js
        fancybox/helpers/jquery.fancybox-media.js
        fancybox/helpers/jquery.fancybox-thumbs.css
        fancybox/helpers/jquery.fancybox-thumbs.js
        fancybox/jquery.fancybox.css
        fancybox/jquery.fancybox.js
        fancybox/jquery.fancybox.pack.js
        font/FontAwesome.otf
        font/coveredbyyourgrace-webfont.eot
        font/coveredbyyourgrace-webfont.svg
        font/coveredbyyourgrace-webfont.ttf
        font/coveredbyyourgrace-webfont.woff
        font/fontawesome-webfont.eot
        font/fontawesome-webfont.svg
        font/fontawesome-webfont.ttf
        font/fontawesome-webfont.woff
        font/fontdiao.eot
        font/fontdiao.svg
        font/fontdiao.ttf
        font/fontdiao.woff
        img/author.jpg
        img/author.png
        img/banner.jpg
        img/cc-by-nc-nd.svg
        img/cc-by-nc-sa.svg
        img/cc-by-nc.svg
        img/cc-by-nd.svg
        img/cc-by-sa.svg
        img/cc-by.svg
        img/cc-zero.svg
        img/favicon.ico
        img/jacman.jpg
        img/logo.png
        img/logo.svg
        img/scrollup.png
        index.html
        js/gallery.js
        js/jquery-2.0.3.min.js
        js/jquery.imagesloaded.min.js
        js/jquery.qrcode-0.12.0.min.js
        js/totop.js
        package.json
        post/Java-泛型/index.html
        post/关于Java多线程的一些知识补充/index.html
        post/第一篇文章/index.html
        scaffolds/draft.md
        scaffolds/page.md
        scaffolds/post.md
        sitemap.xml
        source/_posts/Atom软件的一些使用纪要.md
        source/_posts/CentOS一些常用命令.md
        source/_posts/CentOS更改yum源与更新系统.md
        source/_posts/CentOS服务器初始化设置.md
        source/_posts/GitHub-Pages-Hexo搭建博客.md
        source/_posts/Hexo具体配置参考.md
        source/_posts/JAVA程式師面試32問（含答案）.md
        source/_posts/Java-Thread-join-详解.md
        source/_posts/Java-小项目（待写）.md
        source/_posts/Java-泛型.md
        source/_posts/Java中用双缓冲技术消除闪烁.md
        source/_posts/Java学习-持有对方引用.md
        source/_posts/Java练手小項目.md
        source/_posts/Java面试题.md
        source/_posts/Linux(CentOS6) 调整home 挂载 分区大小.md
        source/_posts/String参数传递问题.md
        source/_posts/eclipse最有用快捷键整理.md
        source/_posts/hello-world.md
        source/_posts/windows下的一些实用批处理.md
        source/_posts/关于Java多线程的一些知识补充.md
        source/_posts/利用swiftype为hexo添加站内搜索.md
        source/_posts/单例模式.md
        source/_posts/常见Linux错误.md
        source/_posts/简单Java-Socket聊天室.md
        source/_posts/简单网络爬虫-以及Email正则表达式.md
        source/_posts/转-循序渐进Java-Socket网络编程（多客户端、信息共享、文件传
输）.md
        source/about/index.md
        source/search/index.md
        tags/Java/index.html
        tags/博客/index.html
        tags/多线程/index.html
        tags/文章/index.html
        themes/landscape/.npmignore
        themes/landscape/Gruntfile.js
        themes/landscape/LICENSE
        themes/landscape/README.md
        themes/landscape/_config.yml
        themes/landscape/languages/default.yml
        themes/landscape/languages/nl.yml
        themes/landscape/languages/no.yml
        themes/landscape/languages/ru.yml
        themes/landscape/languages/zh-CN.yml
        themes/landscape/languages/zh-TW.yml
        themes/landscape/layout/_partial/after-footer.ejs
        themes/landscape/layout/_partial/archive-post.ejs
        themes/landscape/layout/_partial/archive.ejs
        themes/landscape/layout/_partial/article.ejs
        themes/landscape/layout/_partial/footer.ejs
        themes/landscape/layout/_partial/google-analytics.ejs
        themes/landscape/layout/_partial/head.ejs
        themes/landscape/layout/_partial/header.ejs
        themes/landscape/layout/_partial/mobile-nav.ejs
        themes/landscape/layout/_partial/post/category.ejs
        themes/landscape/layout/_partial/post/date.ejs
        themes/landscape/layout/_partial/post/gallery.ejs
        themes/landscape/layout/_partial/post/nav.ejs
        themes/landscape/layout/_partial/post/tag.ejs
        themes/landscape/layout/_partial/post/title.ejs
        themes/landscape/layout/_partial/sidebar.ejs
        themes/landscape/layout/_widget/archive.ejs
        themes/landscape/layout/_widget/category.ejs
        themes/landscape/layout/_widget/recent_posts.ejs
        themes/landscape/layout/_widget/tag.ejs
        themes/landscape/layout/_widget/tagcloud.ejs
        themes/landscape/layout/archive.ejs
        themes/landscape/layout/category.ejs
        themes/landscape/layout/index.ejs
        themes/landscape/layout/layout.ejs
        themes/landscape/layout/page.ejs
        themes/landscape/layout/post.ejs
        themes/landscape/layout/tag.ejs
        themes/landscape/package.json
        themes/landscape/scripts/fancybox.js
        themes/landscape/source/css/_extend.styl
        themes/landscape/source/css/_partial/archive.styl
        themes/landscape/source/css/_partial/article.styl
        themes/landscape/source/css/_partial/comment.styl
        themes/landscape/source/css/_partial/footer.styl
        themes/landscape/source/css/_partial/header.styl
        themes/landscape/source/css/_partial/highlight.styl
        themes/landscape/source/css/_partial/mobile.styl
        themes/landscape/source/css/_partial/sidebar-aside.styl
        themes/landscape/source/css/_partial/sidebar-bottom.styl
        themes/landscape/source/css/_partial/sidebar.styl
        themes/landscape/source/css/_util/grid.styl
        themes/landscape/source/css/_util/mixin.styl
        themes/landscape/source/css/_variables.styl
        themes/landscape/source/css/fonts/FontAwesome.otf
        themes/landscape/source/css/fonts/fontawesome-webfont.eot
        themes/landscape/source/css/fonts/fontawesome-webfont.svg
        themes/landscape/source/css/fonts/fontawesome-webfont.ttf
        themes/landscape/source/css/fonts/fontawesome-webfont.woff
        themes/landscape/source/css/images/banner.jpg
        themes/landscape/source/css/style.styl
        themes/landscape/source/fancybox/blank.gif
        themes/landscape/source/fancybox/fancybox_loading.gif
        themes/landscape/source/fancybox/fancybox_loading@2x.gif
        themes/landscape/source/fancybox/fancybox_overlay.png
        themes/landscape/source/fancybox/fancybox_sprite.png
        themes/landscape/source/fancybox/fancybox_sprite@2x.png
        themes/landscape/source/fancybox/helpers/fancybox_buttons.png
        themes/landscape/source/fancybox/helpers/jquery.fancybox-buttons.css
        themes/landscape/source/fancybox/helpers/jquery.fancybox-buttons.js
        themes/landscape/source/fancybox/helpers/jquery.fancybox-media.js
        themes/landscape/source/fancybox/helpers/jquery.fancybox-thumbs.css
        themes/landscape/source/fancybox/helpers/jquery.fancybox-thumbs.js
        themes/landscape/source/fancybox/jquery.fancybox.css
        themes/landscape/source/fancybox/jquery.fancybox.js
        themes/landscape/source/fancybox/jquery.fancybox.pack.js
        themes/landscape/source/js/script.js
Please move or remove them before you can switch branches.
Aborting

Warning: Your console font probably doesn't support Unicode. If you experience s
trange characters in the output, consider switching to a TrueType font such as C
onsolas!
```

解决方案：
```
D:\快盘\GitHub\Callmerun.github.io [master +21 ~0 -0 !]>  git branch -D hexo
Deleted branch hexo (was be8f8c5).
D:\快盘\GitHub\Callmerun.github.io [master +21 ~0 -0 !]> git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        .gitignore
        .npmignore
        README.md
        _config.yml
        about/
        archives/
        atom.xml
        categories/
        css/
        fancybox/
        font/
        img/
        index.html
        js/
        package.json
        post/
        scaffolds/
        sitemap.xml
        source/
        tags/
        themes/

nothing added to commit but untracked files present (use "git add" to track)
D:\快盘\GitHub\Callmerun.github.io [master +21 ~0 -0 !]> git branch
* master
D:\快盘\GitHub\Callmerun.github.io [master +21 ~0 -0 !]> git branch hexo
D:\快盘\GitHub\Callmerun.github.io [master +21 ~0 -0 !]> git checkout hexo
Switched to branch 'hexo'
D:\快盘\GitHub\Callmerun.github.io [hexo +21 ~0 -0 !]> dir


    目录: D:\快盘\GitHub\Callmerun.github.io


Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----         2016/3/31      9:28            .deploy_git
d----         2016/3/31      9:28            about
d----         2016/3/31      9:28            archives
d----         2016/3/31      9:28            categories
d----         2016/3/31      9:28            css
d----         2016/3/31      9:28            fancybox
d----         2016/3/31      9:28            font
d----         2016/3/31      9:28            img
d----         2016/3/31      9:28            js
d----         2016/3/31      9:28            node_modules
d----         2016/3/31      9:28            post
d----         2016/3/31      9:28            public
d----         2016/3/31      9:28            scaffolds
d----         2016/3/31      9:28            source
d----         2016/3/31      9:28            tags
d----         2016/3/31      9:28            themes
-a---          2016/3/2     16:06        395 .gitattributes
-a---         2016/1/25     17:55         65 .gitignore
-a---          2016/3/2     15:46         65 .npmignore
-a---         2016/1/25     17:16       8060 atom.xml
-a---          2016/3/2     15:46        174 db.json
-a---         2016/1/25     17:16      15758 index.html
-a---          2016/3/2     15:46        442 package.json
-a---         2016/1/25     14:39        354 README.md
-a---         2016/1/25     17:16        770 sitemap.xml
-a---          2016/3/2     15:46       1483 _config.yml


D:\快盘\GitHub\Callmerun.github.io [hexo +21 ~0 -0 !]>
```
