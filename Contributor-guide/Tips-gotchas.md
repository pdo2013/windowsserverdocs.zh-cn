# <a name="tips-and-gotchas"></a>提示和问题

如果您是初次接触 Git，学习来有效地处理可能会令人沮丧。 因此，如果它已包含在本指南中的其他信息，请查找比更容易，此处收集信息。

## <a name="basics"></a>基础知识：

此参与者指南中的命令假设你已配置 Github，以指定默认存储库中的文件中提取。 在 Github 术语中，请求中的文件的位置是你*上游*。 推送到的文件的位置是你*原点*。 

由于上如何设计我们的存储库和工作流，在上游应设置为我们的存储库，这是在 Microsoft 的组织-下 https://github.com/Microsoft/WindowsServerDocs-pr和源应为此存储库，这是你自己的 Github 帐户下的分叉。 例如， https://github.com/MY-GITHUB-ALIAS/WindowsServerDocs-pr 

>若要检查你的设置，请键入```git config -l```。 查看以确保它们引用你预期的位置的 Url。

一些基本的分支的提示：

-  工作分支，不在主节点的本地副本中。 这使得更轻松地从你的分支中的问题中恢复并返回到正常且已知点。

-  基于远程分支上不共享的存储库中的一个分支的本地分支。 然后，会将本地分支推送到远程分支。 从远程分支，你将请求中的分支合并到共享存储库安装了更新。 在本文中的命令显示了如何执行此操作。

-  创建一个本地分支时，你的命令指示 Git 你想要将新分支的基础哪些的分支。 这是何时以及如何指定 master 或里程碑或功能分支。 例如，此命令从上游分支创建一个新分支，并因此已准备好在其中进行工作，然后检查出新分支：

        git checkout upstream/upstream-branch-name -b your-local-branch-name

有关 markdown 的帮助，启动与此[一文](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/)在 Github 中。 对于表，请参阅此[一个](https://help.github.com/articles/organizing-information-with-tables/)。

## <a name="gotchas"></a>Gotcha 的

### <a name="deleting-files"></a>正在删除文件
它不能只是从文件系统中删除文件。 使用 Git 命令，以便让你知道系统旨在执行此操作，否则你已删除的文件将可能成为僵停文件会再次出现。 永远不会在很好的时间。 毕竟，这是源代码管理系统，如果您不告诉它确实要删除这些文件，它将帮助将它们添加为你。 谢谢，Git ！ 有关说明，请参阅[重命名或停用一篇文章](rename-r-retire.md)。

### <a name="merge-conflicts"></a>合并冲突

如果在本地工作时遇到合并冲突，需要修复此错误，或退出它。 大多数情况下最好仅修复这些错误，除非它们不是你的文件。 请记住如果您不执行任何操作，你可能会阻止将所做的更改推送到源，因此将需要对内容执行的操作。

**如何修复**-Azure 指南具有更多有关此信息和说明，用于解决冲突。 **合并冲突的拉取请求应永远不会接受**。 请务必替换我们的存储库和分支的所有示例中的名称，当您查看时[说明](https://microsoft.sharepoint.com/teams/azurecontentguidance/wiki/Pages/Resolve%20a%20local%20merge%20conflict%20yourself.aspx)。 

**如何返回**-如果冲突是在文件中不拥有或没有，则无法修复它们时，此处的如何返回以前的状态的原因。这会重置回如何合并冲突发生之前它们所在的文件在本地集中。 运行此命令：

```git merge --abort```

如果你尚未暂存和提交您的本地工作，就可以做到现在。 但是，如果将这些更改推并打开一个 PR，冲突将可能重新出现如果之前更改与文件中。 你必须修复此错误，或从其他问题，可以接受请求之前人获取帮助。 因此，最好仅使用方法自行解锁，如果您有紧急的工作，例如关键更新。

