---
tags: []
created: 2020-07-02T13:17:53.747Z
modified: 2020-07-02T13:19:11.287Z
---
# Check port in use on Ubuntu

```bash
sudo lsof -i -P -n
```

or

```bash
sudo netstat -tulpn | grep LISTEN
```