---
title: 写项目经验总结
date: 2024-06-14 20:34:27
tags: 前端 git
categories: bo‘k
---


## git merge
`git merge`不对仓库历史做任何改变，它会保留分支上所有的历史commit，然后简单地创建一个合并的commit。
**使用`git merge`将`master`分支合并到`feature`分支**
```
git checkout feature
git merge master
```
![image.png](/tencent/api/attachments/s3/url?attachmentid=22043965)
当执行了git merge之后
![image.png](/tencent/api/attachments/s3/url?attachmentid=22043845)
上面的操作会在`feature`分支上产生一个新的commit，它是`c4`和`c7`的合并为`c8`，`c8`包含了`master`分支上的所有修改

**总结**：
- git merge 会创建一个新的提交，这个提交有两个父提交，分别是你当前所在的分支和要合并的分支。这样，提交历史会变成一个有分叉和合并点的图形结构。
- 在 `git merge` 中，冲突解决是在合并过程中一次性完成的。一旦解决了冲突，就可以继续合并操作。
- 更适合用于管理已经存在的分支，特别是当你想要保留完整的提交历史时。
## git rebase
构造两个分支master和feature，其中feature是在提交点B处从master上拉出的分支
master上有一个新提交M，feature上有两个新提交C和D
![image.png](/tencent/api/attachments/s3/url?attachmentid=22044390)
此时我们切换到feature分支上，执行rebase命令，相当于是想要把master分支合并到feature分支（这一步的场景就可以类比为我们在自己的分支feature上开发了一段时间了，准备从主干master上拉一下最新改动。模拟了git pull --rebase的情形）
下图为变基后的提交节点图
![image.png](/tencent/api/attachments/s3/url?attachmentid=22044506)
当在feature分支上执行git rebase master时，git会从master和featuer的共同祖先B开始提取feature分支上的修改，也就是C和D两个提交，先提取到。然后将feature分支指向master分支的最新提交上，也就是M。最后把提取的C和D接到M后面，注意这里的接法，官方没说清楚，实际是会依次拿M和C、D内容分别比较，处理冲突后生成新的C’和D’。一定注意，这里新C’、D’和之前的C、D已经不一样了，是我们处理冲突后的新内容，feature指针自然最后也是指向D’

rebase，变基，可以直接理解为改变基底。feature分支是基于master分支的B拉出来的分支，feature的基底是B。而master在B之后有新的提交，就相当于此时要用master上新的提交来作为feature分支的新基底。实际操作为把B之后feature的提交先暂存下来，然后删掉原来这些提交，再找到master的最新提交位置，把存下来的提交再接上去（接上去是逐个和新基底处理冲突的过程），如此feature分支的基底就相当于变成了M而不是原来的B了。（注意，如果master上在B以后没有新提交，那么就还是用原来的B作为基，rebase操作相当于无效，此时和git merge就基本没区别了，差异只在于git merge会多一条记录Merge操作的提交记录）

**总结**：
- `git rebase` 会把要合并的分支的提交逐个应用到当前分支的基础上，形成一个线性的提交历史。这样，提交历史会变得更加简洁、清晰。
- 在 `git rebase` 中，冲突解决是在逐个应用要合并的分支的提交时进行的。这意味着你可能需要多次解决冲突，特别是当要合并的分支有很多提交时。
- `git rebase` 更适合用于管理正在开发中的分支，尤其是在准备将分支合并到主分支之前，因为它可以让提交历史更加整洁。
- 当你想要简化提交历史，或者想要将一个分支的提交整合到另一个分支的基础上时，应该使用 `git rebase`。

