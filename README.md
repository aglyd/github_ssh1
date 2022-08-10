# github_ssh1

## 常用指令 

git init 把当前所在目录变成git可以管理的仓库

git remote -v	查看当前git远程地址

git remote add origin +url	把本地仓库与远程仓库关联

git remote set-url origin +url   更改远程仓库关联

或git config -e

直接编辑更改其中origin的url保存即可



git clone [+url]	克隆项目当当前目录下，会生成一个项目名文件，克隆项目不需git init和关联远程仓库

git branch  查看所有分支，当前所在分支名会颜色高亮

git branch [+name]  创建分支

git branch -d [+name]	删除分支

git checkout [+name] 检出（切换）分支

git checkout –b name	创建并切换分支

git merge [+分支名]	将某分支合并到当前分支，注意要先切换到被合并的分支为当前分支，变化的是当前分支内容

*Fast-forward*模式：代表合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快。



git add [+filename1 filename2....]    添加文件/文件夹到版本库，可用相对路径，例如：git add ./src，会将当前路径下的src的所有文件add进去。

git status  查看有哪些本地文件改动了可commit

git commit -m “xxx说明信息”   提交文件到版本库

git push -u origin master   第一次提交（-f：参数强制提交）

git diff [+filename]  查看文件修改(本地文件和远程比较)

cat [+filename]	查看文件

pwd 	查看当前路径

git log	查看commit记录

git reflog 	获取历史版本号

git reset --hard HEAD^ 	回退上一版本

git reset --hard HEAD^^	回退上上版本

git reset --hard HEAD [+版本号]

使用指令git reset --soft HEAD^。

注意此处如果想要连着add也撤销的话，--soft改为--hard；

HEAD^  表示上一次的commit，也可以写成HEAD~1

如果撤回两次之前的，可以使用HEAD^^或者HEAD~2，以此类推



rm [+filename]	删除文件，可直接commit，或误删了用git checkout [+filename]重新检出该文件，前提是删除了还没commit

git rm --cached + [文件路径]，不删除物理文件，仅将该文件从缓存中删除；

git rm --f + [文件路径]，不仅将该文件从缓存中删除，还会将物理文件删除（不会回收到垃圾桶）。

git rm -r --cached +[目录路径]，递归删除已经add到暂存区但还没commit的目录，路径可用相对路径，如--cached ./src，删除当前路径下的src目录下所有文件。

请问 git rm --cache 和 git reset HEAD 的区别到底在哪里呢？
如果要删除文件，最好用 git rm file_name，而不应该直接在工作区直接 rm file_name。
如果一个文件已经add到暂存区，还没有 commit，此时如果不想要这个文件了，有两种方法：
1，用版本库内容清空暂存区，git reset HEAD 但要慎重使用
2，只把特定文件从暂存区删除，git rm --cached xxx

git rm -r --cached .		清空当前路径下暂存区（如果修改了.gitignore不生效时，这样会让.gitignore文件生效）

touch re.md	Linux touch命令用于修改文件或者目录的时间属性，包括存取时间和更改时间。若文件不存在，系统会建立一个新的文件。





## 分支管理策略

   通常合并分支时，git一般使用”Fast forward”模式，在这种模式下，删除分支后，会丢掉分支信息，现在我们来使用带参数 –no-ff来禁用”Fast forward”模式。首先我们来做demo演示下：

1. 创建一个dev分支。
2. 修改readme.txt内容。
3. 添加到暂存区。
4. 切换回主分支(master)。
5. 合并dev分支，使用命令 git merge –no-ff -m “注释” dev
6. 查看历史记录

git config:
git config --list   列出Git可以在该处找到的所有的设置
在git中，我们使用git config 命令用来配置git的配置文件，git配置级别主要有以下3类：
1、仓库级别 local 【优先级最高】
2、用户级别 global【优先级次之】
3、系统级别 system【优先级最低】
git config -l 查看配置
git config --local -l 查看仓库配置【必须要进入到具体的目录下，比如要查看TestGit仓库的配置信息】
git config --global -l 查看用户配置
git config --system -l 查看系统配置

git config -e 编辑配置文件:
git config --local -e 编辑仓库级别配置文件
git config --global -e 编辑用户级别配置文件
git config --system -e 编辑系统级别配置文件

