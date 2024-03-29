---
layout:     post
title:      GitHub-SSH
subtitle:   Connection closed by remote host
date:       2024-01-20 15:56:22 GMT+0800
author:     920
header-img: 
catalog: true
stickie: false
tags:
    - ssh
---


#### Connection closed


报错如下：

```
kex_exchange_identification: Connection closed by remote host
Connection closed by 127.0.0.1 port 7890
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```


究其原因`22`端口被屏蔽

**查看ip**

```
nslookup github.com

# 打印log
Server:     114.114.114.114
Address:    114.114.114.114#53

Non-authoritative answer:
Name:   github.com
Address: 20.205.243.166
```

**检测端口**

```
ssh 20.205.243.166 -p 22

# 打印log
ssh: connect to host 20.205.243.166 port 22: Operation timed out
```

#### 解决方案

[GitHub](https://docs.github.com/zh/authentication/troubleshooting-ssh/using-ssh-over-the-https-port)

修改 `~/.ssh/config`如下

```
Host github.com
  Hostname ssh.github.com
  Port 443
  User git
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```




