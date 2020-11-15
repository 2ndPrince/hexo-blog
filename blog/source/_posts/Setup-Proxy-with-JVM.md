---
title: Setup-Proxy-with-JVM
catalog: true
date: 2020-11-15 13:49:37
subtitle:
header-img:
tags:
- Proxy
categories:
- From-Previous-Blog-General
---

# Setup Proxy with JVM

## 1. Overview

Companies use their own proxy. We need proxy settings in our code base.
In this post, we are going to explore how to set proxy properties using JVM.

---

## 2. What is proxy?

``` lang=html
A proxy server allows indirect connection to network services 
and is used mainly for security (to get through firewalls) 
and performance reasons (proxies often do provide caching mechanisms).
```

## 3. Setup a proxy

``` lang=html
**http.proxyHost **(default: <none>)
The hostname, or address, of the proxy server

**http.proxyPort** (default: 80)
The port number of the proxy server.

**http.nonProxyHosts** (default: localhost|127.*|[::1])
```

You can do this with JVM options.

``` lang=bash
JAVA_FLAGS=-Dhttp.proxyHost=10.0.0.100 -Dhttp.proxyPort=8800 
-Dhttp.nonProxyHosts="localhost|127.0.0.1|10.*.*.*|*.foo.com‌​|etc"
```