git config 添加配置项目:
git config --global user.email “you@example.com”
git config --global user.name “Your Name”
如果你希望在一个特定的项目中使用不同的名称或e-mail地址，你可以在该项目中运行该命令而不要--global选项。

配置客户端长期存储用户各和密码,长期存储密码：
git config --global credential.helper store
获取帮助
git help <verb>  
git <verb> --help  
man git-<verb>  

vim编辑模式：
同Linux编辑模式
vim编辑器若处于不可编辑状态，输入字母 c 可以进入编辑状态
修改完之后按esc键退出编辑状态，再按大写ZZ就可以保存退出vim编辑器。vim操作符中说的 qw 可以保存并退出，或先按w再按q



## git推送大文件失败：使用git-lfs项目来在git上传文件；

项目网站：https://git-lfs.github.com/

使用：

打开该git根目录下的.gitattributes文件（这个文件默认是隐藏的）
在文件中添加一行：大文件名称 filter=lfs diff=lfs merge=lfs -text
保存，重新提交（原来推送失败的那一次提交作废了，必须把大文件和.gitattributes一并提交才可以推送成功）

或者：

```java
git lfs install		//You only need to run this once per user account.
git lfs track "*.pdf"
git add .gitattributes

//之后便可正常commit和push大文件了
```

----



## 在push代码时，出现“git master branch has no upstream branch”问题的原因是没有将本地的分支与远程仓库的分支进行关联。如下图所示：

具体原因： 出现这种情况主要是由于远程仓库太多，且分支较多。在默认情况下，git push时一般会上传到origin下的master分支上，然而当repository和branch过多，而又没有设置关联时，git就会产生疑问，因为它无法判断你的push目标。

Git 的 “master” 分支并不是一个特殊分支。 它就跟其它分支完全没有区别。 之所以几乎每一个仓库都有 master 分支，是因为git init命令默认创建它，并且大多数人都懒得去改动它

远程仓库名字 “origin” 与分支名字 “master” 一样，在 Git 中并没有任何特别的含义一样。origin” 是当你运行git clone时默认的远程仓库名字。 如果你运行 git clone -o booyah，那么你默认的远程分支名字将会是 booyah/master。

解决办法其实就是确定这两个值，方法有两种：

第一种如上图中的提示：**git push --set-upstream origin master**。其中的origin是你在clone远程代码时，git为你创建的指向这个远程代码库的标签，它指向repository。为了能清楚了解你要指向的repository，可以用命令git remote -v进行查看。master是你本地的branch，可以用git branch -a查看所有分支，远程分支是红色的部分。然后确定好这两个值后，将值换掉即可。

另一种方法是：**git push -u origin master**。同样根据自己的需要，替换origin【远端】和master【本地】。
两个命令的区别是第一条命令是要保证你的远程分支存在，如果不存在，也就无法进行关联。而第二条指令即使远程没有你要关联的分支，它也会自动创建一个出来，以实现关联。



---

# [git中的撤销、删除操作总结（git clean、git checkout和git reset）](https://www.csdn.net/tags/MtTaUgwsMjM2NTMtYmxvZwO0O0OO0O0O.html)

## 1. 删除未跟踪的文件-git clean

如下所示，`test.cpp`为新建文件，它属于未跟踪的文件。使用`git clean`从你的工作目录中删除所有没有tracked，没有被管理过的文件。`删除了就找不回了，一定要慎用。`
![在这里插入图片描述](README.assets/70aae26552a54b89a118cca831582203.png)
使用`git clean -h`查看参数意义：

```bash
    -q, --quiet           不打印删除文件的名称
    -n, --dry-run         演习
    -f, --force           强制
    -i, --interactive     交互式清除
    -d                    删除整个目录
    -e, --exclude <模式>  添加 <模式> 到忽略规则
    -x                    也删除忽略的文件
    -X                    只删除忽略的文件
```

这里的忽略文件指的是`gitignore`。如果担心误删，建议每次使用git clean时候，最好`加上交互式参数i，避免误删`。

## 2. 已修改，未暂存-git checkout/git reset

下列master.txt是被修改过的文件，但是还没有加入暂存区，也就是没有使用`git add`操作，此时如果不想要这个修改，可以使用`git checkout`删除此次修改，恢复到修改之前的状态。
![在这里插入图片描述](README.assets/9446f8bd60164767b93e581e9787d02b.png)
使用方式：

