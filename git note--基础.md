我的学习笔记
这是我的第一个试错笔记
第一天：使用了git（这个版本控制器）在windows环境下，使用vscode先提交了github的仓库笔记

git的语句：
先git init 初始化一个仓库
git add . 把文件放入暂存区
git commit -m“main” 提交
git push -u origin main 推送，其中-u：关联。意思是“以后我直接输 git push，你就默认往 origin 的 main 分支推，别再问我了”。
git remote add http://...远程连接

知识点：
git add . 中 “.” 是linux的通用服，在git中这是“当前目录”，“把当前目录下（包括所有子文件夹）的所有变动，全部放进暂存区。”
          这是一个“购物车”。你挑选要提交的文件放在这，没选进去的就不会被提交
git commit 把购物车打包，生成一个永久的快照（版本）
git push 把本地的存档同步到 GitHub 服务器

拓展：
分支 (Branching)：
# 1. 创建并切换到一个新分支叫 dev (开发版)
git checkout -b dev

# 2. 在这里随便改，怎么改都不会影响 main 分支
# ... 修改文件，add，commit ...

# 3. 切回主分支
git checkout main

# 4. 把 dev 的修改合并过来 (Merge)
git merge dev


回滚 (Reset/Checkout)
场景A：文件改乱了，想丢弃修改（还没 add）
Bash
git checkout .
# 警告：这会清空你工作区所有未提交的修改，回到上次 commit 的状态。
场景B：已经 commit 了，想撤销这一次提交

Bash
git reset --soft HEAD^
# 意思：撤销最近一次 commit，但保留代码在“暂存区”（给你个机会重写备注或修改）。
场景C：查看历史穿越

Bash
git log --oneline
# 查看简洁的历史记录，你会看到每个版本都有一个黄色的 ID (比如 a1b2c3d)。
# 想穿越回去查看那个版本？ git checkout a1b2c3d

忽略文件 (.gitignore)
这是一个纯文本文件，名字就叫 .gitignore（注意最前面有个点）。
为什么需要它： 你在 Linux 学习中，会产生很多垃圾文件，比如：
*.log (几百兆的日志文件)
.env (包含数据库密码的配置文件 —— 绝对不能上传到 GitHub！)
temp/ (临时文件夹)
怎么做： 在项目根目录新建 .gitignore 文件，里面写：
*.log
.DS_Store
password.txt
Git 就会彻底“无视”这些文件，git status 再也不会提示它们。


对拓展的进行详细解析：

git checkout -b dev中：

checkout：切换视角/切换频道。
-b：Create Branch（创建分支）。
dev：给这个新宇宙起个名，叫“开发版”。
原理：Git 并没有复制你的文件（那样太占硬盘）。它只是在当前的时间点上，贴了一个新的标签叫 dev，并告诉你：“好，从现在开始，你所有的修改都记录在 dev 这个标签下，跟原来的 main 没关系了。”

git checkout main
在回到主支线的时候：会瞬间变回第 1 步之前的样子，它们没丢，它们只是保存在了 dev 那个平行宇宙里。Git 帮你把硬盘上的文件，瞬间还原成了 main 分支的状态。
你把写满的草稿纸扔在一边，从抽屉里拿出了最开始的那份原稿。现在的桌子上，只有原稿。

git merge dev
merge：合并/融合。
意思是：“我在 main 宇宙里，把 dev 宇宙的成果吸取过来。”
发生了什么： Git 会比较 main 和 dev 的差异。它发现 dev 比 main 多走了三步。 于是，它把那三步的修改，“重放” 到了 main 身上。 瞬间，你的 VS Code 里，刚才消失的代码又回来了！


工作区 (Working Dir)：你正在编辑的文件（还没 add）。
暂存区 (Staging/Index)：你 add 进去准备提交的文件。
版本库 (Repository)：已经 commit 封存的历史记录。

git checkout . 状态：你正在写代码，写了一百行，突然发现思路全错了，或者误删了核心配置。此时你还没有执行 git add
这个命令的意思是：“Git，请去版本库里把所有文件原本的样子拿出来，强制覆盖掉我工作区里现在的样子
这是一个不可逆的操作！
因为你还没提交，所以这些修改没有任何历史记录。一旦执行，你刚才那一百行代码就永远消失了，神仙也找不回。
建议：如果你不确定要不要删，先复制一份备份，或者养成频繁 commit 的习惯。

已经 commit 了，想撤销
命令：git reset --soft HEAD^：HEAD 代表当前版本，^ 代表“爸爸”，合起来的意思是：回退到上一个版本（爷爷版）。、
Git 的 Reset 有三种模式，soft 是最温柔的。
它只撤销“提交”这个动作，不撤销“代码修改”。
结果：你的代码变回了 commit 之前的状态（也就是都在暂存区里，绿色的状态）。
git reset：重置。

git log --oneline + git checkout [ID] 状态：你想看看上周的代码长什么样，或者想找回一段以前被删掉的代码。
git log --online的输出结果：
a1b2c3d (HEAD -> main) 添加了登录功能
8848abc 修改了背景颜色
9527ddd 第一次提交
那个 a1b2c3d 就是版本号 (Commit ID)，是通往那个时空的钥匙。
可能得异常："detached HEAD" (头指针游离) 状态。
正常状态：你是在写书（在分支的最末端），你写的每一页都会自动加在书的最后。
游离状态：你现在是游客。
你穿越回了历史。你可以“看”代码，可以“运行”代码。
但是！如果你在这里修改代码并提交，这些修改是不属于 main 主线的。它们就像是你在历史课本的某一页上贴了个便利贴。当你回到现代（主线）时，这个便利贴就丢了。
