---
title: iTerm2
date: 2020-11-09 20:41:41
tags: iTerm2主题配置
categories: 知识
---

1. 安装iTerm2

   ```
   brew tap caskroom/cask
   brew cask install iterm2
   ```

2. 终端背景配色

   默认配色为xterm-256color（共有七种），位置在 iTerm2 -> Preferences -> Profiles -> Terminal，应该满足不了爱美的你。我们补充一下配色

   ```
   mkdir ~/.iterm2 && cd ~/.iterm2
   git clone https://github.com/mbadolato/iTerm2-Color-Schemes
   ```

   ```
    ~/.iterm2/iTerm2-Color-Schemes $ ls -la   //查看~/.iterm2的目录结构
   ```

   导入配色iTerm2-Color-Schemes目录下的配色，全部选中导入

3. 安装字体 字体链接: https://github.com/ryanoasis/nerd-fonts

   这里说俩种方式,这俩中我分别在一款15寸mac和16寸mac试了一下

   1. 这种方式是丫的真的慢。半个小时的样子吧！

      ```
      brew tap homebrew/cask-fonts
      brew cask install font-hack-nerd-font
      ```

   2. 我通过这种方式安装

      ```
      cd ~/Library/Fonts && curl -fLo "Droid Sans Mono for Powerline Nerd Font Complete.otf" https://github.com/ryanoasis/nerd-fonts/raw/master/patched-fonts/DroidSansMono/complete/Droid%20Sans%20Mono%20Nerd%20Font%20Complete.otf
      ```

4. 修改字体

   文件位置

   ```
   iTerm2 -> Preferences -> Profiles -> Text -> Font -> Change Font
   Text 下面勾选 Use a different font for non-ASCII text
   ```

   通过第三步中的第一种方式安装，请使用 Hack Nerd Font

   通过第三步中的第二种方式安装，请使用 

   Droid-Sans-Mono-Nerd-Font-Complete.otf

5. 安装zsh

   ```
   brew install zsh
   sudo sh -c "echo $(which zsh) >> /etc/shells"
   chsh -s $(which zsh)
   ```

6. 配置zsh 

   ```
   sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
   ```

7. 打开 ~/.zshrc

   ```
   open ~/.zshrc  //默认主题是ZSH_THEME="robbyrussell"
   ```

8. 有个颜值比较高的主题. /Users/mac/.oh-my-zsh/custom/

   ```
   git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k
   ```

9. 修改第七部中的主题

   ```
   ZSH_THEME="powerlevel9k/powerlevel9k"
   
   source ~/.zshrc（修改之后用这个命令让其生效）
   ```

10. 我这里 iTerm2 的代码配色选择的是：Dracula

11. 其他配置

    ```
    POWERLEVEL9K_MODE="nerdfont-complete"  //设置 powerlevel9k 的字体是我们前面下载的
    # Customise the Powerlevel9k prompts 
    POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(ssh dir vcs newline status) //将前面居右的几个元素放在左边了
    POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=()  //右边不放置任何元素（如果你喜欢在右边也可以加）
    POWERLEVEL9K_PROMPT_ADD_NEWLINE=true    //在每个提示之前添加换行符
    ```

12. 安装过程中遇到的问题！！

    1. git clone  速度缓慢?

       ```
       github.global.ssl.fastly.net //199.232.69.194
       github.com  //140.82.113.3
       ```

    2. sudo vim /etc/hosts

    3. hosts文件末尾添加映射

       1. 按键字母"i"进入编辑模式，添加下面俩行
       2. 按”ESC“按键退出编辑模式
       3. 输入":wq"保存并推出

       ```
       199.232.69.194 github.global-ssl.fastly.net
       140.82.113.3 github.com
       ```

    4. 保存DNS

       ```
       sudo dscacheutil -flushcache
       ```

    5. 试过之后好闲还是有点慢，丫的

       但是关闭iTerm2重新启动一下的时候好像变快了。

    6. 第四步中的意思

       ```
       dscacheutil -q host -a name github.com //检测网址的iP地址
       ```

       ```
       sudo dscacheutil -flushcache  //网址https://ss64.com/osx/dscacheutil.html：一堆英文巴拉巴拉
       Flush the entire cache. This should only be used in extreme cases.Validation information is used within the cache along with other techniques to ensure the OS has valid information available to it.
       翻译：刷新整个缓存，这个命令被用于极端的情况，验证信息与其他信息一起在缓存中使用，确保操作系统具有有效信息的技术可用。
       ```

    7. 参考：https://blog.biezhi.me/2018/11/build-a-beautiful-mac-terminal-environment.html





