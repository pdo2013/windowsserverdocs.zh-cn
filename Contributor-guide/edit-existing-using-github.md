---
title: 使用 GitHub 和 Visual Studio Code 编辑现有 Windows Server 文章
description: 如何使用 GitHub 和 Visual Studio Code 编辑与 Microsoft 员工相同的现有 Windows Server 相关文章。
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: d681d2fc2b69898e841932a95738b89515ffb51a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865078"
---
# <a name="edit-an-existing-windows-server-article-using-github-and-visual-studio-code"></a>使用 GitHub 和 Visual Studio Code 编辑现有 Windows Server 文章

这里有两个不同的位置，即保留 Windows Server 技术内容。 其中一个位置是公共的（windowsserverdocs），而另一个是专用的（windowsserverdocs）。 你要确定你参与的位置：

- **我是 Microsoft 员工。** 作为一名 Microsoft 员工，你可以选择基于你要执行的操作：

    - **创建全新的文章。** 若要创建全新的项目，必须创建并设置 GitHub 帐户和工具、分支和克隆 windowsserverdocs 存储库，设置远程分支，创建文章，最后创建新的拉取请求以批准和发布。 对于这些说明，你可以按照[使用 GitHub 创建新的 Windows Server 文章和 Visual Studio Code](create-new-using-github.md)一文中的说明进行操作。

    - **对现有文章进行大更改。** 若要对现有文章进行重大更改，可以按照本文中的说明进行操作。

    - **对现有项目进行少量更改。** 若要对现有文章进行少量更改，你可以按照[使用 web 浏览器和 GitHub 更新现有 Windows Server 文章](github-browser-updates.md)中的说明进行操作。

- **我不是 Microsoft 的员工。** 作为非 Microsoft 员工，你必须参与到公共位置。 有关如何执行此操作的信息，请参阅[Windows Server 相关技术文档](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md)文章。

## <a name="switch-your-repo-and-create-a-new-branch"></a>切换存储库并创建新分支

按照以下步骤编辑现有项目。

### <a name="create-a-new-branch-and-locate-the-file-you-want-to-update"></a>创建新分支并找到要更新的文件

必须先更改 windowsserverdocs 存储库，然后查找要更新的项目，然后才能开始使用内容。

#### <a name="to-create-a-new-branch-in-git-bash"></a>在 Git Bash 中创建新分支

- 打开 Git Bash 并键入命令（一次一个）：

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >我们强烈建议将分支命名为明显且唯一，以便以后可以再次查找。

    命令完成后，你将进入你的新分支，并已准备好编辑你的文件。

#### <a name="to-locate-your-article-and-make-your-edits"></a>查找文章并进行编辑

1. 打开 Visual Studio Code 并依次单击 "**文件**"、"**打开文件夹**"，然后前往包含要编辑的项目的文件夹的 GitHub 位置。

2. 从 "**资源管理器**" 窗格中，选择文件。

3. 更新页面上的信息，然后选择 "**文件** > " "**保存**"。

### <a name="preview-your-text"></a>预览文本

更新文本后，必须预览更改，以确保它们正确显示。

#### <a name="to-preview-your-text"></a>预览文本

1. 在 Visual Studio Code 中，选择右上角的任一**预览**按钮。

    ![预览按钮图标](media/create-new-using-github/preview-button-full-page.png):切换到内容的完整页面预览。

    ![预览按钮图标](media/create-new-using-github/preview-button-side-by-side.png):并行打开您的工作页面旁的预览页面。

2. 请确保您的文章看起来是您预期的。

    在您确定它的外观后，您可以提交您的更改并为发布创建拉取请求。

### <a name="commit-your-changes"></a>提交更改

确保文本显示正确后，可以将所做的更改提交到存储库的本地版本。

#### <a name="to-commit-your-changes"></a>提交更改

- 打开 Git Bash 并键入命令（一次一个，删除可选标记）：

    ```markdown
    OPTIONAL: git status

    git add .

    git commit -m “public comment about what change is”

    OPTIONAL: git pull upstream master

    git push origin <name_of_your_new_branch>

    ```

    可选的 git 状态命令显示已在此提交中修改了哪些文件。 可选的 git 拉上游主节点会从 MicrosoftDocs 主分支中拉伸最新的内容更改，并将本地内容与主要的主内容同步。 这可帮助你提前显示任何潜在的合并冲突，以便你可以在转到 PR 阶段之前修复它们。

### <a name="submit-a-pull-request-for-review-and-publication"></a>提交拉取请求以便查看和发布

完成更新后，必须从编写器中获得批准（留出一些时间）进行发布。

#### <a name="to-submit-your-pull-request"></a>提交拉取请求

1. 中转到 https://github.com/MicrosoftDocs/windowsserverdocs-pr 并选择 "**拉取请求**" 选项卡。

2. 在右窗格的 "**审阅者**" 区域中，选择齿轮图标，然后输入要查看的_windowsservercontent_别名。

    _Windowsservercontent_别名的成员将查看你的更改，或添加有关在进行合并之前必须更改的项的注释。

3. 在注释中键入 " **#sign** "，以便审阅者可以在查看和发布时进行提交。 " **#Sign** " 注释：

    - 将拉取请求的标签从 "**不合并**" 更新为 "**准备合并**"。

    - 允许别名和编写人员知道您已准备好查看内容。

    - 让管理员知道在批准后，你的内容已准备就绪。

    >[!Important]
    >添加 #sign 注释后，windowsservercontent 团队的成员将查看文本，并将其推送到 master，以便它将在下一个计划的发布到 live （10：上午9:30 和3：下午6:30 工作日）时出现。
    >
    >如果你不将 #sign 作为最终备注添加到你的 PR，你的内容将保留在队列中，而不会被推送到主节点并最终投入使用。

## <a name="related-information"></a>相关信息

有关 GitHub 和 markdown 语言的详细信息，请参阅：

### <a name="git-concepts"></a>Git 概念

- [GitHub 指南-Git 手册简介](https://guides.github.com/introduction/git-handbook/)

- [GitHub 指南-分叉项目](https://guides.github.com/activities/forking/)

- [GitHub 指南-了解 GitHub 流](https://guides.github.com/introduction/flow/)

- [了解 Git 分支](https://learngitbranching.js.org/ (非常适合视觉对象学习！))

### <a name="markdown"></a>Markdown

- [我们的内部 markdown 指南](https://review.docs.microsoft.com/help/contribute/markdown-reference?branch=master)

- [外部，GitHub 教程](https://www.markdowntutorial.com/)