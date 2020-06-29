---
tags:
    - backend/npm
id: ""
created: 2020-03-21T11:18:13.121Z
modified: 2020-03-21T11:39:10.900Z
---
# npm publish 413 Request Entity Too Large issue

Run the following to bypass the proxy

```bash
$ npm --registry https://registry.npmjs.org/ publish # --access=public
```