# Git的使用
---

## 实验目的
1. 学习 Git 软件的安装
2. 学习 Git 的简单使用，增加文件，提交，创建分支，增加远程，下拉、上推，合并
3. 学习 Git 的冲突处理 

## 实验平台
- Window 10

## 实验前准备
1. 到Git官网下载[64-bit for Windows](https://github.com/git-for-windows/git/releases/download/v2.26.2.windows.1/Git-2.26.2-64-bit.exe)版本的Git版本控制工具。
2. 翻阅相关资料，掌握分布式版本控制系统 Git 的基础知识。
3. 查找 Git 的使用教程，熟悉 Git 命令以及 Git GUI 的使用。

## 实验内容

### Git的安装

#### 阅读软件许可
![阅读软件许可](imgs/git-download1.jpg)  

阅读软件许可并点击 next 进行下一步安装。

#### 选择安装的组件
![选择安装的组件](imgs/git-download2.jpg)

根据喜好选择：
* 是否添加桌面快捷键图标
* 是否在右键菜单中添加"Git Bash Here"和"Git GUI Here"快速启动
* 将.git*配置文件与默认文本编辑器关联
* 将Bash运行的.sh文件关联

#### 是否创建开始菜单
![是否创建开始菜单](imgs/git-download3.jpg)

#### 选择默认编辑方式
![选择默认编辑方式](imgs/git-download4.jpg)

这里选择Vim作为默认编辑器。

#### 设置路径环境变量
![设置路径环境变量](imgs/git-download5.jpg)

勾选第二项，将为git命令配置全局环境变量，可以在Windows的cmd模式和Git自带命令行模式使用。

#### 选择HTTP远程连接方式
![选择HTTP远程连接方式](imgs/git-download6.jpg)

使用OpenSSL库，而不使用Windows安全通道库，不会产生兼容性问题。

#### 配置换行格式
![配置换行格式](imgs/git-download7.jpg)

#### 使用Git默认终端
![使用Git默认终端](imgs/git-download8.jpg)

选择默认的MinTTY终端。

#### 配置额外选项
![配置额外选项](imgs/git-download9.jpg)

* 启用缓存，在Git命令行模式可以通过键盘上下键切换出先前使用git命令，提高效率。
* 使用Git为Windows提供的默认管理器。

#### 等待安装
![等待安装](imgs/git-download10.jpg)

等待安装完后，可以在选择立即运行Git Bash和查看当前发行版的版本说明。

### Git的使用

在Git使用前可以建立这样的概念：git commit命令通知Git帮你保存一份当前项目的快照，Git将为项目中的每一个文件的当前状态保存当前的“记录”，相当于保存了当前项目的版本。Git命令中绝大部分的操作都会围绕这份“记录”展开。但首先，我们需要有一个Git仓库。

#### 构建本地仓库

在项目目录中右键Git GUI，可以看到当前的Git GUI界面为：

![create1](imgs/git-create1.jpg)

Create New Registory并选择当前项目的文件夹，将当前目录构建为一个Git仓库，界面扩展为：

![create2](imgs/git-create2.jpg)

项目目录下将会出现一个后缀为.git的隐藏文件，代表你已经成功。

以上操作在Git Bash命令行模式下对应命令为
<code>git init</code>

#### 克隆已存在的仓库

如果想复制已存在的Git仓库，以仓库的地址url为参数执行
<code>git clone [url]</code>
命令，等待片刻将成功克隆。

#### 提交

当你处于GUI界面，不用急着提交你的初版，点开Repository-Visualize master's History查看master分支的历史会出现如下错误：

![error1](imgs/git-error1.jpg)

这是因为当前的主分支master下没有任何提交过。接下需要先点开Edit-Options确认你是否填写了你个人的User Name和Email Address，左侧为本地设置，右侧为全局设置。

![error2](imgs/git-use1.jpg)

除非你填写了本地或者全局的User Name和Email Address，不然当你增加需要跟踪的文件时会出现如下错误：

![error3](imgs/git-error2.jpg)

当往目录下添加了一个test.txt的文件，在GUI实用快捷键F5刷新界面，左上框显示Unstaged Changes，这是因为目录新增了文件而还没跟踪。
快捷键Ctrl+I添加对左上框所有文件的跟踪，快捷键Ctrl+Enter提交，发现如下错误：

![error4](imgs/git-error3.jpg)

这是因为我们没有填写右下框的本次提交信息，而填写的提交信息将作为本次提交对应历史节点的描述。填写提交信息First commit并Ctrl+Enter提交，再次查看master分支的历史，成功提交。

![use2](imgs/git-use2.jpg)

以上操作在Git Bash命令行模式下对应命令为  

<code>git config --global user.name ['yourname']</code>

<code>git config --global user.email [youremail]</code>

<code>git add .</code>

<code>git commit -m 'First commit'</code>

#### 取消缓存

如果使用Git Bash命令行模式，错误操作而对某个已跟踪的文件执行了git add命令即缓存了该文件，并且还未提交，使用
<code>git reset HEAD --[yourfile]</code>
命令将回退当前对该文件的缓存。

对应的Git GUI模式操作应该是在左下框找到不想缓存的该文件，点击其文件名前面的图标将其移回左上框。

#### 从缓存区移除文件

相较于取消缓存的git reset命令，
<code>git rm [yourfile]</code>
命令将把文件从缓存区和工作目录中剔除，这意味着我在项目目录下也不再能找到它。

如果只是想将文件移除缓存区不再跟踪，但希望它依然在项目目录下能找到时，可以使用更加缓和的
<code>git rm --cached [yourfile]</code>
命令。


#### 切换分支（下次更新）

每次切换分支前需要将当前分支修改的内容全部提交，否则报错。

#### 合并分支（下次更新）

合并两个不同分支时，可能会遇到各自分支的同级目录下有相同命名相同格式的文件，产生这种冲突时Git做了聪明的处理。



### 实用的Git查看型命令总结

打印当前目录的子目录列表：

<code>ls</code>

<code>ls -a</code>

查看项目中文件在工作目录与缓存的状态：

<code>git status</code>

<code>git status -s</code>

查看已写入缓存与已修改但尚未写入缓存的改动的区别：

<code>git diff</code>

<code>git diff HEAD</code>

<code>git diff --stat</code>

查看已缓存改动<code>git diff --cached</code>



## 实验总结