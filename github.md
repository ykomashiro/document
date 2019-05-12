# Github 教程

## 初始化

首先进入你想要上传但GitHub的文件夹里，单击右键，选择`git Bash`。键入`git init`，之后你的目标文件文件夹会多出一个`.git` 的隐藏文件夹，这是就证明你已经初始化完成。

## 链接到Github仓库

在你的个github 新建一个仓库并复制对应SSH，然后在本地的`bash` 键入命令`git remote add origin "your_repository_ssh"`，Enter之后链接成功。

## 推送到github仓库

```git
git add.
git commit -m "commit"
git push -u origin master
```