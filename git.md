## 基本操作
该基本操作已经默认 Linux 的基本操作已经掌握，如有不会可看我主页中有关 Linux 的讲解文件
- 查看版本：`git --version` 或 `git -v`
- 清除命令行界面：`clear`
- 设置用户信息：
  - 设置用户名：`git config --global user.name 用户名`
  - 设置用户邮箱：`git config --global user.email 用户邮箱`
- 查看配置信息：
  - 查看用户名：`git config --global user.name`
  - 查看用户邮箱：`git config --global user.email`
- 为常用指令配置别名步骤：
  - 打开用户目录，创建 `.bashrc` 文件，若创建不了以点号开头的文件，可用命令：`touch ~/.bashrc`
  - 在 `.bashrc` 文件中输入如下内容：
   ``` bash {.line-numbers}
    # 用于输出git提交日志
    alias git-log='git log --pretty=oneline --all --graph --abbrev-commit'
    # 用于输出当前目录所有文件及基本信息
    alias ll='ls -al'
   ```
   - 在 GitBash 中执行 `source ~/.bashrc`
 - GitBash 乱码问题的解决：
   - 打开 GitBash 执行命令: `git config --global core.quotepath false`
   - `${git_home}/etc/bash.bashrc` 指git的安装目录下的 e 目录下的文件 `bash.bashrc` ，在其最后加入下面两行：
   ``` bash {.line-numbers}
    export LANG="zh_CN.UTF-8"
    export LC_ALL="zh_CN.UTF-8"
   ```
   - **注：** 若找不到git的地址，可以在 GitBash 中输入命令 `where git` 来查询git的安装文件路径

## Git 常用命令
### 获取本地仓库
1. 在电脑任意位置创建一个空目录作为本地的仓库
2. 进入该目录中，右键打开对应的 GitBash 窗口
3. 执行命令 `git init`
4. 创建成功后可在文件夹下看到隐藏的 `.git` 目录

### 基础操作指令
#### 插入一张关于文件状态的转换示意图

1. 查看修改的状态
   - 作用：查看暂存区、工作区文件的修改的状态
   - 命令：`git status`
2. 添加工作区到暂存区
   - 作用：添加工作区一个或多个文件的修改到暂存区
   - 命令：
     - 单个文件名：`git add 文件名`
     - 使用通配符 `*` 添加多个文件 `git add 通配格式`
     - 添加目录中的所有文件：如添加工作区的所有文件 `git add .`
3. 提交暂存区到本地仓库
   - 作用：提交暂存区内容到本地仓库的当前分支
   - 命令：`git commit -m '注释内容'` 
4. 查看提交日志
   - 作用：查看提交记录
   - 命令：`git log [option]`
     - 关于 option ：
       - `--al` ：显示所有分支
       - `--pretty=oneline` ：将提交信息显示为一行
       - `--abbrev-commit` ：使得输出的 commit id 更简短
       - `--graph` ：以图的形式显示
       - **注：** 可以配置别名 `git-log` 来表示这四个选项都加上的命令
5. 版本回退
   - 作用：版本切换
   - 命令：`git reset --hard commitID`
     - commit ID 可以使用 `git log` 的相关指令来查看，最好用`--abbrev-commit`，ID 更为简短
   - 查看已经删除的记录：`git reflog` 这个指令可以看到已经删除的提交记录
6. 添加文件至忽略列表
   - 目的：将一些无需纳入 Git 管理的文件不出现在未跟踪文件列表中，即不让 Git 进行管理；这些文件通常都是些自动生成的文件，比如日志文件、编译过程中创建的临时文件等
   - 方法：在工作目录中创建一个名为 `.gitignore` 的文件（文件名称固定），列出要忽略的文件模式
   - 示例：
   ``` {.line-numbers}
    # no .a files
    *.a
    # but do track lib.a, even though you're ignoring .a files above
    !lib.a
    # only ignore the TODO file in the current directory, not subdir/TODO
    /TODO
    # ignore all files in the build/ directory
    bulid/
    # ignore doc/notes.txt, but not doc/server/arch.txt
    doc/*.txt
    # ignore all .pdf files in the doc/ directory
    doc/**/*.pdf
   ``` 

### 分支
1. 查看本地分支：`git branch`
2. 创建本地分支：`git branch 分支名`
   - **注：** 创建新分支时，新分支会完全继承当前所在分支的 所有代码、提交记录、进度状态，在新分支刚创建时，与当前所在分支一模一样，可以理解为从当前状态开始分叉了
3. 切换分支：`git checkout 分支名`
   - 我们还可以直接切换到一个不存在的分支（创建并切换）：`git checkout -b 分支名`
   - **注：** 
     - 工作区只能在一个分支状态中，不能同时处于多个分支，而 `HEAD` 是指向当前所在分支的“**指针**”
     - 在分支图中看分支位置时，一定注意**要看星号，不要被线影响**，星号 `*` 是每个提交的**唯一身份标识**，线只是结构装饰，绝对不能用线的对齐来对应提交，必须看星号所在的行
