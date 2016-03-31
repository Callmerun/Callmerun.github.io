---
title: GitHub Pages + Hexo搭建博客
date: 2016-01-26 09:33:39
categories: Blog
tags:
  - GitHub
  - Hexo
---

# 概述

这是一篇使用GitHub Pages和Hexo搭建免费独立博客的总结。主要摘抄重要的部分，想要看完整详细版的请到出处：
[GitHub Pages + Hexo搭建博客 ](http://Callmerun.github.io/2015/12/28/GitHub-Pages-Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/)

Hexo部署到GitHub上的文件，是.md（你的博文）转化之后的.html（静态网页）。因此，当你重装电脑或者想在不同电脑上修改博客时，就不可能了（除非你自己写html o(^▽^)o ）。

其实，Hexo生成的网站文件中有.gitignore文件，因此它的本意也是想我们将Hexo生成的网站文件存放到GitHub上进行管理的（而不是用U盘或者云备份啦(╬▔皿▔)凸）。这样，不仅解决了上述的问题，还可以通过git的版本控制追踪你的博文的修改过程，是极赞的。

但是，如果每一个GitHub Pages都需要创建一个额外的仓库来存放Hexo网站文件，我感觉很麻烦（10个项目需要20个仓库(ˉ▽ˉ；)…）。

所以，我利用了分支！！！

简单地说，每个想建立GitHub Pages的仓库，起码有两个分支，一个用来存放Hexo网站的文件，一个用来发布网站。

下面以我的博客作为例子详细地讲述。

<!--more-->

------
# 我的博客搭建流程

## 准备工作：
### 1.安装Git或者Github for Windows

下载 msysgit或者[Github for Windows](https://windows.github.com/)并执行即可完成安装。


### 2.安装Node.js

[Node.js](https://nodejs.org/en/)

在 Windows 环境下安装 Node.js 非常简单，仅须下载安装文件并执行即可完成安装。

`注意` 安装完成后添加Path环境变量，使npm命令生效。新版已经会自动配置Path
`;C:\Program Files\nodejs\node_modules\npm`

## 正式安装配置


> * 创建仓库，`Callmerun.github.io`；

> * 创建两个分支：master 与 hexo；


> * 在`github`网站上设置`hexo为默认分支`（因为我们只需要手动管理这个分支上的Hexo网站文件）；
<font Color=Magenta>当你想要在你的分支上提交内容，请确保是在你的那个分支上。</font>
```
在本地新建一个分支： git branch hexo
切换到你的新分支: git checkout hexo
查看当前分支：git branch
```

> * 使用`git clone git@github.com:Callmerun/Callmerun.github.io.git`拷贝仓库；

> * 在`git shell`或者`Git bash`上进入`本地Callmerun.github.io文件夹`下依次执行
`npm install hexo`
`hexo init`（如果是本地资料丢失，这个就不需要执行了）
`npm install`
`npm install hexo-deployer-git --save`（此时当前分支应显示为`hexo`）;

> * 设置`hexo为默认分支`（因为我们只需要手动管理这个分支上的Hexo网站文件）；

> * 使用`git clone git@github.com:Callmerun/Callmerun.github.io.git`拷贝仓库；

> * 在`本地Callmerun.github.io文件夹`下通过`Git bash`依次执行
`npm install hexo`
`hexo init`
`npm install`
`npm install hexo-deployer-git`（此时当前分支应显示为`hexo`）;


> * 修改`_config.yml`中的deploy参数，`分支应为master`；
```
# Deployment  站点部署到github要配置
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  message: "update"
  repository: https://github.com/Callmerun/Callmerun.github.io.git
  branch: master
  #branch: gh-pages
```

> * 依次执行
`git add .`
`git commit -m “…”`
`git push origin hexo`提交网站相关的文件；

> * 执行`hexo generate -d`生成网站并部署到GitHub上。

这样一来，在GitHub上的Callmerun.github.io仓库就有两个分支，一个hexo分支用来存放网站的原始文件，一个master分支用来存放生成的静态网页。完美( •̀ ω •́ )y！

------
# 我的博客管理流程

### 1.日常修改
在本地对博客进行修改（添加新博文、修改样式等等）后，通过下面的流程进行管理：
<font Color=Magenta>当你想要在你的分支上提交内容，请确保是在你的那个分支hexo上。</font>

> * 依次执行
`git add .`
`git commit -m “…”`
`git push origin hexo`指令将改动推送到GitHub（此时当前分支应为`hexo`）；
> * 然后才执行`hexo generate -d`发布网站到master分支上。

虽然两个过程顺序调转一般不会有问题，不过逻辑上这样的顺序是绝对没问题的（例如突然死机要重装了，悲催….的情况，调转顺序就有问题了）。

### 2.本地资料丢失

<font Color=Red>重装前先从GitHub上下载备份好资料，重要是事说三遍！！！！！</font>
当重装电脑之后，或者想在其他电脑上修改博客，可以使用下列步骤：

> * 使用`git clone git@github.com:Callmerun/Callmerun.github.io.git`拷贝仓库（<font Color=Red>默认分支为`hexo`，重要的事说三遍！！！！！！！</font>）；
或者是从云盘下载下来，最重要的是要保持配置要一致，如_config.yml、主题文件配置、_post文件夹下的文章文件

> * 显示样式：***<font Color=Red>D:\快盘\GitHub\Callmerun.github.io [hexo +21 ~0 -0 !]></font>***

当重装电脑之后，或者想在其他电脑上修改博客，可以使用下列步骤：
> * 使用`git clone git@github.com:Callmerun/Callmerun.github.io.git`拷贝仓库（<font Color=Red>默认分支为`hexo`</font>）；


> * 在<font Color=Magenta>本地新拷贝的Callmerun.github.io文件夹</font>下通过Git bash依次执行下列指令：
`npm install hexo`
`npm install`
`npm install hexo-deployer-git --save`（特别注意记得，`不需要hexo init`这条指令，重要的事说三遍！！！！！！）。


#### 有关于GitHub分支的一下命令
```
在本地新建一个分支： git branch hexo
切换到你的新分支: git checkout hexo
将新分支发布在github上： git push origin hexo
在本地删除一个分支： git branch -d hexo
在github远程端删除一个分支： git push origin :hexo   (分支名前的冒号代表删除)
```

------

#### Hexo简写命令
```
hexo n #生成文章，或者source\_posts手动编辑
hexo s #本地发布预览效果
hexo g #生成public静态文件
最后我选择手动同步更新至GitHub
```

------


# Markdown文件头格式(即`front-matter`)：
在scaffolds文件夹下面的post.md文件设置：
    ---
    title: GitHub Pages + Hexo搭建博客
    date: 2016-01-26 09:33:39
    categories: Blog
    tags:
      - GitHub
      - Hexo
      - 博客
    ---

------
**注意：**
> * `_config.yml`文件中的每个`属性值`前面必须留一个`空格`，建议在 Sublime/Notepad++ 中开启显示所有空格模式。
> * 另每篇文章的 `front-matter` 也要注意这个问题。

# Markdown语法手册：
[Cmd Markdown 简明语法手册 作业部落](https://www.zybuluo.com/mdeditor?url=https://www.zybuluo.com/static/editor/md-help.markdown)
[Cmd Markdown 高阶语法手册 作业部落](https://www.zybuluo.com/mdeditor?url=https://www.zybuluo.com/static/editor/md-help.markdown#cmd-markdown-%E9%AB%98%E9%98%B6%E8%AF%AD%E6%B3%95%E6%89%8B%E5%86%8C)

# 参考
[hexo系列教程：（三）hexo博客的配置、使用](http://zipperary.com/2013/05/29/hexo-guide-3/)
[使用GitHub来管理博客源文件 ](http://wuchong.me/blog/2014/01/17/use-github-to-manage-hexo-source/)
