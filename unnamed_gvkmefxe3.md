---
tags:
  - crossnote/inspirations
created: 2020-06-14T11:54:42.559Z
modified: 2020-06-14T11:54:56.003Z
---

# 使用 TiddlyWiki 打造轻便个人 Wiki 知识库 - 钉子の次元

[Source](http://blog.dimpurr.com/tiddly-wiki/ "Permalink to 使用 TiddlyWiki 打造轻便个人 Wiki 知识库 - 钉子の次元")

这篇文章将简要介绍 TiddlyWiki 的特点，并且分享一些常用的参考链接、资源、插件和常见问题的解决方案，以方便有相近需求的小伙伴们。

TiddlyWiki ，按官方说法是一个「非线性个人 Web 笔记本」。相比其他笔记和 Wiki 系统，其最大的特点之一是程序本体和数据全部都在一个单 html 文件内，与此同时仍然有着非常强大的功能和插件系统。在我看来，管理以文字、代码和数学公式为主的个人知识库， TiddlyWiki 可谓是最好的选择之一。

可以在我的知识库站点「Dimpurr’s Knowledge Base \#1」：<http://note.dimpurr.com/> 体验一下 TiddlyWiki 的使用和效果。

注意，任意访客都可以体验到 TiddlyWiki 的全部功能，包括编辑和设置。不用担心，你可以随意折腾，因为你无法将更改保存到我的服务器上，只能下载到本地。

[![TiddlyWiki 效果预览](http://img1.dimpurr.com/dimblog/2017/10/tiddlywiki-1-1024x640.jpg)](http://img1.dimpurr.com/dimblog/2017/10/tiddlywiki-1.jpg)

TiddlyWiki 效果预览

我想找到一个能方便快捷的打打笔记和维护个人知识库 Wiki 的方式已经很久了。

笔记软件，例如 EverNote 、 WizNote 、 OneNote 的确十分不错，但是也会带来客户端是否跨平台、启动速度是否好看甚至默认文字排版是否美观的问题；而且，常规的笔记软件也达不到 Wiki 级别方便的 Tag 标签系统。当然， Wiki 系统有经典的 MediaWiki 系统，还有许许多多的静态 Wiki 系统、 Wiki 知识库类客户端。然而， MediaWiki 庞大、复杂和丑陋； Wiki 客户端程序有好有坏，有设计简陋也有强大美观，但是最大的限制还是往往不跨平台；一些可以用 Github Pages 部署的，基于 Markdown 的 Wiki 系统尽管几乎能在功能上满足我的需求，但是每一次撰写新条目和部署的复杂度还是令人难以接受。

你可能已经看出了我口味相当的刁钻和需求相当的诡异 …… 高中时因为没有时间折腾，我搭建了一个 Ghost 博客来存放简单的读书笔记，然而随着某次 VPS 上的 SQLite 被我搞跪了之后，天国的 WIKI\#0 除了遗留下来数据库里的几篇文章之外，就这样成为了历史。高中毕业的暑假来临，我决定动手寻找一款我需要的 Wiki 系统。于是，我找到了 TiddlyWiki 。

## TiddlyWiki 的特点

- **程序和数据全部存储在一个单文件 html 中**
  - 这让 TiddlyWiki 既可以在你的本机运行，类似一个在浏览器中运行的绿色版单文件应用程序；又可以上传到服务器上，和网络上的所有人分享
  - 同时部署极其简单，只需要一个能存放 html 的服务器，上传上去就可以使用，根本不需要 PHP、 Node.js、Python 或者其他什么语言环境，也没有任何配置步骤
  - 你可以随时再把服务器上的 TiddlyWiki 保存进本地或者 U 盘，以便带进任何没有网络的环境查阅
- **作为 Wiki 系统，有 Tag 标签和条目关联等必须的基本功能，和强大的编辑器**
  - 你能用 Tag 快速整理条目
  - 你能用 `[[条目名]]` 这样的语法快速链接到其他条目
  - 你能用条目名作为 Tag 其他条目，达到设置子条目的效果
  - 你甚至可以用 `<<list-links "[tag[ACM]sort[title]]-[tag[OJ 草稿和题解]]">>` 这样的过滤器生成一个包含特定 Tag ，但是删除掉另一个特定 Tag 的条目列表！
  - TiddlyWiki 使用一种类似 Markdown 但是稍有差异的语法，不过很快可以习惯并且非常好用
- **方便的插件和主题系统**
  - 可以通过插件支持代码高亮、 LaTex 数学公式、标准 Markdown 语法、文章嵌入 TODO 列表、条目加密锁定 ……
  - 可以安装 Material Design 风格、博客风格的样式主题 ……
- **外观和操作设计别致，使用体验好**
  - TiddlyWiki 是一个典型的单页面 Web 应用，所以打开的时候全部内容都已经载入和缓存在了浏览器中，换而言之你不需要刷新页面，操作和访问体验非常快速和流畅
  - TiddlyWiki 在右侧是搜索和多种方式的条目索引，而左边是可以卡片式展开多个和关闭的条目，还可以为特定的条目顺序和组合生成静态链接，浏览和使用十分方便

## 如何开始

进入 [http://tiddlywiki.com](http://tiddlywiki.com/) 官网，阅读下面的 GettingStarted 条目。这里根据你的浏览器版本会切换的对应的教程，不过总的来说，你只需要点击 Download Empty 按钮就可以下载好一个全新的 TiddlyWiki 的 html 文件了。或者，你可以按照官方说明通过 Node.js 从 npm 安装。

你已经可以开始本地体验和使用了。

注意，你可能需要去设置页面手动安装并启用中文语言包。

当然，我觉得大部分人应该都有上传到服务器上，以便发送地址和在线共享的需求。一般我们会需要把默认的 tiddlywiki.html 重命名成 index.html ，这样访问就很方便了。

不过如果你直接上传 html 文件到服务器，你会发现，每次保存都会重新向本地下载一个编辑后的 html ，然后你需要不厌其烦的用 FTP 再进行上传和替换 …… 其实，只需要简单的配置 PHP 或者 Node.js 保存服务，就可以解决这个问题。

额外的： [http://tiddlyspot.com](http://tiddlyspot.com/) 提供了一个似乎是免费的在线托管 TiddlyWiki 的服务，还提供了专门用于 GTD 的一些模板； [TiddlyDesktop](https://github.com/Jermolene/TiddlyDesktop/) 是一个 TiddlyWiki 专用的浏览器，或者说桌面客户端。

### PHP 保存

PHP 保存非常容易配置。官方的教程在这里：[http://tiddlywiki.com/\#Saving%20on%20a%20PHP%20Server](http://tiddlywiki.com/#Saving%20on%20a%20PHP%20Server)

1. 在 <https://code.google.com/archive/p/bidix/downloads> 下载一份 `TiddlyHome_*.*.*.zip`
2. 找到里面 `_th\lib\store.php` 这个文件，解压并编辑里面的 `$USERS = array( 'UserName1'=>'Password1', etc)` 为你想要的用户名和密码
3. 上传 store.php 到你的服务器，去 TiddlyWiki \> 保存 \> TiddlySpot 保存模块 ，设置 高级设置 \> 服务器网址 为这个 store.php 文件的完整地址，然后在上面填写用户名和密码
4. 现在，点击保存时，已经会直接保存在服务器上了
5. 注意：
6. 你可能需要把备份文件名设置成 index.html
7. 每次保存都会自动创建一份备份，你可以定期手动清理
8. 建议修改备份文件夹为 backup ，这样会把备份保存在 backup/ 子目录而非 . 根目录下

因为 TiddlyWiki 并没有用户登陆界面，这个设置页面就相当于登陆页面。密码是按浏览器保存的，所以如果你想在当前浏览器退出登录，到设置页面清除密码设置就好。如果你换了一个浏览器打开 Wiki ，你将需要进入设置重新填写一次密码(相当于登陆)，才能使用在线保存。

#### 可能遇到的 store.php 错误

如果你在如上配置完成后，点击保存后弹出正在保存 Wiki，此后就没有反应、并且在浏览器开发者工具中的 Network 网络面板看到 500 Internal Servel Error ，你可以尝试打开 PHP 的错误日志查看报错：

    vim  /usr/local/php/etc/php-fpm.conf
    php_flag[display_errors] = On # 直接在网页上显示错误信息
    #php_admin_value[error_log] = /usr/local/php/var/log/php_errors.log
    #php_admin_flag[log_errors] = on
    cat /usr/local/php/var/log/php-error.log # 或者直接在浏览器中查看报错

如果遇到关于 split() 函数的问题：

[![TiddlyWiki store.php split() error](http://img1.dimpurr.com/dimblog/2017/10/QQ%E5%9B%BE%E7%89%8720180316213404-600x340.png)](http://img1.dimpurr.com/dimblog/2017/10/QQ%E5%9B%BE%E7%89%8720180316213404.png)

TiddlyWiki store.php split() error

那么可能是你运行的 php 版本已经废弃这个函数，编辑 `store.php` 文件并查找替换所有的 `split`为 `explode` 即可。

## 可能用到的资源

我收集的 TiddlyWiki 相关资源，都会第一时间整理到我的知识库： [http://note.dimpurr.com/\#TiddlyWiki%20 使用](http://note.dimpurr.com/#TiddlyWiki%20%E4%BD%BF%E7%94%A8)

比较重要的包括：

- 第三方官网繁体中文翻译 [http://tw5-zh.tiddlyspot.com](http://tw5-zh.tiddlyspot.com/) (感谢 Bennyli 提醒)
- 编辑器标记语法参考 [http://tiddlywiki.com/\#WikiText](http://tiddlywiki.com/#WikiText)
- TiddlyWiki Community (官方整理的社区资源列表) [http://tiddlywiki.com/\#Community:Community%20Plugins](http://tiddlywiki.com/#Community:Community%20Plugins)
- tid.li Plugins (一个个人第三方插件源) <http://tid.li/tw5/plugins.html>
- CommunityPlugins (更大的一个第三方插件索引) [http://erwanm.github.io/tw-community-search/\#CommunityPlugins](http://erwanm.github.io/tw-community-search/#CommunityPlugins)

## 关于插件

需要注意的是， TiddlyWiki 最新的版本 5 有重大的变化，导致针对老版本设计的插件全部失效无法安装。你可能会在网上搜索到很多老版本的插件源，以及告诉你使用新建条件、粘贴插件代码内容的方式安装，都已经无法再使用了。所以，记得确认你找到的插件支持 TW5 。

目前，正确的插件安装方式除了在设置页面的官方插件源在线安装，对于第三方插件源来说，一般是你拖动第三方插件源提供的链接、图标或者按钮(不一定有效)，或者其设置页面的插件名称(一定有效)，拖动到你的 Wiki 页面上，完成导入安装。

我这里安利下我用到的插件，更多的可以在官方插件、主题市场和上面的插件源里自己发掘。

- TiddlyWiki 官方插件程式库
  - **Highlight.js: syntax highlighting** 代码高亮，程序员必备
  - **Markdown parser** 添加标准 Markdown 支持，如果你希望和 md 格式的平台互相导入和导出的话；大部分情况下，如果可以我建议使用原生 TiddlyWiki 语法，因为功能更加强大和对插件支持更好
  - **KaTeX: mathematical typography** 数学公式输入和排版
- **[MathJax](http://mathjax-tw5.kantorsite.net/)** 相比 KaTeX 更强大的 TeX 解析器
  - 为了兼容新版主题，你可能需要 [做点微小的工作](https://gist.github.com/kpe/cc0547b318e6f8d4ddaa#gistcomment-1885438) 修改一行插件代码
- [TiddlyWiki Community Search](http://erwanm.github.io/tw-community-search/#)
  - **[tw5-checklist](http://grosinger.net/tw5-checklist/)** 我经常使用的，一个轻量级在文章中嵌入 checklist 的插件，适合做些学习计划等
  - **Encrypt-Tiddler** 对单个条目启用输入密码查看
- [tid.li Plugins](http://tid.li/tw5/plugins.html)
  - **ToDoNow** 一个强大的简直有点过头的嵌入 Todolist 插件
  - **EditorCounter & Autosaver **为编辑器添加字数统计和一定字数更改后自动保存 (原生自带了条目修改确认和删除操作时自动保存功能，去设置里开启即可)
- **[TiddlyMap](http://tiddlymap.org/)** 一个强大的令人发指的流程图、思维导图等绘制插件

至于主题也有不少，不过我对默认的主题很满意 (你可能会发现 TiddlyWiki 的默认样式巧合的和我的 [Clearision](http://blog.dimpurr.com/clearision/) 博客主题灰色风格的设计十分相似) ，外加懒得折腾，所以就没有更换。

当然，尽管内容数据很难占据多少空间，安装过多不必要的插件却可能很快使 html 源文件尺寸增大，这点需要注意。

[![使用 MathJax 插件在 TiddlyWiki 显示数学公式](http://img1.dimpurr.com/dimblog/2017/10/tiddlywiki-mathjax-600x375.jpg)](http://img1.dimpurr.com/dimblog/2017/10/tiddlywiki-mathjax.jpg)

使用 MathJax 插件在 TiddlyWiki 显示数学公式

## 关于文本编辑

[![TiddlyWiki 表格排版](http://img1.dimpurr.com/dimblog/2017/10/tiddlywiki-table-1024x640.jpg)](http://img1.dimpurr.com/dimblog/2017/10/tiddlywiki-table.jpg)

TiddlyWiki 表格排版

请务必花点时间阅读编辑器标记语法参考 [http://tiddlywiki.com/\#WikiText](http://tiddlywiki.com/#WikiText) 的内容，你会发现十分值得。这里强调几点我觉得特别有用的内容。

插入图片的正确姿势是 `[img[http://img1.cheny.org/dptool/img/170921112724_v2-0d6d1cde06a90b753193b510e5b9a5a4_r.jpg]]` 。文本中的 URL 会被自动识别为链接，如果你想要给一段自定义文本设置超链接，试试 `[ext[个人成长/学习/考试/品格/自控/时间管理 - Dimpurr 的知乎收藏|https://www.zhihu.com/collection/104053246]]`(ext 大部分情况下可以省略)。

相比 Markdown 的用缩进排版， TiddlyWiki 的无序列表 \* 和有序列表 \# ，以及缩进子列表不用 Tab 而是用两次列表符号比如 \*\* 或者 \*\# 刚开始可能显得有点让人迷惑。习惯就好。

用 TiddlyWiki 排版表格真的非常的爽！你可以自由的设置表头，表尾，表名，每个单元格对齐方式，跨格，而且语法非常简单方便，输入流畅。请阅读官方文档 [http://tiddlywiki.com/\#Tables%20in%20WikiText](http://tiddlywiki.com/#Tables%20in%20WikiText) 。

前面展示过自动生成条件列表，并且按 Tag 过滤的「魔法」。参考：[http://tiddlywiki.com/\#Transclusion%20in%20WikiText](http://tiddlywiki.com/#Transclusion%20in%20WikiText) ，记得阅读底部的 See also 详细说明！

如果你愿意折腾，你可以尝试学习 Macro 宏和 Variables 变量的用法。

TiddlyWiki 有时会把符合 PascalCase / UpperCamelCase 的词自动识别为条目链接。你只需要在前面加入一个波浪线 ~ 转义，比如 ~TiddlyWiki ，就会恢复为纯文本。

安装了 Highlight.js 插件后，你可以这样指定代码块使用的高亮语法：

    ```bash
    ➜ ~ pwd
    /Users/dimpurr
    ```

在 Highlight.js 插件设置页面你可以找到支持的语法列表。注意所有 shell 命令的标识符是 bash ，而不是 sh 或者其他的什么。

## 一些小问题

### 禁用搜索最小字数限制

TiddlyWiki 默认的搜索框存在字符数限制，要求搜索关键词大于三个字符。对于英语环境来说这很合理，但是对于中文来说，二字词的搜索是很常见的，因此很不方便。修改这个设置只需要：

- 添加一个标题为 `$:/config/Search/MinLength` 的新条目
- 内容为 `1`

### 禁用自动 WikiLink

TiddlyWiki 默认自动会把符合 CamelCase 的文本替换为条目链接，称为 WikiLinks 或者 WikiWords 。一般来说你可以用 ~WikiText 来转义禁止链接，但是对于长篇文章这样做实在辛苦。网上流传的基本是直接禁用 WikiLink 功能的方法，但是这样之前 ~ 反转移过的文字又会显示出 ~ 符号。

一个既能让之前的 ~ 不显示，同时也不会有自动链接的方法是：

1. 点击搜索框旁边的图标进入 AdvancedSearch
2. 搜索 `$:/core/modules/parsers/wikiparser/rules/wikilink.js` ，或者点击 `$:/core` 再找到这个条目，点击进入编辑
3. 系统会提示 `这是一个修改过的默认条目。删除此条目可以还原为 $:/core 插件中的默认版本。` 因此不用担心
4. 找到最后面的 `return [{return [{ type: "link",` ，在这一行上面加上一行 `return [{type: "text", text: linkText}];` ，以便在本要返回链接的时候返回纯文本

### 使用树状结构组织条目内容

如果你对条目的组织结构有强迫症，又不像我一样觉得内容零散到只能手动编写目录，使用自带的树状目录系统是个好选择。

    <div class="tc-table-of-contents">
    <<toc-selective-expandable 'Contents' sort[title]>>
    </div>

通过以上代码可以创建以 Contents 条目(该条目不会显示出来)为根节点，按照标签关系嵌套的树状目录结构列表。

通过将这个条目命名为「目录」，并加上 `$:/tags/SideBar` 标签，就可以使这个目录显示到侧边栏。效果如下：

[![TiddlyWiki 树状目录](http://img1.dimpurr.com/dimblog/2017/10/TIM%E6%88%AA%E5%9B%BE201712161811101-600x298.png)](http://img1.dimpurr.com/dimblog/2017/10/TIM%E6%88%AA%E5%9B%BE201712161811101.png)

TiddlyWiki 树状目录
