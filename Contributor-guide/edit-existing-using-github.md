---
title: 编辑现有 Windows Server 文章使用 GitHub 和 Visual Studio Code
description: 如何编辑现有 Windows Server 相关项目，使用 GitHub 和 Visual Studio Code 中，作为 Microsoft 员工。
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: d2d95cc28089ceb74bf9690f6bd78611e7d9a27a
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624585"
---
# <a name="edit-an-existing-windows-server-article-using-github-and-visual-studio-code"></a>编辑现有 Windows Server 文章使用 GitHub 和 Visual Studio Code

有两个不同的位置，我们在其中保存 Windows Server 技术内容。 位置之一是公共 (windowsserverdocs)，而另一个是私钥 (windowsserverdocs pr)。 你是谁确定你参与到哪个位置：

- **我是一名 Microsoft 员工。** 作为 Microsoft 员工，必须基于你想要做的选项：

    - **创建一个全新的文章。** 若要创建全新的文章，必须创建并设置 GitHub 帐户和工具，分叉和克隆 windowsserverdocs pr 存储库中，设置远程分支，创建项目，最后创建新的拉取请求进行审批和发布。 有关这些说明中，可以按照中的说明[创建新的 Windows Server 文章使用 GitHub 和 Visual Studio Code](create-new-using-github.md)一文。

    - **对现有文章进行较大的更改。** 若要对现有文章进行重大更改，可以按照这篇文章中的说明。

    - **对现有文章进行次要更改。** 若要对现有文章进行次要更改，可以按照中的说明[更新现有 Windows Server 工具中使用 web 浏览器和 GitHub 文章](github-browser-updates.md)一文。

- **我并不是 Microsoft 员工。** 作为非 Microsoft 员工，你必须参与到公共位置。 有关如何执行该操作的信息，请参阅[的 Windows Server 技术文档](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md)一文。

## <a name="switch-your-repo-and-create-a-new-branch"></a>切换存储库并创建一个新的分支

请按照下列步骤以编辑现有的项目。

### <a name="create-a-new-branch-and-locate-the-file-you-want-to-update"></a>创建一个新分支并找到你想要更新的文件

你可以开始工作对你的内容之前，你必须先切换到 windowsserverdocs pr 存储库，然后找到你想要更新的文章。

#### <a name="to-create-a-new-branch-in-git-bash"></a>若要创建新分支在 Git Bash

- 打开 Git Bash，并键入的命令 （一次一个）：

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >我们强烈建议命名你的分支是一些明显、 唯一以便稍后可以找到它。

    命令完成后，你将新分支中即可，并编辑你的文件。

#### <a name="to-locate-your-article-and-make-your-edits"></a>若要查找您的文章并进行编辑

1. 打开 Visual Studio Code 并转到**文件**，选择**打开文件夹**，，然后转到具有你想要编辑的项目的文件夹的 GitHub 位置。

2. 从**资源管理器**窗格中，选择的文件。

3. 更新页面上的信息，然后选择**文件** > **保存**。

### <a name="preview-your-text"></a>预览文本

更新文本后，您必须预览所做更改以确保它们正确显示。

#### <a name="to-preview-your-text"></a>若要预览文本

1. 在 Visual Studio Code 中，选择任一**预览版**从右上角的按钮。

    ![预览按钮图标](media/create-new-using-github/preview-button-full-page.png)：切换到整页的内容预览。

    ![预览按钮图标](media/create-new-using-github/preview-button-side-by-side.png)：将打开你的工作页面上，通过并行旁边预览页。

2. 请确保您的文章看起来你期望它要查找的方式。

    您确定看起来正确之后，可以提交所做的更改和创建发布的拉取请求。

### <a name="commit-your-changes"></a>提交所做的更改

请确保您的文本看起来正确之后，您可以将更改提交到存储库的本地版本。

#### <a name="to-commit-your-changes"></a>若要提交所做的更改

- 打开 Git Bash，并键入的命令 （删除可选标记，一次一个）：

    ```markdown
    OPTIONAL: git status

    git add .

    git commit -m “public comment about what change is”

    OPTIONAL: git pull upstream master

    git push origin <name_of_your_new_branch>

    ```

    可选的 git 状态命令显示哪些文件已被修改作为此提交的一部分。 可选的 git 拉取上游 master 拉取下 MicrosoftDocs master 分支，同步本地内容与主要的主内容的最新内容更改。 这有助于使您可以修复它们，再到 PR 阶段获取显示提前任何潜在的合并冲突。

### <a name="submit-a-pull-request-for-review-and-publication"></a>提交拉取请求进行评审和发布

已完成更新后，您必须从您的写入器获取批准 （留出一些时间） 的发布。

#### <a name="to-submit-your-pull-request"></a>若要提交你的拉取请求

1. 转到 https://github.com/MicrosoftDocs/windowsserverdocs-pr，然后选择**拉取请求**选项卡。

2. 在中**审阅者**区域中的右窗格中，选择齿轮图标，然后输入_windowsservercontent_评审的别名。

    成员_windowsservercontent_别名将检查所做的更改或添加合并可以发生之前必须更改的内容的相关备注。

3. 类型 **#sign 认可**变成注释，以便审阅者知道提交进行审阅和发布。 **#Sign 认可**注释：

    - 更新你的拉取请求中的标签**执行合并**到**就绪到合并**。

    - 允许的别名和编写器知道您是准备好你查看的内容。

    - 允许管理员知道，批准后，你的内容是做好准备工作实时。

    >[!Important]
    >Windowsservercontent 团队的成员添加 #sign 认可注释后，将查看文本并将其来掌握以便它将使用下一个转推计划发布到实时 （上午 10:30 和工作日下午 3:30）。
    >
    >如果你未添加 #sign 认可为最后一个注释在拉取请求，你的内容将保留在队列中而无需推送到主分支，并最终生存。

## <a name="related-information"></a>相关的信息

有关 GitHub 和 markdown 语言的详细信息，请参阅：

### <a name="git-concepts"></a>Git 概念

- [GitHub 指南-Git 手册简介](https://guides.github.com/introduction/git-handbook/)

- [GitHub 指南分支项目](https://guides.github.com/activities/forking/)

- [GitHub 指南了解 GitHub 流](https://guides.github.com/introduction/flow/)

- [了解 Git 分支](https://learngitbranching.js.org/ (适合 visual 学习器 ！))

### <a name="markdown"></a>Markdown

- [我们的内部 markdown 指南](https://review.docs.microsoft.com/help/contribute/markdown-reference?branch=master)

- [外部，GitHub 教程](https://www.markdowntutorial.com/)