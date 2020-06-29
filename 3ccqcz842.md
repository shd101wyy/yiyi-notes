---
tags:
  - backend/postgresql
created: 2020-03-21T11:16:57.039Z
modified: 2020-03-21T11:39:04.699Z
---

# PostgreSQL max connections issue

While developing the application, I keep getting the issue like:

```
(/home/yiyiwang/Workspace/crossnote_backend/resolvers/chat.go:242)
[2019-12-17 11:51:39]  pq: sorry, too many clients already
```

After searching online, seems like I will need to do the following:
Modify `/etc/postgresql/10/main/postgresql.conf` file, and change `max_connections` and `shared_bufferes`

```
max_connections = 300
shared_buffers = 256MB
```
