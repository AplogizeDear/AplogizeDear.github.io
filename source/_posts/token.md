---
layout: []
title: github启用 personal access token后，历史项目如何进行认证变更
date: 2021-08-14 10:18:13
tags:
---

今天在修改了远程的access token之后，本地提交博客内容的时候便不能再使用了，报了以下错误

Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.

接下来查了很多资料后，大概知道怎么处理了。以下是我处理的方式，我在这里汇总一下

1. 如果忘记了你远程的access token的话，就去你的gihub下面修改你自己accesstoken

   点开你的头像setting - 左侧按钮Developer setting - personal access tokens

2. Generate new token 重新生成你自己的token，如果你的Travis配置过token的话，在此也要重新设置你的token

3. 到了这里你的代码还是不能提交的，接下来你要修改你本地的密码。我使用的是mac，所以这里以为mac为准，window的没有试过。

4. 首先重置你本地的密码 

   ```gitHub
    git config --local credential.helper ""
   ```

5. 然后这个时候就会提示你输入账号密码

   注意：此时的密码就是你重新设置后的 token

   然后你就可以正常的玩耍了。

7. 如果你此时你有使用Travis-ci 打包的代码，此时应该修改你的access token。