```bash
#方法一：删除master.txt的修改
git checkout master.txt

#方法二：删除所有修改
git checkout .

#方法三：回退
git reset --hard
```

## 3. 已暂存，未提交-git reset

下列master.txt，已经提交`git add`，但是没有`git commit`。

![在这里插入图片描述](README.assets/0c2a607e67e54032bff53bc07ee8860c.png)
此时如果想要删除此次已经提交的修改，也就是说`暂存区的内容不想要了`，或者只是想要把暂存区的内容还原至修改的状态。可以使用下列的一些命令：

```bash
#方法一：删除暂存区和修改的工作区
git reset --hard

#方法二：只是把暂存区回退至修改的状态
git reset HEAD

#方法三：只是把暂存区回退至修改的状态
git reset HEAD file.txt
```

## 4. 已提交，未推送-git reset

如果已经进行至`git commit`，但是还没有push至远程，这种情况直接回退版本就可以了。

```bash
#方法一：回退至上一版本
git reset HEAD^
#注意 HEAD 就是回退到当前版本，git reset HEAD^ 回退到上一版本

#方法二：回退至指定版本 
git reset <版本号> #git log 去查版本号

# 方法三：只是撤销commit，撤回至git add的状态，结合第4步可以回退至修改前的版本
git reset --soft HEAD^

# 方法四：使用远程仓库覆盖本地仓库
git reset --hard origin/master 
```

## 5. 已推送的修改

如果把本地的代码push至远程，此时可以采用回滚的形式回退版本，然后强制推送覆盖远程仓库。

以此执行下列代码：

```python
git reset --hard HEAD^ #回退至上一版本
git push origin HEAD --force #强制提交一次，之前错误的提交就从远程仓库删除
```

如果还想要找回之前的修改，可以使用`git reflog`，或者结合第四步一起使用。



---

# Authentication failed for错误解决

出现这种问题可能有以下几点：

1）git的密码输入错误（本质），当然在知道用户名密码的情况下都输错的低级错误那就不说了。

2）git的密码修改后而未及时修改为新的密码，导致出错。

3）git的账户被锁定。这个也是本人出错的原因，这儿跟大家分享一下解决方法。

首先，先查看有没有设置全局变量以及配置的全局变量对不对，如果不对的话就重新设置一下。

查询用户信息：git config --list

//查看一下你的信息修改的信息对不对

```
user.name=yuhua
user.email=xxx@xxx.com
```

如果不对就重新配置一下：

git config --global user.name [username]  如：git config --global user.name yh

git config --global user.email [email]         如：git config --global user.email 111@163.com

如果，是因为第二条原因出现的错误，那么我们就重新输入一下用户名及修改后的新密码：

我们需要做如下操作（idea工具下操作）

1）在file->Settings->Passwords下在Do not save,forget passwords after restart前打勾。

2）在控制面板的凭据管理器中找到**windows凭据**，将git的密码修改成已经改过的新密码（操作完后保险起见可以重启一下机器）。

3)如果是第三条原因引起的就该寻求一下解锁账户的方法了（每个公司可能都不太一样，我们用自己的工作app根据提示解锁一下就行，相当于验证一下是本人。有的公司可能需要找一下运维的同事给解锁一下）。



----

# [git 查看暂存区](https://blog.51cto.com/u_15127661/4597316)

 一、总结
一句话总结：
git ls-files 命令是用来查看暂存区中文件信息




二、git 查看暂存区
一、简介

git ls-files 命令是用来查看暂存区中文件信息

二、常用参数

参数说明（括号里是简写）

--cached(**-c**)显示暂存区中的文件，git ls-files (-c)命令**默认的参数**
--deleted(**-d**)显示删除的文件,git ls-files -d
--modified(**-m**) 显示修改过的文件
--other(**-o**)显示没有被git跟踪的文件
--stage(**-s**) 显示mode以及文件对应的Blob对象，进而我们可以获取暂存区中对应文件里面的内容。
二、实例
1.如何查看暂存区中有哪些文件？

**git ls-files**



2.如何查看暂存区中bb.txt文件内容是什么？

首先，我们需要查查bb.txt文件对应的Blob对象，如下：

**git ls-files -s -- bb.txt** 或者直接 **git ls-files -s**

然后通过Blob对象，查询bb.txt里面的内容：

git cat-file -p 6cef
![git 查看暂存区_人工智能_04](README.assets/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=.png)
