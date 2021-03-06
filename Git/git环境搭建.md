### 写在上班第二天

上班两天，感受到充满极客的味道，虽然对周围的同事不是特别的熟悉，但上班第一天的时候还是能感受到各位同事的热情欢迎。废话不多说，在这里记录一下上班这两天的主要工作。

#### 搭建前端开发环境

刚来的第一天，主要围绕前端开发环境的搭建，主要安装的一下环境依赖有：

- Node.js
- npm(node自带，不过用的是淘宝镜像，当然得安装cnpm，这个大家都懂)
- gulp(全局安装：cnpm install gulp -g)
- grunt(公司以前配置好的自动化构建工具，现在还在用，所以自己还是得装上)
- git(这个是必须的，第一次用git的时候我们需要做一些用户信息的配置：
要配置的是你个人的用户名称和电子邮件地址。这两条配置很重要，每次 Git 提交时都会引用这两条信息，说明是谁提交了更新，所以会随更新内容一起被永久纳入历史记录：  
`$ git config --global user.name "John Doe"`
`$ git config --global user.email johndoe@example.com`)
- gitlab(公司用的代码仓库，如果同一部电脑有用到github和github的话，需要在git上作一些相应的配置)
生产两个SSH-Key，一个用于公司gitlab的，一个用于自己github上的。
1. 生成一个github用的SSH-Key
`$ ssh-keygen -t rsa -C "youremail@your.com" -f ~/.ssh/id_rsa`

2. 生成一个gitlab用的SSH-Key
`$ ssh-keygen -t rsa -C "youremail@yourcompany.com" -f ~/.ssh/gitlab_rsa`  
在`~/.ssh/`目录会生成github-rsa私钥和github-rsa.pub和公钥。 我们将github-rsa.pub中的内容粘帖到github / gitlab服务器的SSH-key的配置中。

3. 在 ~/.ssh 目录下新建一个config文件
`touch config`
添加以下内容：

``` sh
# gitlab
Host gitlab.com (这里需要替换成公司的gitlab域名，如：code@xxxx.com, 也可以 *gitlab.com)
  Port 59898 (配置端口)
  HostName gitlab.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/gitlab_rsa
# github
Host github.com
  HostName github.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/github_rsa
```

4. 测试
`$ ssh -T git@github.com`
输出：Hi Yusingz! You've successfully authenticated, but GitHub does not provide shell access.
表示成功连接上github了，连接gitlab也是一样的，你可以在gitlab上新建一个test测试项目，然后自己测试看看是否成功。
 
### 编辑器（推荐使用VS Code）
##### VS Code

1. One Dark Pro主题(目前一直在用的主题🌟)
2. GitLens 🌟
3. ESlint 🌟
3. Prettier Code formatter(格式化工具🌟)
4. Pug(Jade) snippets
5. Vetur(vue代码高亮🌟)
6. Bookmarks(Mark lines and jump to them 快捷键切换: cmd + alt + k 🌟)
7. Vscode-fileheader(生成文件头注释 自动更新修改时间🌟)

#### 使用Fiddlers或者Charles抓包工具
测试环境下，使用抓包工具，截取网络资源，使用本地修改的静态资源替换  
https协议需要安装证书
[Charles从入门到精通](https://blog.devtang.com/2015/11/14/charles-introduction/)

#### windows下使用Fiddlers
#### mac下使用Charles

#### SwitchyOmega代理工具
配置选项

### 使用Mac有关的命令操作
#### 显示隐藏文件

- 在Finder下使用 Command + Shift + . 可以显示隐藏文件、文件夹，再按一次，恢复隐藏；  
finder下使用Command + Shift + G 可以前往任何文件夹，包括隐藏文件夹。

- 在终端下显示全部文件
defaults write com.apple.finder AppleShowAllFiles -bool true
osascript -e 'tell application "Finder" to quit'

- 不显示全部文件  
defaults write com.apple.finder AppleShowAllFiles -bool false
osascript -e 'tell application "Finder" to quit'

- 剪切快捷键
Command + C 复制，然后使用 Command + Alt + V 剪切到相应目录

### Centos 6给Jenkins使用root权限执行脚本
1.将jenkins账号分别加入到root组中

`gpasswd -a root jenkins`

2.修改/etc/sysconfig/jenkins文件中，

\# user id to be invoked as (otherwise will run as root; not wise!)
JENKINS_USER=root
JENKINS_GROUP=root

可以修改为root权限运行

3.重启Jenkins
`service Jenkins restart`

4.验证 
在Jenkins中的shell脚本中执行命令
`whoami`

### SSH终端连接工具
1. 使用Finalshell  
- Mac一键安装脚本  
- curl -o finalshell_install.sh www.hostbuf.com/downloads/finalshell_install.sh  
- chmod +x finalshell_install.sh  
- 使用sudo ./finalshell_install.sh启动服务  
2. 使用Xshell

### MySQL 数据库
#### Workbench 常用快捷键
- 新建tab(new tab) ctrl+t
- 执行当前语句(execute current statement) ctrl+enter
- 执行全部或选中的语句(execute all or selection) ctrl+shift+enter
- 查看执行计划(explain current statement) ctrl+alt+x
- 注释 --加空格，如 –- select * from table 或者直接在执行语句前面加`#`加空格即可


### Git 仓库大小写敏感问题

Q: windows下修改文件夹 tools => Tools 实际上并没有在线上仓库中体现

所有我们进行以下操作：
`git config core.ignorecase false` // 保持大小写敏感
`git rm --cached src/components/tools` // 移除小写文件夹目录
`git config core.ignorecase true` // 再恢复大小写不敏感 