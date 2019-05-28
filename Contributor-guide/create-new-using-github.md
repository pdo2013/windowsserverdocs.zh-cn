---
title: 创建新的 Windows Server 文章使用 GitHub 和 Visual Studio Code
description: 如何创建新 Windows Server 相关的项目，使用 GitHub 和 Visual Studio Code 中，作为 Microsoft 员工。
author: eross-msft
ms.author: lizross
ms.date: 05/02/2019
ms.openlocfilehash: d75bf135266a4783ab2617977b344782cea679ef
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624563"
---
# <a name="create-new-windows-server-articles-using-github-and-visual-studio-code"></a>创建新的 Windows Server 文章使用 GitHub 和 Visual Studio Code

有两个不同的位置，我们在其中保存 Windows Server 技术内容。 位置之一是公共 (windowsserverdocs)，而另一个是私钥 (windowsserverdocs pr)。 你是谁确定你参与到哪个位置：

- **我是一名 Microsoft 员工。** 作为 Microsoft 员工，必须基于你想要做的选项：

    - **创建一个全新的文章。** 若要创建全新的文章，必须创建并设置 GitHub 帐户和工具，分叉和克隆 windowsserverdocs pr 存储库中，设置远程分支，创建项目，最后创建新的拉取请求进行审批和发布。 对于这些说明，请继续阅读本文。

    - **对现有文章进行较大的更改。** 若要对现有文章进行重大更改，可以按照中的说明[编辑现有 Windows Server 文章使用 GitHub 和 Visual Studio Code](edit-existing-using-github.md)一文。

    - **对现有文章进行次要更改。** 若要对现有文章进行次要更改，可以按照中的说明[更新现有 Windows Server 工具中使用 web 浏览器和 GitHub 文章](github-browser-updates.md)一文。

- **我并不是 Microsoft 员工。** 作为非 Microsoft 员工，你必须参与到公共位置。 有关如何执行该操作的信息，请参阅[的 Windows Server 技术文档](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md)一文。

## <a name="prerequisites"></a>系统必备

在可以开始使用存储库之前，必须创建设置 GitHub 帐户、 设置双重验证，并安装和配置所有必需的工具。 如果已执行此操作，则可以向下跳过[分叉存储库部分](#fork-the-repository)的这篇文章。

1. [创建 GitHub 帐户和配置文件](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#create-a-github-account-and-set-up-your-profile)

2. [将帐户链接到你的 Microsoft 帐户和 Microsoft 或 MicrosoftDocs 组织](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#link-your-github-and-microsoft-accounts)

3. [启用双重验证](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#enable-two-factor-authentication-and-create-an-access-token)

4. [授权生成系统访问 GitHub 帐户](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#authorize-the-ops-build-system-to-access-your-github-account)

5. [安装 Visual Studio Code](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-visual-studio-code)

6. [安装 GitHub 和其工具](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-git-client-tools)

7. [安装 Docs 创作包](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-the-docs-authoring-pack)

## <a name="set-up-your-own-version-of-the-repo"></a>设置存储库的版本

已创建并设置了你的 GitHub 帐户和工具后，可以创建你的存储库的个性化版本。 这是将在其中创建分支和进行所有更改。

### <a name="fork-the-repository"></a>分叉存储库

您需要的源文件的本地副本，因此可以到生产存储库从分叉创建拉取请求。

#### <a name="to-fork-the-repository"></a>若要创建存储库分支

1. 登录到你的 GitHub 帐户，请转到 https://github.com/microsoftdocs/windowsserverdocs-pr。

2. 选择**分叉**。

    ![分支页上突出显示的按钮](media/create-new-using-github/fork-button.png)

3. 选择 GitHub 帐户作为分支位置。

    ![分支页上突出显示的按钮](media/create-new-using-github/fork-location.png)

### <a name="clone-the-repository"></a>克隆存储库

您需要克隆存储库获取到本地设备的存储库的本地副本。

#### <a name="to-clone-the-repository"></a>若要克隆存储库

1. 转到 https://github.com/settings/developers，然后选择**个人访问令牌**在左窗格中。

2. 选择**生成新令牌**，为你的令牌提供一个有意义且唯一的名称，选择所有的可用作用域，并选择**生成令牌**。

3. 复制令牌并将其放到某个安全位置。 将需要此过程的其余部分并离开页面后，你无法再以返回到它。

4. 打开 Git Bash 命令，并将目录更改为要存储在存储库。 我们建议使用， `C:\users\<your_name>\GitHub`。

5. 键入以下命令使用您的特定信息，一次一个地克隆存储库并设置远程分支：

    ```markdown

    git clone https://<your_github_username>:<your_personal_access_token>@github.com/<your_github_username>/windowsserverdocs-pr.git

    cd windowsserverdocs-pr

    git remote add upstream https://<your_github_username>:<your_personal_access_token>@github.com/MicrosoftDocs/windowsserverdocs-pr.git

    git fetch upstream master
    ```

6. 运行以下命令以确保您的远程已正确设置：

    `git remote -v`

7. 应看到此输出类似：

    ```markdown
    $ git remote -v

    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (fetch)
    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (push)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (fetch)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (push)
    ```

    如果你远程输出不会如下所示，可以尝试再次通过第一个运行`git remote remove upstream`。

## <a name="create-a-branch-and-a-new-article"></a>创建一个分支和新文章

请按照下列步骤创建项目。

### <a name="create-a-new-branch-and-a-new-file"></a>创建一个新的分支和一个新文件

你可以开始工作对你的内容之前，必须在本地存储库中创建一个新的分支。

#### <a name="to-create-a-new-branch-in-git-bash"></a>若要创建新分支在 Git Bash

- 打开 Git Bash，并键入的命令 （一次一个）：

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >我们强烈建议命名你的分支是一些明显、 唯一以便稍后可以找到它。

    命令完成后，你将新分支中即可，并创建新文件。 只需更改到 windowsserverdocs pr 存储库一次每个实例的 Git Bash。 如果关闭 Git Bash，需要将目录更改再次打开它之后。

#### <a name="to-create-a-new-file-in-your-branch"></a>若要在你的分支中创建一个新的文件

1. 打开 Windows 资源管理器，转到 GitHub 目录，并在文件夹结构中创建新的文本文件。 你的文件名称必须全部小写字母和连字符分隔。 例如，_模拟的是-windows-server.md_。

     此外必须更改文件扩展名为.txt 从。 md。 在出现的错误消息，选择**是**若要使用新的文件扩展名保存该文件。

2. 打开 Visual Studio Code 并转到**文件**，选择**打开文件夹**，，然后转到在步骤 1 中创建的文件的 GitHub 位置。

3. 从**资源管理器**窗格中，选择你的新文件。

4. 将文本添加到页上，然后依次**文件** > **保存**。

### <a name="preview-your-text"></a>预览文本

将文本添加到新文件后，您必须预览所做更改以确保它们正确显示。

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

完成文章后，您必须从您的写入器获取批准 （留出一些时间） 的发布。

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
