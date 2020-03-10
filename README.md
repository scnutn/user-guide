# 华师塔内PC协会 github 账号使用指南-0.1.0

<!--
名称：华师塔内PC协会 github 账号使用手册-0.1.0
版本：0.1.0
创建时间：2020-03-11 00:07
最后编辑：2020-03-11 00:16
描述：此文档为塔内 PC 协会 Github 账号的使用指南，阐述使用本账号使用教程，希望帮助协会
成员快速了解本账号的使用方式
-->

## 一、账号目的和意义
Git 是高效的 文件、项目 管理工具 和 团队协作工具  

建立此账号主要有如下目的：
1. 有条理地管理协会文件、资料，以促进各成员之间的资料信息有效同步
2. 有效地对协会所进行的各个项目（如 活动项目组工作、“塔内PC”小程序 等）进行有效的协作
3. 促进协会各成员共同参与协会项目，打破原有的部门间、项目组内外合作的壁垒
4. 促进协会的工作流程和信息公开透明，促进协会工作规范化

此账号的建立有如下意义：
1. 有助于协会各项资料有效、长期保存
2. 有助于更方便地管理各种资料文件
3. 提高部门间合作的效率，对于长期项目尤其如此
4. 将协会的各个项目集中，方便管理
5. 使协会的程序项目更易于使用现代化的协作开发方式
6. 有助于降低跨部门，跨项目组的合作成本
7. 可追踪的历史纪录及可控的权限设置有助于协会工作规范化
8. github 提供的工具有助于更方便地发布信息

## 二、使用方式
此处使用方式以使用 终端版本的 git 客户端 为例进行讲解，GUI 版本的 git 客户端 操作与终
端版本类似
#### 第一步：在本地生成 rsa 密钥
```
# 生成新的密钥是必须的，因为 Github 对于一个密钥只允许在一个账户中添加
# 用自己的邮箱替换以下的 <:--your-email--:>，后面类似
# 如果没有配置过用户信息，需要先配置用户信息（配置过则忽略）
# ​git config --global user.name <:--your-name--:>
# ​git config --global user.email <:--your-email--:>
# 用户名不必与某一个 github 账号一致，但邮箱需要（不是这个账号的邮箱，是自己账号的邮箱）
ssh-keygen -t rsa -C <:--your-email--:>
```
这一步会提示输入密钥储存的位置，如果此前曾生成过密钥或不想将此账号作为主要的账号，则应该
手动指定一个位置，如`./.ssh/id_rsa`，完成后生成的密钥会储存在这一步输入的位置（提示中
的括号是默认路径，如果没有输入会使用它）；密码可有可无，一般无密码即可
#### 第二步：将公钥添加到账号
首先需要复制生成的公钥，打开上一步生成密钥的位置，文件夹中带有`.pub`后缀的是公钥，另外
一个是私钥，用文本编辑器（记事本也可以）打开公钥文件`id_rsa.pub`，将里面的内容全部复制，
然后进入 github，登陆本账号，在账号设置（点右上角头像）的`SSH and GPG keys`中点击
`New SSH key`，将复制的公钥粘贴进`Key`输入框内，并指定一个名字，按照
`<:--职务--:>-<:--姓名--:>`的格式命名，点击确认后即可添加公钥
#### 第三步：从本地访问远程仓库
以克隆仓库为例，分为两种情况：
##### 1 -> 本地只有一个密钥，在默认位置
那么直接使用 git 命令即可，ssh 会在默认位置查找密钥
```
git clone <:--repo--:>  # <:--repo--:> 是在仓库主页上复制的 SSH 链接
```
##### 2 -> 本地原来有一个密钥，新生成的密钥不是默认密钥
> 如果使用的是 Windows 操作系统，以下命令只能在 Git Bash 中执行，而不能在命令提示符中
执行  

此时最简单并且方便的方法是使用 ssh 密钥代理，用以下命令启动代理：
```
eval $(ssh-agent -s)
```
再将用于这个账号的密钥提交给代理进行托管：
```
ssh-add <:--path-to-private-key--:>
# e.g. ssh-add ./.ssh/id_rsa
# 注意此时添加的是私钥
```
这时就可以像第一种情况一样直接使用 git 命令了
```
git clone <:--repo--:>  # <:--repo--:> 是在仓库主页上复制的 SSH 链接
```
如果想要关闭代理，可以使用
```
ssh-agent -k
```
该命令会结束当前代理服务的进程，下次再进入时需要重新开启代理服务  
一般情况下，如果不是有超过两个账号，代理服务是可以不关闭的，如果没有结束代理服务就关闭
终端，代理服务会留在后台继续运行，但再次打开终端时不会自动连接上次的代理服务  
为了能够在重新打开控制台时直接使用之前启动的代理服务，可以借助`ssh-connect`这一工具  
`ssh-connect`是一个 python 程序，首先安装`ssh-connect`：
```
pip install ssh-connect
```
安装后在 Git 终端中就可以使用
```
eval $(python -m ssh-connect)
# 语法与启动代理的语法相近
```
来连接正在运行的代理服务了  
这条命令比较麻烦，可以通过设置命令别名来简化，打开 shell 的配置文件，如果使用类 unix 
系统，它应该在`~/.bashrc`（为当前用户设置）或`/etc/bashrc`（为所有用户设置）；如果在 
Windows 下使用 Git bash，它应该在`/etc/bash.bashrc`（根目录为 Git bash 的安装位置），
在这个文件中添加一行
```
alias ssh-connect='python -m ssh-connect'
```
再重新打开终端，就可以用
```
eval $(ssh-connect)
```
这一较简洁的命令了
> ssh 代理也可以用于私钥设置了密码，但是不想每次都输入密码的情景，只有启动代理时才需要
输入密码，代理启动后只要连接了代理都不需要重新输入密码  

> `ssh-connect`对于同时在后台运行两个或以上的代理服务的优先级是不确定的，目前尚不支持
多代理切换，未来可能考虑添加这一特性，目前如果有需要，可以使用`keychain`