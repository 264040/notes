```objc
1.git init   //初始化本地git仓库
2.git add <file>  //添加文件
3.git status    //查看状态
4.git commit    //提交
5.git push      //推送到仓库
6.git pull     //从远程仓库拉取数据
7.git clone http...   //从远程仓库拷贝数据  不指定分支
8.git clone -b master https://github.com/264040/czhczh.git // 指定分支拷贝
```





# 1. 初始化一个空的git本地仓库

```
git init
```

# 2. 声明你的身份

```
git config --global user.name "fantj"
git config --global user.email "8440xxx@qq.com" 

#删除身份
git config --global --edit
```

# 3. 声明你的远程仓库路径

```
git remote add origin https://gitee.com/xxx/xxx.git (你的远程项目地址) 
```

##### 查看远程仓库地址

`git remote -v` 应该要显示出你的远程仓库地址，如果不是对应的地址。先删除后添加。如果是正确的则跳过下面的代码。

```
如果结果是正确的则跳过下面的代码。
git remote rm origin
git remote add origin xxxxx.git 
```

##### 查看全局配置信息

```
git config --global --list

user.email=84407xxx@qq.com
user.name=fantj 
```

# 4. 检测是否成功连接上远程仓库

执行`git fetch`

```
E:\workspace\go-xxx>git fetch
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From https://gitee.com/xxxx/go-xxxx
 * [new branch]      master     -> origin/master 
```

**注意**： 如果出现上述，证明成功连接到远程仓库了，没有出现也没关系，证明你本地没有你的gitee账户信息，随便打个命令`git clone http://gitee.xxxx.git`或者`git pull origin master`就会让你输入密码，注意尽量一次性输正确，否则需要去win10 账户下修改(`控制面板->用户账户->管理凭据->寻找修改你的gitee密码`)。

# 5. 拉取远程仓库

```
git pull origin master 
From https://gitee.com/xxx/go-xxxx
 * branch            master     -> FETCH_HEAD
```

如果这步有错请检查你的gitee密码是否正确。

# 6. 准备上传工作

```
git add .
git commit -m "first commit"
git push origin master
 
E:\workspace\go-xxx>git push origin master
Enumerating objects: 83, done.
Counting objects: 100% (83/83), done.
Delta compression using up to 4 threads
Compressing objects: 100% (75/75), done.
Writing objects: 100% (82/82), 4.10 MiB | 1.73 MiB/s, done.
Total 82 (delta 6), reused 0 (delta 0)
remote: Powered by Gitee.com
 
```

出现上面则为上传成功。 否则就就是没添加ssh

添加公钥(ssh)：https://juejin.cn/post/7029992163555409927?share_token=0b0bc4a8-be88-4188-9438-a24ab3b1897b

```
ssh-keygen -t rsa -C "邮箱"
cat ssh-key.pub

```

# git add 命令

[![Git 基本操作](https://www.runoob.com/images/up.gif)Git 基本操作](https://www.runoob.com/git/git-basic-operations.html)

------

**git add** 命令可将该文件添加到暂存区。

添加一个或多个文件到暂存区：

```
git add [file1] [file2] ...
```

添加指定目录到暂存区，包括子目录：

```
git add [dir]
```

添加当前目录下的所有文件到暂存区：

```
git add .
```

以下实例我们添加两个文件：

```
$ touch README                # 创建文件
$ touch hello.php             # 创建文件
$ ls
README        hello.php
$ git status -s
?? README
?? hello.php
$  
```

# git上传github

```js
vi log.txt // 编辑log.txt
`cd .>index.html || type nul> name.txt //cmd  || touch index.html  //git（linux）   // 三种种创建文件的方式`
git init  //初始化本地仓库
git config --global user.name "beilaing"   // 配置用户名
git config --global user.email "2640402754@qq.com"   // 配置email
git config --global --list   // 查看配置
git config --global --edit   // 修改配置 修改完成后  按 esc 输入 :wq 即可保存退出
git add index.html   // 上传文件到队列中
git status / status -s  // 查看队列文件
// 已在队列的文件在本地被修改请再上传一次队列
git rm --cached index.html  // 删除队列文件
git commit -m "updata file" // 添加本地仓库

git branch login  // 创建分支
git checkout login  // 切换到login分支
git master login  // 合并分支 前提在master分支下合并
git remote add origin http:\\github.com...  //添加远程仓库
git remote -v   // 查看添加的仓库
git push -u origin master  // 上传到远程仓库 上传不了就用敲一下这个再上传git config --global  http.sslVerify "false"



// 基本流程
git init 
git config --global user.name "name"
git config --global user.email "2640402754@qq.com"
git remote add origin http:\\github.com...
git add .
git commit -m "描述"
git push -u origin master


在.gitgnore文件添加不需要提交的文件名



```

