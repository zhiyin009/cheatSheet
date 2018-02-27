'''
    学习笔记 廖雪峰git教程
    https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375233990231ac8cf32ef1b24887a5209f83e01cb94b000
    Create time:18-02-24
'''
# version -- 版本
git log                             # 打印历史版本 | | 可选参数：--graph --pretty=oneline --abbrev-commit 打印版本分支图
git reflog                          # 历史操作
git status                          # 当前状态

# file -- 文件
# git管理分为：工作区，暂存区，版本区
git add file                        # 提交文件file至暂存区
git commit -m "comment"             # 提交版本（暂存区），comment备注
git reset --hard HEAD~n             # 回滚n个版本
git reset --hard version_id         # 回滚至版本version_id（可从reflog查询历史版号）
git reset HEAD file                 # 撤销暂存区文件file修改，参考情景1
git checkout -- file                # 撤销工作区内文件file修改，还原到暂存区或者上个版本
git rm file                         # 删除文件（工作区、暂存区）
git rm --cached file                # 删除文件（暂存区）

# remote -- 远程库
git remote                          # 打印远程库 | 可选参数：-v显示更详细的信息
git remote add path                 # 关联远程库，情景2
git clone path                      # 克隆远程库，会在./下创建一个项目文件
git pull                            # 拉取最新版本，参考情景5
git push -n origin master           # 第一次推送master分支所有内容
git push origin master              # 推送master分支到origin库

# branch -- 分支
git branch                          # 打印分支列表，情景6
git merge dev                       # 合并dev分支到当前分支 | 可选参数：--no-ff禁用Fast forward模式，删除分支后，保留分支信息。
git branch dev                      # 创建分支dev
git checkout dev                    # 切换分支dev
git chechout -b dev [origin/branch-name]
                                    # 创建并切换分支dev，相当于git branch dev && git checkout dev，[origin/branch-name]为远程库分支
git branch -d dev                   # 删除分支dev | 可选参数：-D强制删除
# 分支类型
    # master分支是主分支，因此要时刻与远程同步；

    # dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

    # bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；

    # feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

# stash -- 隐藏
git stash                           # 隐藏当前环境变动（提供一个干净的工作区和暂存区），情景4
git stash list                      # 打印隐藏列表
git stash apply stash_id            # 恢复环境，但是恢复后，stash内容并不删除
git stash drop stash_id             # 删除环境
git stash pop stash_id              # 恢复同时删除

# tag -- 标签
git tag                             # 打印当前标签列表 |　可选参数：-s：用GPG标签 -a：后接tag名 v0.1 -m "blablabla" 
git tag name                        # 打下标签name
git show name                       # 打印标签name的版本信息

# alias -- 别名
git config --global alias.st status # 配置st作为 status的别名
git config --global alias.unstage 'reset HEAD'
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
.git/config                         # 库配置文件
.gitconfig                          # 当前用户配置文件

# ignore -- 忽略文件
# 有些时候，你必须把某些文件放到Git工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件，每次git status都会显示Untracked files ...
# 这个问题解决起来也很简单，在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。
# 不需要从头写.gitignore文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore
# 忽略文件的原则是：
# 1.忽略操作系统自动生成的文件，比如缩略图等；
# 2.忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
# 3.忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

# scene -- 情景 
# 情景1：写错了，而且add过了，先用git reset HEAD file撤回暂存区，然后git checkout -- file撤回工作区。
# 情景2：Git支持多种协议，默认的git://使用ssh，但也可以使用https等其他协议。https速度慢，每次推送都必须输入口令，但是在某些公司只开放http端口。
# 情景3：conflict出现时，Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容，修改后保存再提交
# 情景4：开发的时候，突然需要修BUG。可以先git stash，然后创建一个新的分支issue，在issue中debug提交后，merge回master，再git stash pop继续开发
# 情景5：同事已向origin/dev分支推送了，碰巧自己也对同样的文件作了修改，导致冲突。先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送
# 情景6：git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令：git branch --set-upstream dev origin/dev
