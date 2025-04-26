# 代码提交规范

# 1. 保护分支

master分支被设置为保护分支, 意味着除了仓库owner以外, 不允许其他开发者直接在master上推送代码(直接推送保护分支会报错)

## 2. 代码提交流程规范

1. 接受邀请为仓库协作者
2. git clone 将仓库克隆下来
3. **接受需求**: 假设本次需求是 "A文章编写", 就在本地新建分支: feat/article_a, 并将该分支推送到远程仓库
4. 在 feat/article_a 分支上完成本次需求的代码编写
5. **提交代码:** 提交pr前必须将远程master分支代码拉一份到本地分支, 如下

```shell
git checkout feat/article_a # 在本地切换到正在开发的分支
git pull origin master      # 将远程master分支拉取到本地开发分支, 保证自己的代码是团队中最新的代码(master意味上线上运行着的代码)
git push                    # 将本地 feat/article_a的最新代码,推送到远程的feat/article分支
```

此时有冲突需要在本地解决冲突

6. 提交pr: 在本仓库上点击 new pull request, 将远程的feat/article 提交pr到 master分支.
7. code review: 代码会由仓库owner审核, 提出修改意见, 在提出修改意见后需要修改代码, 重新提交(只需要git push即可)
8. review后,代码被合到master
