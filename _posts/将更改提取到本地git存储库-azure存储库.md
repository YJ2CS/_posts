---
title: 将更改提取到本地Git存储库-Azure存储库
siteurl: 将更改提取到本地git存储库_azure存储库
author: YJ2CS
avatar: /custom/avatar.webp
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - Git
tags:
  - Git
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-eda8'
date: 2020-11-10T22:16:00.000Z
date updated: '2020-12-26T19:54:10+08:00'

---

剪辑自: <https://docs.microsoft.com/zh-cn/azure/devops/repos/git/pulling?view=azure-devops&tabs=visual-studio>
Azure仓库| Azure DevOps Server 2019 TFS 2018 | TFS 2017 | TFS 2015 | VS 2017 | VS 2015
使用以下命令，使用团队其他成员的更改来更新本地存储库中的代码：
• fetch ，它会从远程存储库下载更改，但不会将更改应用到您的代码中。 --提取
• merge，该更改fetch将对本地存储库中分支执行的更改应用到本地。  --合并
• pull，这是一个组合命令，先执行a fetch然后执行a merge。  --拉取
在本教程中，您将学习如何：
• 通过下载下载更改
• 使用合并更新分支
• 提取并合并与拉
• 使用master的最新更改更新本地分支
影片总览
如果尚未推送的提交与正在合并或拉取的提交之间存在合并冲突，则需要先解决这些冲突，然后再完成代码更新。
通过下载下载更改
您可以通过远程将更改下载到本地分支fetch。Fetch向远程仓库询问其他人推送但您没有的所有提交和新分支，并将它们下载到您的仓库中，并根据需要创建本地分支。
Fetch 不会将任何更改合并到您的本地分支机构中，而只会下载新提交以供您查看。
提示
为了帮助使分支机构列表保持清洁和最新，请配置Git在提取过程中修剪远程分支机构。您可以从命令行或在Visual Studio中配置此设置。

1. ![](57ed7e49.png)

2.![](163e7539.png)

3. ![](460c6dc7.png)

使用合并更新分支
通过应用下载的更改fetch使用的merge命令。Merge接受从中检索的提交，fetch并尝试将它们添加到本地分支。合并将保留本地更改的提交历史记录，以便在与push共享分支时，Git将知道其他人应如何合并您的更改。
合并所面临的挑战是，从fetch分支中获取的提交与分支中现有的未推送提交发生冲突时。Git通常非常聪明，可以自动解决合并冲突，但是有时您必须手动解决合并冲突，并使用新的合并提交完成合并。
• 视觉工作室
• 命令行
从“ 更改”视图执行“ 拉出”或“ 同步”时，Team Explorer会合并。同步是拉动远程更改然后推送本地更改，同步本地和远程分支上的提交的组合操作。

1. 通过选择“ 主页”图标并选择“ 同步”，在Team Explorer中打开“ 同步”视图。

![](31ebaa4b.png)
2. 选择同步。
![](275e3f86.png)
3. 同步操作完成后，将显示一条确认消息。
![](475b1c3e.png)
在merge没有任何标志或参数的情况下运行，会将下载的提交添加fetch到本地分支中。如果有任何冲突，Git会添加一个合并提交。该合并提交有两个父提交（每个分支一个），并且包含为解决分支之间的冲突而提交的更改。
git merge
更新e2ccee6..55b26a5已
更改1个文件，1个插入（+）

您可以合并而不用提交--no-commit尝试执行合并，但不提交最终更改，这使您有机会在使用提交完成合并之前检查更改的文件。
提取并合并与拉
Pullfetch然后执行a ，然后执行a merge下载命令并使用一个命令而不是两个命令更新本地分支。pull当您不担心在将更改合并到自己的分支中之前要查看这些更改时，可使用该命令快速使您的分支与远程服务器保持最新。
• 视觉工作室
• 命令行
打开团队资源管理器，然后打开“同步”视图。然后单击“ 对远程更改的传入承诺”下的“ 拉”链接，并将其合并到本地分支中。在您打开的项目中提取更新文件，因此请确保在提交之前提交更改。pull

1. 通过选择“ 主页”图标并选择“ 同步”，在Team Explorer中打开“ 同步”视图。

![](c24e90e5.png)
2. 选择拉来获取远程更改并将它们合并到您当地的分行。（有两个“ 拉”链接，一个在顶部，一个在“ 传入的提交”部分中。您可以使用任何一个，因为它们都做同样的事情。）
![](2b44af0e.png)
3. 拉操作完成后，将显示一条确认消息。
![](2c2c1a9a.png)
git pull without any options will do a fetch of the changes you don't have from origin and will merge the changes for your current branch.

> git pull

Updating 55b26a5..e7926cd
1 file changed, 2 insertions(+), 1 deletion(-)

Pull a remote branch into a local one by passing remote branch information into pull:

> git pull origin users/frank/bugfix

This is a useful way to directly merge the work from remote branch into your local branch.
使用master的最新更改更新您的分支
在分支机构中工作时，您可能希望将主分支机构中的最新更改合并到分支机构中。您可以使用两种不同的方法来执行此操作：变基或合并。
• Rebase会在当前分支的提交中进行更改，然后在另一个分支的历史记录中重播它们。当前分支的提交历史记录将被重写，以便从重新定位目标分支中的最新提交开始。
• 合并使用合并提交将更改从源分支更改为目标分支，这将成为提交历史记录的一部分。
