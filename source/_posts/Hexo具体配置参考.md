title: Hexo具体配置参考 #可以改成中文的，如“新文章”
date: 2016-01-14 21:31:42 #发表日期，可自定义修改排序
categories: Blog
tags:
  - Blog
  - Hexo
---

如何使用 Jacman 主题（官方）

http://wuchong.me/blog/2014/11/20/how-to-use-jacman/

hexo具体配置参考

http://ibruce.info/2013/11/22/hexo-your-blog/

http://zipperary.com/archives/

[HEXO+Github,搭建属于自己的博客](http://www.jianshu.com/p/465830080ea9)

[使用GitHub和Hexo搭建免费静态Blog](http://wsgzao.github.io/post/hexo-guide/)

[如何搭建一个独立博客——简明 Github Pages与 jekyll 教程](http://cnfeat.com/blog/2014/05/10/how-to-build-a-blog/)

[一步步在GitHub上创建博客主页(1)](http://www.pchou.info/web-build/2013/01/03/build-github-blog-page-01.html)

[使用GitHub和Hexo搭建免费静态Blog](http://wsgzao.github.io/post/hexo-guide/)

[如何搭建一个独立博客——简明 Github Pages与 jekyll 教程](http://cnfeat.com/blog/2014/05/10/how-to-build-a-blog/)

[一步步在GitHub上创建博客主页(1)](
http://www.pchou.info/web-build/2013/01/03/build-github-blog-page-01.html)


Markdown语法
http://www.markdown.cn/


### 使用hexo + github过程中出现的错误：
1、error: failed to push some refs to
```
D:\快盘\GitHub\Callmerun.github.io [hexo +34 ~0 -0 | +34 ~1 -1 !]> git push orig
in hexo
To https://github.com/Callmerun/Callmerun.github.io.git
 ! [rejected]        hexo -> hexo (non-fast-forward)
error: failed to push some refs to 'https://github.com/Callmerun/Callmerun.githu
b.io.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
请查看：
[git push用法和常见问题分析](http://www.cnblogs.com/renkangke/archive/2013/05/31/conquerAndroid.html)


2、error: fatal: RPC failed; result=56, HTTP code = 200s  push错误
```
D:\快盘\GitHub\Callmerun.github.io [hexo +0 ~1 -0]> git push origin hexo
Counting objects: 175, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (157/157), done.
error: fatal: RPC failed; result=56, HTTP code = 200s
WThe remote end hung up unexpectedly
Writing objects: 100% (175/175), 2.60 MiB | 4.00 KiB/s, done.
Total 175 (delta 91), reused 0 (delta 0)
fatal: The remote end hung up unexpectedly
Everything up-to-date
```

https://confluence.atlassian.com/stashkb/git-clone-fails-error-rpc-failed-result-56-http-code-200-693897332.html
提供的解决方案：
**On Windows**
Execute the following in the command line before executing the Git command:

```
set GIT_TRACE_PACKET=1
set GIT_TRACE=1
set GIT_CURL_VERBOSE=1
```
