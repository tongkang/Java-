#### git 六行配置

```java
git config --global user.name tongkang
git config --global user.email tongkang97@gmail.com
git config --global push.default simple
git config --global core.quotepath false
git config --global core.editor "code --wait"
git config --global core.autocrlf input

--展现出刚刚所有的配置
git config --global --list

--运行code会直接打开code客户端
```



#### git就是一个版本控制

##### git  init  初始化

初始化后，有个`.git`目录，这个目录相当于容纳你的代码快照，是需要提交的代码

如果不想提交，可以创建一个`.gitignore`不需要提交的，`.gitignore`中直接添加文件名就可以

```
-- 添加文件
git add 路径（可以是绝对路径，相对路径，.和*）
add可以进行删除文件，执行命令（删除后要进行提交）
    rm 文件名
    git add 文件名   
    git add .  提交修改所有  

-- 查看状态，有更改过的文件
git status

-- 提交一个版本，相当于复制一个代码快照到`.git`目录中

git commit -m 字符串

-- 另一个和m差不多的，执行命令后打开vscode，里面会有增删的信息，直接可以输入更改的信息。保存退出，就复制到 .git 仓库了
（它能帮我回顾我刚刚改了什么东西，会迫使我们把提交理由写的更加详细）
git commit -v 
```

##### git切换各个版本

```
-- 切换版本
如果文件没有提交或是忽略，直接切换会导致文件不见
git reset --hard 版本的前六位号码（可以是6位，也可以是4位，也可以是 7位，只要是惟一的即可。）

-- 查看所有的提交版本历史
git reflog 
```



##### git的分支

```
git branch x  --建立分支x
git checkout x   --切换分支x，不影响未提交的文件
git branch   --查看所有分支，有 * 代表你在哪个分支上
git branch -d x  --删除分支，删除x分支
```

#####  git分支的合并

```
切换到一个分支上，然后执行 git merge x --把x分支合并过来
如果发生冲突
  执行git status 查看状态
  改动文件，提交文件
  最后，git commit 提交

```



#### git的远程仓库GitHub

##### 建立公钥私钥

1、`ssh-keygen -t rsa -b 4096 -C 你的邮箱`

2、进去GitHub的`setting`--->`SSH and GPG keys`--->`New SSH key`

3、执行`cat ~/.ssh/id_rsa.pub`得到公钥，复制粘贴进去

4、测试连接是否成功：`ssh -T git@github.com`，给你一个公钥就`yes`



##### 上次文件到GitHub（上传）

1、执行两条语句`git remote add origin git@xxxxxxx`
`git push -u origin master`一定要是**git**开头，也就是**SSH**连接（自己电脑上**SSH**更快，建立有密钥；别人电脑只能用**HTTPS**）。

`-u origin master`后面可以可不写，那么直接推到master分支上

**上传另一个分支到GitHub**

- 方法一

  `git push origin x:x`把x分支上传到x分支

- 方法二

  `git checkout x ` `git push -u origin x`

##### 克隆项目下来（下载）

`git clone git@?/xxx.git`：会在当前目录下创建一个xxx目录

`git clone git@?/xxx.git yyy`：会在本地新建一个yyy的目录

`git clone git@?/xxx.git .`：**最后一个字符是点，前面有空格**。这是先新建一个目录（最好是空的）



#### 把log显示的更好看点

最后 code ~/.bashrc 在文件最后加上

```
alias glog="git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit -- | less"
```



#### GitHub搭建个人博客

