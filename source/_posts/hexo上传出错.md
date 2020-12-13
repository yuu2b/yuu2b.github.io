---
title: hexo上传出错
author: yuu
avatar: https://cdn.jsdelivr.net/gh/yuu2b/cdn/img/custom/avatar.jpg
authorLink: nzynl.top
categories: 技术
comments: true
date: 2020-11-27 16:07:01
authorAbout: 一切节省，归根到底都归结为时间的节省。
authorDesc:
tags: blog
keywords:
description: 解决 hexo d 出现No such device or address错误
photos: https://cdn.jsdelivr.net/gh/yuu2b/cdn/img/blog/hexo上传出错/yly.jpg
---


hexo写好页面后使用 `hexo d` 命令上传。执行过程中弹出github的登陆窗口，确认账号密码正确后始终报以下错误：
```
Error: Logon failed, use ctrl+c to cancel basic credential prompt.
bash: /dev/tty: No such device or address
error: failed to execute prompt script (exit code 1)
fatal: could not read Username for 'https://github.com': No such file or directory
FATAL Something's wrong. Maybe you can find the solution here: https://hexo.io/docs/troubleshooting.html
```
因为之前都是正常的，应该不是我瞎操作导致的。网上查过一遍后发现删除.git .git_deploy 文件夹等方法均不能解决问题。然后我用小乌龟上传文件测试发现git还是会弹出登陆窗口，输入密码后似乎还是会产生错误，因为之后小乌龟又弹出了连接github的账户密码输入框。到此我基本判断是git出问题了。正巧**此时我收到一封邮件**：

![github邮件](https://cdn.jsdelivr.net/gh/yuu2b/cdn/img/blog/hexo上传出错/1.jpg)
啊这，我回想了一下**我的git是一年前装的，那就升级一下吧。于是安装好最新的git后成功解决。特此记录一下。