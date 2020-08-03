---
tags:
    - crossnote
    - crossnote/beta3
created: 2020-06-28T04:31:51.848Z
modified: 2020-07-17T07:28:23.826Z
---
# Crossnote Beta3 版本设计
#crossnote/beta3 

## Front-matter 修改

更改 Front-matter

```yaml
---
title: Title of the note, which should match the file name
created: Date this note is created
modified: Date this note is modified
pinned: false
favorited: false
---
```

## 左边栏 修改

删除左边栏 `标签`，`未标签` ，`待办事项` 项，添加 `快速访问`？

📆  今天          -> 📆  今天的笔记
📁  笔记          -> 🗒  所有的笔记
🕸  知识图      -> \*新增
⭐️  快速访问   -> \*新增
...
📎 附件            -> 保留
⚠️ 冲突            -> 保留
⚙️ 设置            -> \*新增

☑️  待办事项 -> 删除
🏷  标签          -> 删除
🈚 未标签       -> 删除
🔐 已加密       -> 删除

## 布局修改
左边栏
编辑器

## 编辑器功能 修改
删除 `加密`，未来 `加密` 变为 Widget。  
搜索 
* `linked: Crossnote Beta3 版本设计` 来显示引用的笔记列表
* `unlinked: Crossnote Beta3 版本设计`

## 想法

取消第二竖边栏显示笔记列表，改用 Graph View。  
例如一个笔记的 linked references 以及 unlinked references 则按照 references 和该笔记的距离差异来表示时间排序。
Check [D3-force](https://github.com/d3/d3-force)


