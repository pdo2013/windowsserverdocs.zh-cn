# <a name="contributing-to-windows-server-technical-documentation"></a>参与 Windows Server 技术文档

感谢您对 Windows Server 技术文档的关注 ！ 感谢你向我们的文档提供反馈以及编辑和添加内容。有两个不同的位置，我们在其中保存 Windows Server 技术内容。 位置之一是公共 (windowsserverdocs)，而另一个是私钥 (windowsserverdocs pr)。 你是谁确定你参与到哪个位置：

- **我并不是 Microsoft 员工。** 作为非 Microsoft 员工，你必须参与到公共位置。 有关如何执行该操作，继续阅读本文。

- **我是一名 Microsoft 员工。** 作为 Microsoft 员工，必须基于你想要做的选项：

    - **创建一个全新的文章。** 若要创建全新的文章，必须创建并设置 GitHub 帐户和工具，分叉和克隆 windowsserverdocs pr 存储库中，设置远程分支，创建项目，最后创建新的拉取请求进行审批和发布。 这些说明，请参阅[创建新的 Windows Server 文章使用 GitHub 和 Visual Studio Code](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/create-new-using-github.md)一文。

    - **对现有文章进行较大的更改。** 若要对现有文章进行重大更改，可以按照中的说明[编辑现有 Windows Server 文章使用 GitHub 和 Visual Studio Code](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/edit-existing-using-github.md)一文。

    - **对现有文章进行次要更改。** 若要对现有文章进行次要更改，可以按照中的说明[更新现有 Windows Server 工具中使用 web 浏览器和 GitHub 文章](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/github-browser-updates.md)一文。

## <a name="sign-a-cla"></a>签订 CLA

所有的参与者***不***是 Microsoft 员工必须[签署 Microsoft 贡献许可协议 (CLA)](https://cla.microsoft.com/)之前编辑任何 Microsoft 存储库。 如果已编辑 Microsoft 存储库中在过去，祝贺你 ！
已完成此步骤。

## <a name="editing-topics"></a>编辑主题

我们已尝试使编辑现有的公共文件一样简单。

### <a name="to-edit-a-topic"></a>若要编辑主题

1. 转到页面 https://docs.microsoft.com/windows-server想要更新，然后选择**编辑**。

    ![GitHub Web，显示编辑链接](media/contribute-link.png)

2. 登录到 （或注册） 的 GitHub 帐户。

    必须具有 GitHub 帐户才能访问使你能够编辑某个主题的页面。

3. 选择**铅笔**图标 （在红框） 要编辑的内容。

    ![GitHub Web，在红框中显示的铅笔图标](media/pencil-icon.png)

4. 使用 Markdown 语言，对主题进行更改。 要了解如何编辑使用 Markdown 的内容，请参阅：

    - **如果要链接到 GitHub 中的 Microsoft 组织：** [Windows Server 参与者指南](https://github.com/MicrosoftDocs/windowsserverdocs-pr/tree/master/Contributor-guide)

    - **如果你是 Microsoft 的外部：** [掌握 Markdown](https://guides.github.com/features/mastering-markdown/)

5. 使你建议的更改，然后选择**预览更改**以确保它正确无误。

    ![GitHub Web，显示预览更改选项卡](media/preview-changes.png)

6. 完成后编辑主题，滚动到页面底部，键入在分支的描述性名称，然后单击**建议文件更改**你个人的 GitHub 帐户中创建分支。

    ![GitHub Web，显示建议文件更改按钮](media/propose-file-change.png)

    **比较更改**屏幕显示以查看所做的更改是在分支之间的原始内容。

7. 上**比较更改**屏幕上，你将看到是否存在要签入文件的任何问题。

    如果没有任何问题，您会看到该消息，**能够合并**。

    ![GitHub Web 显示比较更改屏幕](media/compare-changes.png)

8. 选择**创建拉取请求**。

    拉取请求的让您告诉其他人已推送到存储库中的一个分支在 GitHub 的更改。 打开拉取请求后，可以讨论和查看与协作者可能的更改和添加后续提交所做的更改合并到基分支之前。 有关详细信息，请参阅[有关拉取请求](https://help.github.com/articles/about-pull-requests)

9. 输入标题和说明，用于提供有关什么是请求中相应的上下文的审批者。

10. 滚动到底部的页上，确保只有已更改的文件位于此拉取请求。 否则，你可能会覆盖从其他人的更改。

11. 选择**创建拉取请求**再次以便实际将提交拉取请求。

    拉取请求发送到主题的编写器并查看所做的编辑。 如果接受你的请求，则将发布更新。

## <a name="resources"></a>资源

- 可以使用你最喜欢的文本编辑器来编辑 Markdown。 我们建议[Visual Studio Code](https://code.visualstudio.com/)，来自 Microsoft 的轻量的免费开放源代码编辑器。