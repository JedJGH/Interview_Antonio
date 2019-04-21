
### 5.git rebase和git merge的区别(饿了么)
    merge操作会生成一个新的节点，之前的提交分开显示。而rebase操作不会生成新的节点，是将两个分支融合成一个线性的提交。
### git fetch是干嘛的(美团)

### git的6个基本操作
    1). 创建本地仓库
       创建.gitignore配置文件
       git init
       git add *
       git commit -m "xxx"
    2). 创建远程仓库
       New Repository
       指定名称
       创建
    3). 将本地仓库推送到远程仓库
       git remote add origin https://github.com/zxfjd3g/xxx.git 关联远程仓库
       git push origin master
    4). 如果本地有更新, 推送到远程
       git add *
       git commit -m "xxx"
       git push origin master
    5). 如果远程有更新, 拉取到本地
       git pull origin master
    6). 克隆远程仓库到本地
       git clone https://github.com/zxfjd3g/xxx.git
