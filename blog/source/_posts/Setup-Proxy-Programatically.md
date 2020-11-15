---
title: Setup-Proxy-Programatically
catalog: true
date: 2020-11-15 13:53:11
subtitle:
header-img:
tags:
- Proxy
categories:
- From-Previous-Blog-General
---

# Setup Proxy Programatically

## 1. Overview

In the previous post, we've setup a proxy using JVM option.
In this post, we are going to explore how to setup a proxy from our code base.
There are benefits doing so.

---

## 2. Make a configuration bean

Create the following three field attributes to contain proxy configs.

``` lang=java
private String proxyHost;
private String proxyPort;
private String nonProxyHosts;
```

## 3. Use @PropertySource annotation

@PropertySource("classpath:my-proxy.properties")
This annotation should be used inside @configuration.

``` lang=html
@PropertySource

Annotation providing a convenient and declarative mechanism for adding a
PropertySource to Spring's Environment. To be used in conjunction with
Configuration classes.
```

## 4. Use @Value and @PostConstruct annotations

The final configuration would look like this;

``` lang=java
@Configuration
@PropertySource("classpath:my-proxy.properties")
public class WebProxy {

    @Value("${me.2ndPrince.demo.proxyHost}")
    private String proxyHost;

    @Value("${me.2ndPrince.demo.proxyPort}")
    private String proxyPort;

    @Value("${me.2ndPrince.demo.nonProxyHosts}")
    private String nonProxyHosts;

    @PostConstruct
    public void enableMyCompanyProxy() {
        System.setProperty("http.proxyHost", proxyHost);
        System.setProperty("http.proxyPort", proxyPort);
        System.setProperty("http.nonProxyHosts", nonProxyHosts);

        System.setProperty("https.proxyHost", proxyHost);
        System.setProperty("https.proxyPort", proxyPort);
        System.setProperty("https.nonProxyHosts", nonProxyHosts);
    }
}
```
