```
# git的常用命令
## 使用场景
1. 本地仓库与git remote仓库的关联
    * 首先要在github账户上，创建与本地仓库同名的仓库repository;
    * 进入本地仓库的根目录，开始下面一系列的操作
        1. 可以选择创建readme.md文件；　echo "# -" >> README.md
        2. 初始化该仓库；　git init
        3. 将创建的文件README.md文件放置暂存区；　git add README.md
        4. 将暂存区的修改提交; git commit -m "你的提交信息"
        5. 将本地仓库推送到github上；git remote add origin git@github.com:账户名/仓库名.git,git push -u origin master
    * 到此已经基本完成。但是还是需要将你的电脑配置一下
        1. git config --global user.name "your account name"
        2. git config --global user.email "your account email"
        3. 到此已经在当前用户配置了你的信息。下面需要生成你的ssh-key并添加到github上
        4. 生成秘钥；ssh-keygen -t rsa -C "your account email" (连续回车三次)
        5. 最后将公钥添加到git账户里，就OK啦。
    * 有时候工作环境和个人环境都需要一个秘钥，这时候可以在生成秘钥的时候，指定名字并且需要添加config文件进行指定
        1. 生成指定名称的秘钥；ssh-keygen -t rsa -C "your account email" -f ~/.ssh/指定文件名
        2. 添加特别的配置，在ssh目录下新建config文件，内容如下
            ```ruby
            Host github-aysee
            HostName github.com
            User git
            IdentityFile C:/Users/username/.ssh/aysee
            你应该可以看懂的吧－－
            ```
         3. 在指定项目下，指定作者
            ```
            git config user.name "Author name"
            git config user.email "Author email"
            ```

2. 一次git提交生命过程中遇到的兄弟们（这里并不准备详细的介绍这些哥们）
    * 新建文件/目录，修改，提交
    * 一般仓库中的文件可能存在于这三种状态
    ```
        1. Untracked files 文件未被跟踪　【处于工作区】
        2. Changes to be committed 文件已经被暂存，这是下次提交的内容　【处于暂存区】
        3. Changes but not updated 文件被修改，但没有添加到暂存区　【工作区】
    ```
    * 在开始新的路上，要把有变化的文件添加到索引库中，这个时候首先要遇见：git add命令
        1. git add -A \[path]: 表示把path内的所有的tracked文件那些被修改／被删除／以及untrack的文件，添加到索引库中
        2. git add -u \[path]: 把path中所有的tracked文件，那些被修改／被删除的文件，添加到索引库中
        3. git add -i \[path]: 可以查看到path中所有被修改过／被删除但是还没有提交的文件
        4. add兄弟家族还有好多，具体可以看看--help管家
        
    * 经历了add家族的接待，那些被添加到索引库的文件即将遇到另外一个兄弟：git commit命令，
        1. git commit -m "提交的描述信息"　：提交暂存区的文件
        2. git commit -a -m "提交的描述信息"　：　提交暂存区以及工作区中已被tracked文件
        3. git commit --amend "再次补充添加上次提交的描述信息"：补充提交信息
    * 经由commit的指引，我们来到了暂存区，这里都是即将要被push到老家的兄弟们：git push命令
        1. 该命令用于将本地分支更新到暂存区的文件，推送到远程主机：
        2. git push \<远程主机>　\<本地分支>:\<远程分支名> ; 一般常用 git push，其他更具体的用法请Ｇｏｏｇｌｅ
        3. 常见的一些情况：
            * ```
                1.git push origin master :表示　将本地分支master推送到origin主机的master分支，如果不存在，则新建
                2.git push origin :master 表示　删除指定的远程分支master　
                    等同于推送一个空本地分支到远程分支
                    git push origin --delete master
                3.git push 推送当前分支到远程
                4.git push --all　推送所有本地分支到远程
                ```
      
3. 天下大事，合久必分，分久必合。
    * 接下来git branch主要用法
        1. git branch 查看本地分支列表，当前分支“*”标示
        2. git branch \<branchname>　创建分支，基于当前HEAD指向的提交
        3. git branch \<branchname> \<start-point>　创建分支，基于指定的提交节点
        4. git branch -d \<branchname> 　删除分支，弱删除（删除时检查索要删除的分支是否已经合并到其他分支中，否则拒绝）
        5. git branch -D \<branchname>　强制删除
        6. git branch -m \<oldbranch>　\<newbranch> 重命名分支，如果版本库中已经存在newbranch的分支，拒绝
        7. git branch -M \<oldbranch>　\<newbranch>　重命名分支，即使版本库中已经存在newbranch的分支，强制执行
        
    * 那么git merge主要用法：把一个分支或某个commit的修改合并到现在的分支上
        1. ```
              usage: git merge [options] [<commit>...]
                 or: git merge [options] <msg> HEAD <commit>
                 or: git merge --abort  
            ```
        2. 先切换到目的分支：git checkout destination-branch
        3. 再将目标分支合并到目的分支：　git merge aim-branch
        4. 如果有冲突，先解决，再经过一次提交过程，完成合并
        
        
4. 关于git的一些必要概念
    * 工作区：当前工程目录范围下
    * 暂存区：即stage，一般存放在工程根目录下的.git/index文件夹，
    * 版本库：工作区有一个隐藏目录.git　这个就是当前工程的版本库,是一个简单的数据库，其中包含所有用来维护与管理项目修订版本与历史信息。
```