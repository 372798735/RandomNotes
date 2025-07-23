## 一、git分布式版本控制

+ 了解Git基本概念
+ 能够概述git工作流程
+ 能够使用Git常用命令
+ 熟悉Git代码托管服务
+ 能够使用idea操作git

## 二、版本控制的了解

###### 2.1、集中式和分布式

1. 集中版本控制工具:
集中式版本控制工具，版本库是集中存放在服务器的，team里每个人work时从中央服务器下载代码，是必须联网才能工作，局域网或互联网。个人修改后然后提交到中央版本库。

2. 分布式版本控制工具:
分布式版本控制系统没有“中央服务器”，每个人的电脑上都是一个完整的版本库，这样工作的时候，无需要联网了，因为版本库就在自己的电脑上。多人协作只需要将各自的修改推送给对方，就能互相看到对方的修改了。举例：git

###### 2.2、git的由来历史

![](https://cdn.nlark.com/yuque/0/2023/png/2488285/1699262965541-4ce30129-e6ec-489f-9f64-72caef35ed80.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2488285/1699264500482-07fdd25e-0ed3-4e93-8be9-b8222cb6fee2.png)

###### 2.3 、git工作机制

最后一个： git push    远程库

![](https://cdn.nlark.com/yuque/0/2023/png/2488285/1699265000938-57c9be8c-c5ee-4ea3-b03a-e22d24163a88.png)

###### 2.4 、Git和代码托管中心

代码托管中心是基于网络服务器的远程代码仓库，一般我们简单成为 远程库。

+ 局域网： GitLab (一般用于公司内部，不对外开放的代码)
+ 互联网：GitHub(外网) 、Gitee 码云（国内网站）

## 三、Git常用命令

###### 1、设置用户签名（用户名和邮箱）

```typescript
git config --global user.name   "your name"
git config --global user.email   "email"
git config --global --list   // 查看所有已有配置
/*
说明：签名的作用是区分不同操作者的身份，用户的签名信息在每一个版本的提交信息中能够看到，以此
确认本次提交是谁做的，Git首次安装必须设置一下用户签名，否则无法提交代码。，这里设置用户签名和
将来登录的 GitHub（或其它代码托管中心）的账号没有任何关系。

确认是否设置成功：C:\Users\Administrator  下的 .gitconfig 文件查看
*/
```

在Windows设置里面搜索：凭据管理器  就能搜索到已经绑定的管理账号

###### 2、添加暂存区、状态查看

```typescript
git status  // 状态查看
git add  文件名 // 添加文件到暂存区
git add .  // 添加所有文件到暂存区
git rm --cached 文件名 // 移除暂存区文件，恢复到工作区
```

###### 3、提交本地库

```typescript
git commit -m "提交描述内容" // 提交所有文件到本地库
git commit -m "提交描述内容" 文件名 // 提交单个文件到本地库后的信息如下
/**
[master (root-commit) 74295c4] 鏃ュ織淇℃伅   --有分支、前七位版本号、提交信息
 1 file changed, 1 insertion(+)              -- 表示一个文件被修改，插入一行代码
 create mode 100644 hello.txt                -- 创建可以一个文件
*/
git status  // 提交本地库后打下该命令显示如下
/**
  On branch master                            -- 表示当前在哪个分支
  nothing to commit, working tree clean       -- 表示没有可提交的，工作树是干净的
*/
git reflog  // 精简的git提交信息
/**
74295c4 (HEAD -> master) HEAD@{0}: commit (initial): 日志信息  --版本号 头指针指向分支master 提交信息
*/
git  log  // 详细的git提交信息
/**
commit 74295c474b0e8f94c0082c726cb8872e60d62c00 (HEAD -> master)
Author: 刘少林 <18819270610@163.com>
Date:   Tue Nov 7 21:55:50 2023 +0800

    日志信息

*/


// 对文件修改后重复上述操作后 
git commit -m "second commit" hello.txt
/**
[master bca266e] second commit                   --有分支、前七位版本号、提交信息
 1 file changed, 1 insertion(+), 1 deletion(-)   --一个文件改变 增加一行，删除一行表示删除改行重新增加修改的
*/
git reflog 
/**
bca266e (HEAD -> master) HEAD@{0}: commit: second commit  --第二次提交本地库 指针指向这个
74295c4 HEAD@{1}: commit (initial): 日志信息               -- 第一次提交
*/
```

提交的信息类型type :

![](https://cdn.nlark.com/yuque/0/2023/png/2488285/1700797976513-cd550d49-1b5f-4810-a4ba-3120d11ec999.png)

###### 4、版本穿梭

原理：Git版本切换，底层其实是移动HEAD指针。

```typescript
git reset --hard bca266e  // 继承上面提交 切换版本到第二版本
git reflog
/**
bca266e (HEAD -> master) HEAD@{0}: reset: moving to bca266e   --表示当前版本移动到第二个版本
f871467 HEAD@{1}: commit: third commit                        --表示第三个版本
bca266e (HEAD -> master) HEAD@{2}: commit: second commit      --表示第二个版本 并且当前头部指针指向的分支master分当前版本
74295c4 HEAD@{3}: commit (initial): 日志信息                   --表示第一个版本
*/
```

###### 5、创建/合并/冲突合并 分支

什么是分支：在版本控制过程中，同时推进多个任务，为每个任务，我们就可以创建每个任务的单独分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开来，开发自己分支的时候，不会影响主分支的运行。对于初学者而言，分支可以简单理解为副本，一个分支就是一个单独的副本。（分支底层其实也是指针的引用）

```typescript
git branch -v // 查看当前项目的所有分支
git branch 分支名称 // 在当前分支创建分支
git checkout 分支名称 // 切换分支 切换分支后会继承之前创建分支基础上的提交信息
git checkout -f 分支名称 // 强制切换分支
git merge  分支名称 // 当前分支合并其他分支的内容

// 注意 合并代码如果子包有冲突使用当前更改而不是丢弃更改，这样合并的话提交记录合并不过来

/**
合并冲突报错：
Auto-merging hello.txt                                        
CONFLICT (content): Merge conflict in hello.txt                   --合并冲突
Automatic merge failed; fix conflicts and then commit the result.  --自动合并失败，修复冲突后提交

打开冲突文件如下： 合并的时候现解决冲突的代码然后删除特殊符号
*/
hellow world！33333
<<<<<<< HEAD               // 这个表示指针头部到下面的分隔符等号表示当前分支的修改
hellow world！
hellow world！master test
=======                    // 两个分支修改内容冲突的分隔符        
hellow world！hot-fix test
hellow world！
>>>>>>> hot-fix            // 合并过来的分支到上面等号表示合并过来的分支的修改
hellow world！

// 解决冲突后继续提交 注意commit 时候不用写提交文件
git add 文件名
git commit -m 提交信息

```

###### 6、Git团队协作机制

团队内协作

![](https://cdn.nlark.com/yuque/0/2023/png/2488285/1699539357995-20bee88b-6c5b-4f0f-970e-df3109ceb4c3.png)

跨团队协作

![](https://cdn.nlark.com/yuque/0/2023/png/2488285/1699539380132-c7edd0f5-a127-4c50-978e-75731fcee9d9.png)

fork别人代码提交给别人需要 发送拉取请求

![](https://cdn.nlark.com/yuque/0/2023/png/2488285/1699711531614-3bb0b29d-64b4-45f3-8473-46aad7baa0ce.png)

###### 7、远程地址起别名

```typescript
git remote -v  // 查看别名
git remote add 别名  远程仓库地址  // 创建别名
/*创建别名后 用 git remote -v 输出信息如下*/  
git-demo        https://github.com/372798735/git-demo.git (fetch)  // 表示根据别名可以拉取
git-demo        https://github.com/372798735/git-demo.git (push)   // 表示根据别名可以推送
```

###### 8、推送本地仓库到远程库、拉取远程库、克隆远程库

查看本电脑是否与远程git做关联查看电脑里面的 凭据管理器

```typescript
git remote add origin 远程库链接仓库地址 // 关联远程仓库
git push 远程库链接仓库地址 master // 此时如果本地还没有和远程库建立链接那么会弹出登录框
git pull 远程库链接仓库地址  master  // 拉取远程库地址
git clone 远程库链接仓库地址  // 克隆远程库代码 
/**
当执行 git clone 克隆远程库自动帮你做了一下三件事
1、拉取代码
2、初始化本地仓库
3、创建别名
*/
```

###### 9、ssh免密登录

```typescript
ssh-keygen -t rsa -C git的账号名称 // 输入完成后连续敲击三次回车
/*
会在C盘的 C:\Users\Administrator\.ssh  生成公钥和私钥两个文件，打开
cat id_rsa.pub 公钥文件复制密码并添加到github里面 
https://github.com/settings/keys
然后就可以像https链接对git远程仓库操作
*/
```

###### 10、git submodule 子包

```typescript
git submodule add 远程库仓库链接地址  // 项目新增子包
git submodule status // 查看子包状态
git submodule update --remote // 如果刚拉下来的项目有子包时更新下载子包的内容
git submodule update 子包名称  // 拉取子包的内容
```

###### 11、删除本地分支和远程分支

```typescript
 git checkout dev_20180927 // 1 先切换到别的分支:

 git branch -d dev_20181018 // 2 删除本地分支

 git branch -D dev_20181018 // 3 如果删除不了可以强制删除

 git push origin --delete dev_20181018 // 删除远程分支

```

###### 12、线上版本回退

```javascript
1、回退版本号
git reset --hard 25acd2b2  --要回退到的版本号
2、查看已经是否回退到指定版本号 
git log/reflog
3、推送到远程库即可
git push -f -u origin master  // 其中 master 为需要推送的分支名称(强制推送失败)
// 强制推送失败的解决办法
https://blog.csdn.net/summerfor2015/article/details/106620935

合并代码冲突完成后注意使用 commit 提交再 push 不然会有问题

```

###### 13、git大小写问题

```typescript
git config core.ignorecase false   // git默认忽略大小写，这样写就是让git不忽略大小写
```

###### 14、Git-将某次commit从一个分支转移到另一个分支

 [https://www.coonote.com/git-note/git-submits-code-for-a-certain-branch.html](https://www.coonote.com/git-note/git-submits-code-for-a-certain-branch.html)

```javascript
git cherry-pick 94070bf919891e587351a78bdb2f53e50fecb36a // 要合并的记录
```

###### 15、<font style="color:#000000;">清除克隆项目中的git历史记录，以新项目提交的自己的git仓库</font>

```typescript
// 步骤一：清除历史记录
rm -r .git
// 步骤二：初始化为自己的git仓库
git init 
// 步骤三：将全部文件提交到暂存区,注意： ‘.’ 符号前面需要空格
git add .
// 步骤四：将全部文件提交到本地仓库
git commit -m 'first commit'
// 步骤五：关联到自己的远程项目仓库,这时可能弹出需要登录自己git账号的弹窗
// -u：这个参数是 --set-upstream 的简写形式。它将远程分支设置为本地分支的上游（即关联分支）。
// 这意味着 Git 会记住本地分支和远程分支之间的关联，
// 下次你再执行推送或拉取操作时就不需要再指定上游分支了
git remote add origin '远程项目地址'
// 步骤六：提交项目到远程 或者推送并创建 master 分支
git push -u origin 'master'// git push --set-upstream origin master
```

###### 16、代码提交忽略husky校验

```javascript
 git commit -m "feat: 反馈问题优化" --no-verify
```

###### 17、修改已经提交的 commit 信息

```javascript
git cherry-pick 94070bf919891e587351a78bdb2f53e50fecb36a  // 注意切换后直接push就行

// 提交后记得切回之前的分支删除之前的 commit

```

###### 18、修改远程branch分支名称

```javascript
// 1、确保当前不在要修改的分支上，在进行分支重命名之前，确保你不在要修改名称的分支上。
git checkout 其他分支名
// 2、重命名分支
git branch -m 旧分支名 新分支名
// 3、 推送重命名后的分支
git push origin 新分支名
// 4、旧的分支还存在所以需要删除远程仓库中旧的分支
git push origin --delete 旧分支名
```

###### 19、查看所有远程分支

```javascript
git branch -r
```

###### 20、丢弃冲突文件

```javascript
git reset -- new-energy-base
```

###### 21、重置温蒂分支到远程仓库相同状态，覆盖本地的更改（<font style="color:#DF2A3F;">注意：本地有用的更改记得备份</font>）

```javascript
git reset  --hard origin/master  // master 为覆盖到本地的分支
```

###### 22、撤销合并

```javascript
1、撤销合并但尚未提交(未推送)
git merge --abort
git reset --merge （git版本需要 2.0及以上）

2、撤销合并并且已经提交但未推送
git reset --hard HEAD~1  // HEAD 移动到合并前的提交
```

###### 23、多人合作如果本地有commit 在 git pull 时远程有文件更改就会提示合并，如果不是代码冲突的合并，为了减少git提交产生冗余的信息可以忽略这个合并提交，方法参考改文章

[https://gitcode.csdn.net/65ea8a4f1a836825ed794712.html?dp_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6OTYwNjgyLCJleHAiOjE3MzY0OTU4OTAsImlhdCI6MTczNTg5MTA5MCwidXNlcm5hbWUiOiJ3ZWl4aW5fNDU1MzE5NDUifQ.4z2EJsZ_1tMX35aH3hVH4soQuAH2QSrg3Nb7iBwBj0I&spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Eactivity-1-103841807-blog-129175346.235%5Ev43%5Econtrol&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Eactivity-1-103841807-blog-129175346.235%5Ev43%5Econtrol](https://gitcode.csdn.net/65ea8a4f1a836825ed794712.html?dp_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6OTYwNjgyLCJleHAiOjE3MzY0OTU4OTAsImlhdCI6MTczNTg5MTA5MCwidXNlcm5hbWUiOiJ3ZWl4aW5fNDU1MzE5NDUifQ.4z2EJsZ_1tMX35aH3hVH4soQuAH2QSrg3Nb7iBwBj0I&spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Eactivity-1-103841807-blog-129175346.235%5Ev43%5Econtrol&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Eactivity-1-103841807-blog-129175346.235%5Ev43%5Econtrol)

24、取消变基

```javascript
git rebase  --abort
```

###### 25、暂存本地修改的代码

```javascript
/** 
  暂存所有未提交的修改，包括新增文件 
  -u：这是 --inlcude-untracked 的简写，通常情况下， git stash 只会保存已跟踪文件
  （即已经被 Git 管理的文件）的修改，而使用 -u 选项后，会将未跟踪的文件
  （即新创建但还未被 git add 的文件）也一并保存到 stash 中。
  -m: "临时保存修改" ：这是 --message 的简写，用于为本次 stash 操作添加一条描述信息。
  在这个例子中，描述信息是 "临时保存修改"，方便后续查看 stash 列表时了解每个 stash 的内容。
**/
git stash push -u -m "临时保存修改"
/** 恢复最近一次暂存的修改，并从栈中移除该记录 */
git stash pop
/** 恢复最近一次暂存的修改，但不从栈中移除记录 */
git stash apply
/**  查看所有暂存记录 */
git stash list
/** 恢复指定编号的暂存记录，例如恢复编号为 1 的记录 */
git stash pop stash@{1}
/** 删除编号为0的stash记录 */
git stash drop stash@{0}
/** 删除最新的stash记录 */
git stash drop
/**  删除所有stash记录  */
git stash clear
```

## 参考文件

阮一峰大佬的git著作：[https://www.bookstack.cn/read/git-tutorial/README.md](https://www.bookstack.cn/read/git-tutorial/README.md)
