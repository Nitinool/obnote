

##### git同步笔记教程


准备
1 初始化本地仓库 在本地文件的文件夹打开git bash使用 git init 初始化本地仓库
2 设置.gitignore文件 这一步确保某些文件不需要同步
3 链接远程仓库 git remote add origin https://github.com/your-username/obsidian-notes.git
这一步意思是 连接了一个名叫origin的远程仓库

第一次提交
``` 
#所有操作在obsidian的笔记文件夹目录下的git bash 里面完成

# 添加所有文件到暂存区
git add .

# 创建第一个提交
git commit -m "Initial commit"

# (可选但推荐) 确保你的主分支名为 main
git branch -m master main

# 首次推送到远程仓库，并建立跟踪关系
git push -u origin main
```

日常提交和拉取最新的笔记
```
# 从名为 `origin` 的远程仓库，拉取 `main` 分支的最新内容，并与你的本地分支合并。
git pull origin main

git add .
git commit -m "写下本次修改的摘要信息"
git push origin main
```










常用命令
- git status 查看当前仓库状态
- git init 初始化当前仓库
- git add + 文件名 将当前文件添加到缓冲区
- git commit -m + 文件名 提交当前文件到仓库
- git log 查看日志
- git branch 查看分支
- git branch + 名字 创建分支
- git checkout + 名字 切换分支
- git merge + 名字 在某个分支合并“名字”分支
- git tag v1.0 为当前分支添加标签


链接github和git
1.首先在gitbash输入 ssh-keygen -t rsa 生成sshkey 然后一直回车
2.会生成两个文件 一个密钥 id_rsa  一个公钥  id_rsa.pub
3.在github主页设置新建一个ssh key 把公钥的内容添加进去
4.在gitbash输出 ssh -T git@github.com 查看是否链接成功

向github远程仓库push和pull代码
第一种情况 本地没有git仓库
1.先直接把远程仓库clone下来 也就是 git clone + 仓库的http连接
2.如果需要添加新的文件到仓库 那就git add + git commit -m 这样文件就在本地仓库了
3.push 到远程仓库 git push origin master 第一次需要验证账号密码或者验证token

第二种情况 本地有git仓库 需要关联远程仓库并合并
1.首先把本地仓库初始化并连接远程仓库 git remote add origin + 远程仓库地址
2.git remote -v 查看当前连接仓库
3.git pull origin master  把远程仓库下载下来 让本地保持最新
4.把本地需要更细的文件先git add git commit -m 提交
5.在用git push origin master 更新远程仓库

asdada