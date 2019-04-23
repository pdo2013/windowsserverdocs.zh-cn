# <a name="rename-or-delete-an-article"></a>重命名或删除项目

在重命名或删除项目之前，需要遵循此过程来避免或减少中断的链接数。 客户不喜欢断开的链接，并不是有关关闭时命中一个听起来比较羞涩。 请注意，重命名或删除项目是在此过程中，最后一个步骤不在第。


## <a name="step-1-manage-inbound-links"></a>第 1 步：管理入站的链接

确定是否有任何内容，如博客、 论坛和其他内容在 web 上找到的非 Microsoft 入站的链接。 请与博客所有者可以更改这些链接，然后删除或更新论坛帖子中的链接。 借助 web 分析工具可帮助识别可能需要以这种方式管理的高流量入站的链接。

## <a name="step-2-remove-all-crosslinks-to-the-article-from-the-windowsserverdocs-pr-repository"></a>步骤 2：文章从 WindowsServerDocs pr 存储库中删除所有交叉链接

1. 刷新你的本地分支 – 运行`git pull upstream <branch>`，即要刷新从主服务器，请运行 `git pull upstream master`

2.  扫描 WindowsServerDocs pr 文件夹以查找具有你想要重命名或停用的文章的链接的项目，然后更新的文章，以删除链接或将它们替换为另一篇文章的链接。 可以使用搜索和替换实用工具来查找交叉链接，如果你已安装。 如果不这样做，则可以使用 Windows PowerShell:

 a. 启动 Windows PowerShell。

 b. 在 PowerShell 提示符下，将更改为 WindowsServerDocs pr 文件夹：

 `cd WindowsServerDocs-pr\WindowsServerDocs-pr`

 c. 运行命令以列出包含要重命名或删除的文章的链接的所有文件：

 `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name`

  若要将文件名称的列表发送到文本文件 （在此示例中名为 psoutput.txt），运行：

  `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name | Out-File C:\Users\<your account>\psoutput.txt`

3. 添加并提交所有更改，将它们推送到分支，并打开拉取请求。 有关说明，请参阅[Git 命令，以便创建或更新文章](git-steps-create-update-content.md)。

## <a name="step-3-update-fwlinks"></a>步骤 3:更新 Fwlink

检查有任何指向文章的 Fwlink FWLink 工具。 任何 Fwlink 指向替换内容;如果您不拥有该链接在别名上，请将其联接。 如果所有者不会更新该链接，向提交票证 MSCOM 更改该链接。 详细信息-[内部 wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Manage%20inbound%20links%20to%20retired%20topics.aspx)。

## <a name="step-4-remove-crosslinks-to-the-article-from-table-of-contents"></a>步骤 4：删除交叉链接到文章的内容的表中

适用于维护 ToC.md 文件的人员。 此文件填充的表左侧的技术库中的内容。 如果不知道要联系谁，发送电子邮件至wssc-pra@microsoft.com。

## <a name="step-5-add-redirects"></a>步骤 5：添加重定向
如果您要重命名或删除文件，将重定向，以便不会破坏现有的链接：

1. 将旧文件保留在现有位置与现有的文件名称。
2. 将此元数据块替换为文件中的内容：
   ```
   ---
   redirect_url: <redirection-URL-or-file>
   ---
   ```
   \<重定向 URL 或文件 > 是到其他位置的完整 URL 或路径 + 文件名相同 OPS 存储库中的其他主题。

   例如-这将是整个文件：

   ```
   ---
   redirect_url: ../../failover-clustering/whats-new-in-failover-clustering
   ---
   ```

3. 在创建重定向，单击 PR-如果重定向你的注释中的链接会转到目标主题的拉取请求。

## <a name="step-6-rename-or-delete-the-article"></a>步骤 6：重命名或删除项目

如果不使用重定向，你已完成前面的步骤和所有受影响的文章发布后执行此步骤。 如果你将重定向，重命名或删除项目将撤消此和会导致断开的链接。 若要重命名文件，只需将其重命名在文件系统中，然后添加、 提交和推送更改，并打开拉取请求。
若要删除的文件，您首先需要知道，它不起作用只是从文件系统删除文件，因为这是源代码管理系统，它需要知道您用于执行此操作。 否则你已删除的文件可能会重新出现。
有两种方法来执行此操作：

- 文件系统和 git:从文件系统中删除文件。 然后从 git 工具，运行以下项之一  ```Git add -A``` | ```Git add --all``` | ```Git add -u```
- 只是 git: 运行 ```git rm foo.md```

    详细信息[ http://stackoverflow.com/questions/2047465/how-can-i-delete-a-file-from-git-repo ](http://stackoverflow.com/questions/2047465/how-can-i-delete-a-file-from-git-repo)和 [https://git-scm.com/docs/git-rm](https://git-scm.com/docs/git-rm) 

## <a name="step-7-find-and-fix-straggler-broken-links"></a>步骤 7：查找并修复 straggler 断开的链接

使用内容的 QA 工具查找前面的步骤没有捕获，然后删除或修复链接断开的链接。

## <a name="step-8-remove-cached-pages-from-search-engines"></a>步骤 8：从搜索引擎中删除的缓存的页

请转到这些 web 页面以从搜索引擎中删除缓存的网页：[Bing](https://www.bing.com/webmaster/tools/content-removal?rflid=1)
[Google](https://www.google.com/webmasters/tools/removals?pli=1)


### <a name="contributors-guide-links"></a>参与者的指南的链接

- [指南文章的索引](./contributor-guide-index.md)

