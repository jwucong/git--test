
# git
> [官方文档](https://git-scm.com/docs)


### 1. 项目创建

1. **git init**

   新建一个 git 仓库，如果指定了目录，则新建一个对应的目录作为仓库，否则将当前目录作为仓库。

   ```bash
   git init [directory]
   ```

2. **git clone**

   将一个已存在的 git 仓库下载到本地，可以指定一个文件夹名称。

   ```bash
   git clone url [directory]
   ```


### 2. 文件操作(建立git快照)

1. **git status**

   查看文件的改动
   
   ```bash
   git status
   ```

2. **git add**

   将 working 中的改动添加到 index 中，pathspec 是一个路径匹配模式，被这个模式命中的文件都会被添加。如：  
   
   a. 仅添加有更新的文件(不会添加新增的文件)：git add -u   
   b. 添加所有文件: git add -A   
   c. 添加指定文件: git add README.md   
   d. 添加一堆文件: git add a.md b.md c.md   
   e. 添加.md后缀名的文件: git add *.md
   
   ```bash
   git add [options] [pathspec]
   ```
   
3. **git rm**
   
   删除文件(夹)

   ```bash
   # -f: 强制删除
   # -r: 递归删除
   git rm [options] [pathspec]
   ```
   
4. **git mv**

   移动、重命名文件(夹)
   
   ```bash
   git mv <from> <to>
   ```
   
5. **git commit**

   把改动记录到本地git仓库中，commit操作如果不加任何参数，会自动打开一个编辑器，通常是vi(m)编辑器
   
   ```bash
   git commit [options] [pathspec]
   ```

6. **git reset**

   ```bash
   git reset [options] [commitId]
   ```
 
   将当前的HEAD移动到commitId这个节点上，此命令的最终结果是，节点往后退，节点内容也往后退，
   此操作通常会配合以下三个配置选项：  
   
   --soft: 保留 working 的改动 和 index 记录   
   --mixed: 保留 working 的改动，撤销 index 记录，此配置为默认配置  
   --hard: 直接擦除所有改动
   
   例如,当前的最新节点为G，想要退回到节点C(假设通过git log查到commitId为c0)，则:  
   
   ```bash
   # 回退前节点图: A --> B --> C --> E --> F --> G (HEAD)
   
   git reset c0
   
   # 回退后节点图: A --> B --> C (HEAD)
   ```
   
   通过上图可以看出，reset在回退时，删除了 E、F、G这三个中间节点，在git log中查询不到了(可以通过git reflog查到)，
   同时，如果节点G已经push到远端，回退后造成了远端提交超前本地提交，再push时需要-f强制推送；
   另外，因为--hard配置直接擦除修改的特性；所以，我们认为reset是一个危险的操作，
   一般情况下，建议只在本地分支、个人分支或者需要回退的节点没有push到远端时才进行reset操作，
   不建议在公共分支上使用此命令，除非公共分支上出现了一些"见不得人"的记录，
   如果需要回退公共分支上的代码，推荐使用下面的git revert命令
   
7. **git revert**
   > 使用此命令后会自动打开编辑器(通常是vi(m)编辑器，也可能是其他)让你去写此次 revert 操作的原因。

   ```bash
   git revert [options] [commitId]
   ```
   
   git revert也是一个回退操作，与reset不同的是，revert将使用一次新的commit来回退到指定的节点上，
   此命令的最终结果是，节点往前推，节点内容往后退。
   options配置参数一般不需要，
   值得注意的是，此命令使用的commitId和reset使用的commitId有所不同，
   这里需要的commitId是你需要回退到的目标节点的靠近 HEAD 那一侧的那个邻居节点的commitId(有点绕哈，不知道有没有说清楚)
   
   例如,当前的最新节点为G，想要回退到节点C，那么commitId应该用E的commitId(假设通过git log查到commitId为e0),则: 
   > 节点C的邻居是B和E，靠近 HEAD 那一侧的是E，所以应该用E的commitId
   
   ```bash
   # 回退前节点图: A --> B --> C --> E --> F --> G (HEAD)
   
   git revert e0
   
   # 如果实在不知道e0怎么来的，那就直接找C的id(c0)，然后:
   # git revert c0^
   
   # 回退后节点图: A --> B --> C --> E --> F --> G --> H (HEAD)
   ```

   从上图看，revert操作产生了H这一个新的commit，它的commitId和C的不同，但是内容和C的一样，
   所以说该命令的最终结果是：节点往前推，节点内容往后退。这就跟"这场战争使我们经济倒退30年"这句话是一样的，
   时间不断的往前，但是在这个时间下的生活状态却跟30年前一样。这就是revert和reset最大的区别。
   
   由于git revert操作完整的保留了commit历史，所以一般建议用于公共分支的代码回退。
  

