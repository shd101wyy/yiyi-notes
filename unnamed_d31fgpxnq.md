---
note:
    createdAt: 2020-06-02T08:05:32.127Z
    modifiedAt: 2020-06-02T09:54:56.317Z
    tags: [backend, backend/maven]
    id: ""
---
# Maven proxy

Create `settings.xml` in `~/.m2`

```xml
<settings>
    <localRepository />
    <interactiveMode />
    <usePluginRegistry />
    <offline />
    <pluginGroups />
    <servers />
    <proxies>
        <proxy>
            <id>v2ray</id>
            <active>true</active>
            <protocol>socks5</protocol>
            <username></username>
            <password></password>
            <host>127.0.0.1</host>
            <port>1088</port>
            <nonProxyHosts>localhost|127.0.0.1</nonProxyHosts>
        </proxy>
    </proxies>
    <profiles />
    <activeProfiles /> 
</settings>
```

