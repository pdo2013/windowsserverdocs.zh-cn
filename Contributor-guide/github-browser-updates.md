---
title: 编辑现有 Windows Server 工具中使用 web 浏览器和 GitHub 文章
description: 如何使用 web 浏览器和 GitHub，作为 Microsoft 员工的现有 Windows Server 文档中进行快速编辑。
author: eross-msft
ms.author: lizross
ms.date: 05/02/2019
ms.openlocfilehash: d4574de7774a43092815a3d154020559c50fdcf9
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624597"
---
# <a name="update-existing-windows-server-articles-using-a-web-browser-and-github"></a>更新现有 Windows Server 工具中使用 web 浏览器和 GitHub 文章

有两个不同的位置，我们在其中保存 Windows Server 技术内容。 位置之一是公共 (windowsserverdocs)，而另一个是私钥 (windowsserverdocs pr)。 你是谁确定你参与到哪个位置：

- **我并不是 Microsoft 员工。** 作为非 Microsoft 员工，你必须参与到公共位置。 有关如何执行该操作的信息，请参阅[Contributing.md](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md)文件。

- **我是一名 Microsoft 员工。** 作为 Microsoft 员工，必须基于你想要做的选项：

    - **创建一个全新的文章。** 若要创建全新的文章，必须创建并设置 GitHub 帐户和工具，分叉和克隆 windowsserverdocs pr 存储库中，设置远程分支，创建项目，最后创建新的拉取请求进行审批和发布。 这些说明，请参阅[创建新的 Windows Server 文章使用 GitHub 和 Visual Studio Code](create-new-using-github.md)一文。

    - **对现有文章进行较大的更改。** 若要对现有文章进行重大更改，可以按照中的说明[编辑现有 Windows Server 文章使用 GitHub 和 Visual Studio Code](edit-existing-using-github.md)一文。

    - **对现有文章进行次要更改。** 若要对现有文章进行次要更改，可以按照这篇文章中的说明。

    > [!IMPORTANT]
    > 发布到 docs.microsoft.com 的所有存储库均遵循[Microsoft 开放源代码行为准则](https://opensource.microsoft.com/codeofconduct/)或[.NET 基础行为准则](https://dotnetfoundation.org/code-of-conduct)。 有关详细信息，请参阅[代码的行为准则常见问题](https://opensource.microsoft.com/codeofconduct/faq/)。 或联系[ opencode@microsoft.com ](mailto:opencode@microsoft.com)，或[ conduct@dotnetfoundation.org ](mailto:conduct@dotnetfoundation.org)有任何疑问或意见。
    >
    > 包含在较小的更改和澄清到公共存储库中的文档和代码示例[docs.microsoft.com 使用条款](https://docs.microsoft.com/legal/termsofuse)。 新的或重大更改生成一条注释在拉取请求中，要求提交联机的贡献许可协议 (CLA)，如果您不是 Microsoft 的员工。 我们需要你之前在我们评审或接受拉取请求完成联机窗体。

## <a name="quick-edits-to-existing-articles-using-github-and-a-web-browser"></a>将对现有文章使用 GitHub 和 web 浏览器快速编辑

快速编辑可简化报告和修复文档中的小错误和遗漏的过程。 尽管所有工作，小的语法和拼写错误_执行_会全面我们发布的文档。

1. 转到 https://github.com/MicrosoftDocs/windowsserverdocs-pr/tree/master/WindowsServerDocs。

2. 导航到想要编辑的文章，然后选择**编辑此文件**按钮。

   ![编辑此文件按钮](media/github-browser-updates/edit-this-file.png)

3. 编辑主题，然后向下滚动到底部，简要介绍所做的更改，并选择**提交更改**。

    ![提交的更改与标题信息](media/github-browser-updates/commit-changes.png)

## <a name="submit-the-pull-request"></a>提交拉取请求

创建你的拉取请求后，必须将它提交进行审批和发布。

### <a name="to-submit-your-pull-request"></a>若要提交你的拉取请求

1. 上**打开拉取请求**页上，更新你的提交消息，以使其更适合 pr。 例如： 在第一个段落中修复拼写错误。

2. 请确保只提交和预期要包含的文件包含。 此外拉取请求 （通常） 转到正确的分支上游存储库，任一母版中的检查，或者发布分支 （有时）。

3. 在中**审阅者**区域中的右窗格中，选择齿轮图标，然后输入_windowsservercontent_。 别名的成员将以查看所做的更改的点上，并且可以合并你的拉取请求或添加要在合并前更改的事项的注释。

4. 选择**创建拉取请求**。 新的拉取请求链接到工作分支中的分支。 合并拉取请求，直到你推送到同一个工作分支分支中的任何新提交将自动包含在 pr。 新标签添加到你的拉取请求，显示**执行合并**。 这只意味着你的内容仍在进行中和不应进行检查或推送到实时站点。

5. 准备好进行的某成员别名以查看你的内容时，必须添加文本 **#sign 认可**的注释。 此注释：

    - 更新你的拉取请求中的标签**执行合并**到**就绪到合并**。

    - 允许的别名和编写器知道您是准备好你查看的内容。

    - 允许管理员知道，批准后，你的内容是做好准备工作实时。