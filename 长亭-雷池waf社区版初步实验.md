---
tags:
  - PBC实验
date: 40_2023-10-31
---

进入官网查看帮助文档：
https://stack.chaitin.com/tool/detail/717 

雷池waf不开源，采用docker部署，帮助文档有一键安装运行命令

`bash -c "$(curl -fsSLk https://waf-ce.chaitin.cn/release/latest/setup.sh)"`

## 配置需求

- 操作系统：Linux
- 指令架构：x86_64
- 软件依赖：Docker 20.10.14 版本以上
- 软件依赖：Docker Compose 2.0.0 版本以上
- 最小化环境：1 核 CPU / 1 GB 内存 / 5 GB 磁盘

详细安装教程：https://waf-ce.chaitin.cn/docs/guide/install

## 快速使用

### 登录

浏览器打开后台管理页面 `https://192.168.211.129:9443`。根据界面提示，使用 **支持 TOTP 的认证软件** 扫描二维码，然后输入动态口令登录。

### 配置防护站点

雷池以反向代理方式接入，优先于网站服务器接收流量，对流量中的攻击行为进行检测和清洗，将清洗过后的流量转发给网站服务器。

![[waf配置防护站点.png]]
### NGINX配置

实体机开启nginx，配置如下：

```
server {
        listen       8000;
        server_name  somename  alias  another.alias;

        location / {
            proxy_pass  http://192.168.211.129:8000;
            proxy_set_header   X-Real-IP        $remote_addr;
            proxy_set_header   X-Forwarded-For  $remote_addr;
        }
    }
```

## 结果

![[Pasted image 20231030155846.png]]

在实体机浏览器上访问127.0.0.1:8000/DVWA，会跳转到waf地址192.168.211.129:8000，但实际显示的是DVWA web页面，正常的业务能访问，进行sql注入实验，发现会自动拦截。
![[Pasted image 20231030160146.png]]
![[Pasted image 20231030160209.png]]
![[Pasted image 20231030160254.png]]


## 其他

也可做黑白名单、高频访问限制、高频攻击限制等，我简单试了下，能正常使用。
雷池waf的UI界面做的很好看，但社区版阉割了一些功能，具体能拦截哪些攻击，能防护到什么程度还不清楚，因为本机配置限制，无法模拟小型网络，后续如果部署到本地机房测试使用，再深入了解。。