4. 合并分支：`git merge 分支名` 将一个分支上的提交合并到当前的分支上（建议合并到分支 main/master 上）
5. 删除分支：
   - `git branch -d 分支名` 删除分支时，需要做各种检查
   - `git branch -D 分支名` 删除分支时，不做任何检查，强制删除
   - **注：** 不能删除当前分支，只能删除其他分支
6. 解决冲突：
   - 说明：两个分支上对文件的修改可能会存在冲突，如同时修改了同一个文件的同一行，那么这时就要手动解决冲突
   - 解决步骤：
     - 处理文件中存在冲突的地方（ Git 会自动帮我们把冲突的内容在文件中标注出来，我们只要按需修改即可）
     - 将解决冲突了的文件加入暂存区
     - 提交到仓库
7. 开发中分支使用原则与流程：在开发中，一般有如下分支使用原则与流程：
   - master(生产)分支：线上分支、主分支，中小规模项目作为线上运行的应用对应的分支；
   - develop(开发)分支：是从 master 创建的分支，一般作为开发部门的主要开发分支，如果没有其他并行开发不同期上线要求，都可以在此版本进行开发，阶段开发完成后，需要是合并到 master 分支准备上线。
   - feature/xxxx分支：是从 develop 创建的分支，一般是同期并行开发，但不同期上线时创建的分支，分支上的研发任务完成后合并到 develop 分支
   - hotfix/xxxx分支：从 master 派生的分支，一般作为线上 bug 修复使用，修复完成后需要合并到 master 、test 、develop 分支
   - 还有一些其他分支，在此不在详述，如 test 分支（用于代码测试）、pre 分支（预上线分支）等等

#### 此处添加一个示意图片 -- 实际开发时分支使用的示意图

## Git 远程仓库
### 配置 SSH 公钥
- 生成 SSH 公钥
  - 输入命令 `shh-keygen -t rsa`
  - 再不断回车
    - 如果公钥已经存在，则会自动覆盖
- 查看公钥：`cat ~/.ssh/id_rsa.pub`
- 验证是否配置成功：`ssh -T git@gitee.com`

### 操作远程仓库
1. 添加远程仓库
   - 命令：`git remote add <远端名称> <仓库路径>`
     - 远端名称：默认是 origin ，取决于远端服务器的设置
     - 仓库路径：从远端服务器获取此 URL
     - 例：`git remote add origin git@giteecom:czbk_zw/git_test.git`
   - 注：此操作之前要先初始化本地库，然后再与已创建的远程库进行对接
2. 查看远程仓库：`git remote` 
3. 推送到远程仓库：`git push [-f] [--set-upstream] [远端名称 [本地分支名][:远端分支名]]`
   - 如果远端分支名和本地分支名相同，则可以只写本地分支，如 `git push origin master`
   - `-f` 表示强制覆盖到远端上
   - `--set-upstream` 表示当前分支推送到远端的同时并且建立起和远端分支的关联关系
   - 如果当前分支已经和远端分支关联，则可以省略分支名称和远端名，即 `git push`
4. 本地分支与远程分支的关联关系：
   - 可以通过命令 `git branch -vv` 来查看本地分支和远程分支的一定关系
5. 从远程仓库克隆：
   - 功能：如果已经有一个远端仓库，可以直接 clone 到本地
   - 命令：`git clone <仓库路径> [本地目录]`
     - **注：**本地目录可以省略，会自动生成一个目录
6. 从远程仓库中抓取和拉取：
   - 说明：远程分支和本地分支一样，都可以进行 `merge` 操作，只是需要先把远端仓库里的更新都下载到本地，再进行操作
   - 抓取命令：`git fetch [remote name] [branch name]`
     - **抓取指令就是将仓库里的更新都抓取到本地，不会进行合并**
     - 如果不指定远端名称和分支名，则抓取所有分支
   - 拉取命令：`git pull [remote name] [branch name]`
     - **拉取指令就是将远端仓库的修改拉到本地并自动进行合并，等同于 `fetch + merge`**
     - 如果不指定远端名称和分支名，则抓取所有并更新当前分支
7. 解决合并冲突：
若两个不同的用户修改了同一个文件，且修改了同一行位置的代码，此时就会发生合并冲突。
比如 A 用户在本地修改代码后优先推送到远程仓库，此时 B 用户在本地修订代码，提交到本地仓库后，也需要推送到远程仓库，那么此时 B 用户就晚于 A 用户了，**故需要先拉取远程仓库的提交，经过合并后才能推送到远端分支**。
#### 此处展示示意图 --远端仓库的合并冲突
在 B 用户拉取代码时，因为 A 、 B 用户同一段时间修改了同一个文件的相同位置代码，故会发生合并冲突
**远程分支也是分支，所以合并时冲突的解决方式也和本地分支冲突的解决方式相同**