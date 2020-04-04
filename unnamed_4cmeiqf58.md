---
note:
    createdAt: 2020-04-02T03:13:23.534Z
    modifiedAt: 2020-04-02T03:38:37.802Z
    tags: [crossnote]
    id: ""
---
# Crossnote notebook publish design

用户需要在 README.md 中编写 notebook front-matter

```yaml
---
notebook:
  owner: shd101wyy
  name: Yiyi notes
---
```

然后才可以 Publish
当前只支持发布 GitHub，GitLab，以及码云 Gitee 的公开仓库。

---
PushlishNotebookDialog 设计

* We only support to publish a public git repository from GitHub, GitLab, and Gitee as a notebook to Crossnote.
* Please make sure you have added the `notebook` front-matter to your `README.md`. See the [instructions here](...)
* `Git URL`
* `Git Branch`
  `Publish the notebook` 
