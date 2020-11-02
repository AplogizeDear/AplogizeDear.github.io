---
title: git命令
date: 2020-11-02 23:04:12
categories: 知识
tags: git使用
---

1. 工作了很久，总结一下自己经常用到的命令，拓展一下不知道的git命令

   常用命令

   ```
   git add .   (添加所有文件到缓存区)
   git commit -m '你的描述' （提交暂存区到仓库区）
   git push '你要提交的分支代码' （提交代码到远程）
   
   git remote -v 查看远程仓库
   
   git branch -a  查看所有分支
   
   git checkout -b '分支'   建立一个分支，并且切换到新建的分支
   
   git checkout '分支'  切换到某个已经有的分支
   
   git merge '分支'  将分支代码合并到当前工作的分支
   
   git status  查看当前状态
   
   git fetch [remote]  下载远程仓库的所有变动
   ```

   以上就是经常用到的git命令

2. 学一些不常用的

   ```bash
   //分支
   git branch [branch-name]  新建一个分支，但是还停留在当前分支
   git branch -r   列出所有远程分支
   
   // 删除
   git branch -d [branch-name]  删除本地分支
   git push origin --delete [branch-name]  删除远程分支
   
   // 查看
   git shortlog -sn    查看所有提交过代码的人，按照提交次数排序
   
   
   撤销
   git reset [commit]  重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
   
   git reset --hard [commit]   同时重置暂存区和工作区，与指定commit一致
    
   ```

3.  大多数公司在提交的时候会设置上传的eslint检测，其实这个就是代码提交规范，涉及到了git，在这里写一下。作用嘛就是有利于快速查找到某个版本

   3.1 一般是 **git commit -m 'feat:做了什么样子的功能'**  其他的类似

   ```
   公式：<type>(<scope>): <subject>
   
   feat：新功能（feature）
   fix：修补bug
   docs：文档（documentation）
   style： 格式（不影响代码运行的变动）
   refactor：重构（即不是新增功能，也不是修改bug的代码变动）
   test：增加测试
   chore：构建过程或辅助工具的变动
   ```

   ###### 3.2 项目中使用

   `validate-commit-msg`  来检查项目中 `Commit message` 是否规范。

   1. ```
      npm install --save-dev validate-commit-msg
      ```

   2. 方式一：建立 `.vcmrc` 文件

      ```
      {
        "types": ["feat", "fix", "docs", "style", "refactor", "perf", "test", "build", "ci", "chore", "revert"],
        "scope": {
          "required": false,
          "allowed": ["*"],
          "validate": false,
          "multiple": false
        },
        "warnOnFail": false,
        "maxSubjectLength": 100,
        "subjectPattern": ".+",
        "subjectPatternErrorMsg": "subject does not match subject pattern!",
        "helpMessage": "",
        "autoFix": false
      }
      ```

   3. 使用方式二：写入 `package.json`

      ```
      {
        "config": {
          "validate-commit-msg": {
            /* your config here */
          }
        }
      }
      ```

   4. `validate-commit-msg` 插件地址

      https://github.com/conventional-changelog-archived-repos/validate-commit-msg

   5. ghooks 钩子

      ```
      {
        …
        "config": {
          "ghooks": {
            "pre-commit": "gulp lint",
            "commit-msg": "validate-commit-msg",
            "pre-push": "make test",
            "post-merge": "npm install",
            "post-rewrite": "npm install",
            …
          }
        }
        …
      }
      ```

   6. ###### 生成 Change log

      ```
      npm install -g conventional-changelog
      cd jartto-domo
      conventional-changelog -p angular -i CHANGELOG.md -w
      ```

      为了方便使用，可以将其写入 `package.json` 的 `scripts` 字段。

      ```
      {
        "scripts": {
          "changelog": "conventional-changelog -p angular -i CHANGELOG.md -w -r 0"
        }
      }
      
      npm run changelog
      ```

      

   