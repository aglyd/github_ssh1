# github_ssh1

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



git add [+filename1 filename2....]    添加文件到版本库

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

rm [+filename]	删除文件，可直接commit，或误删了用git checkout [+filename]重新检出该文件，前提是删除了还没commit

touch re.md	Linux touch命令用于修改文件或者目录的时间属性，包括存取时间和更改时间。若文件不存在，系统会建立一个新的文件。



分支管理策略。

   通常合并分支时，git一般使用”Fast forward”模式，在这种模式下，删除分支后，会丢掉分支信息，现在我们来使用带参数 –no-ff来禁用”Fast forward”模式。首先我们来做demo演示下：

1. 创建一个dev分支。
2. 修改readme.txt内容。
3. 添加到暂存区。
4. 切换回主分支(master)。
5. 合并dev分支，使用命令 git merge –no-ff -m “注释” dev
6. 查看历史记录