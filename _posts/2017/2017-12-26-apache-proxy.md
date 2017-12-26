---
layout: post
title: apache httpd proxy 配置
category: apache
tags: [httpd,proxy]
---

 小机器多用途，当然也可以转到其它机器

# 主要就是不想加端口，没有复杂的原因

apache httpd 对其它机器转发，centos安装httpd后发现默认就LoadModule所有，所以配置就不用开这开那了

## 安装httpd

centos里安装

```sh
yum -y install httpd
```

## 修改配置文件

 找到这个文件/ect/httpd/conf/httpd.conf

```conf
NameVirtualHost *:80

<VirtualHost *:80>
    ServerName gp.2os.win
    ServerAlias gp.2os.win
    ProxyPass / http://localhost:9001/
    ProxyPassReverse / http://localhost:9001/
</VirtualHost>

<VirtualHost *:80>
    ServerName gp2.2os.win
    ServerAlias gp2.2os.win
    ProxyPass / http://localhost:9002/
    ProxyPassReverse / http://localhost:9002/
</VirtualHost>

```

## 会出现的小状况

代码中直接获取用户IP的代码就修改一下，因为转发了一次，取的IP就是转发机器的了

```java
public String getRemortIP(HttpServletRequest request) {
  if (request.getHeader("x-forwarded-for") == null) {
   return request.getRemoteAddr();
  }
  return request.getHeader("x-forwarded-for");
}
```

当然也可能被转了好几次才过来，转的不一定也是httpd

```java
public String getRemoteHost(HttpServletRequest request){
    String ip = request.getHeader("x-forwarded-for");
    if(ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)){
        ip = request.getHeader("Proxy-Client-IP");
    }
    if(ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)){
        ip = request.getHeader("WL-Proxy-Client-IP");
    }
    if(ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)){
        ip = request.getRemoteAddr();
    }
    return ip.equals("0:0:0:0:0:0:0:1")?"127.0.0.1":ip;
}
```
