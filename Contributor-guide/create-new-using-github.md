---
title: 使用 GitHub 和 Visual Studio Code 创建新的 Windows Server 文章
description: 如何使用 GitHub 和 Visual Studio Code 创建与 Microsoft 员工相同的新的 Windows Server 相关文章。
author: eross-msft
ms.author: lizross
ms.date: 05/02/2019
ms.openlocfilehash: 3f09c36c1e3960728ff016f5801deb854e3d3c96
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865067"
---
# <a name="create-new-windows-server-articles-using-github-and-visual-studio-code"></a>使用 GitHub 和 Visual Studio Code 创建新的 Windows Server 文章

这里有两个不同的位置，即保留 Windows Server 技术内容。 其中一个位置是公共的（windowsserverdocs），而另一个是专用的（windowsserverdocs）。 你要确定你参与的位置：

- **我是 Microsoft 员工。** 作为一名 Microsoft 员工，你可以选择基于你要执行的操作：

    - **创建全新的文章。** 若要创建全新的项目，必须创建并设置 GitHub 帐户和工具、分支和克隆 windowsserverdocs 存储库，设置远程分支，创建文章，最后创建新的拉取请求以批准和发布。 有关这些说明，请继续阅读本文。

    - **对现有文章进行大更改。** 若要对现有文章进行重大更改，你可以按照[使用 GitHub 和 Visual Studio Code 文章编辑现有 Windows Server 一文](edit-existing-using-github.md)中的说明进行操作。

    - **对现有项目进行少量更改。** 若要对现有文章进行少量更改，你可以按照[使用 web 浏览器和 GitHub 更新现有 Windows Server 文章](github-browser-updates.md)中的说明进行操作。

- **我不是 Microsoft 的员工。** 作为非 Microsoft 员工，你必须参与到公共位置。 有关如何执行此操作的信息，请参阅[Windows Server 相关技术文档](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md)文章。

## <a name="prerequisites"></a>先决条件

在开始使用存储库之前，必须先创建并设置 GitHub 帐户、设置双因素验证，并安装和配置所有必需的工具。 如果已完成此操作，则可以跳转到本文的 "[分叉" 部分](#fork-the-repository)。

1. [创建 GitHub 帐户和配置文件](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#create-a-github-account-and-set-up-your-profile)

2. [将你的帐户链接到你的 Microsoft 帐户和 Microsoft 和 MicrosoftDocs 组织](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#link-your-github-and-microsoft-accounts)

3. [启用双重验证](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#enable-two-factor-authentication-and-create-an-access-token)

4. [授权生成系统访问你的 GitHub 帐户](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#authorize-the-ops-build-system-to-access-your-github-account)

5. [安装 Visual Studio Code](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-visual-studio-code)

6. [安装 GitHub 及其工具](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-git-client-tools)

7. [安装文档创作包](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-the-docs-authoring-pack)

## <a name="set-up-your-own-version-of-the-repo"></a>设置自己的存储库版本

创建并设置 GitHub 帐户和工具后，可以创建存储库的个人版本。 你将在此处创建分支并进行所有更改。

### <a name="fork-the-repository"></a>分叉存储库

你需要源文件的本地副本，因此你可以创建从分支到生产存储库的拉取请求。

#### <a name="to-fork-the-repository"></a>分叉存储库

1. 登录到 GitHub 帐户，并前往 https://github.com/microsoftdocs/windowsserverdocs-pr 。

2. 选择 "**分叉**"。

    ![页面上突出显示的分叉按钮](media/create-new-using-github/fork-button.png)

3. 选择 GitHub 帐户作为分叉位置。

    ![页面上突出显示的分叉按钮](media/create-new-using-github/fork-location.png)

### <a name="clone-the-repository"></a>克隆存储库

需要在本地设备上克隆存储库的本地副本。

#### <a name="to-clone-the-repository"></a>克隆存储库

1. 选择 "**访问**" ，然后从左窗格中选择"个人访问令牌 https://github.com/settings/developers "。

2. 选择 "**生成新令牌**"，为令牌提供有意义且唯一的名称，选择 "所有可用作用域"，然后选择 "**生成令牌**"。

3. 复制该令牌并将其放在一个安全的地方。 在此过程的其余部分将需要此操作，离开页面后，将无法返回。

4. 打开 Git Bash 命令，并将目录更改为要存储存储库的位置。 建议使用`C:\users\<your_name>\GitHub`。

5. 使用你的特定信息一次键入以下命令，克隆存储库并设置远程分支：

    ```markdown

    git clone https://<your_github_username>:<your_personal_access_token>@github.com/<your_github_username>/windowsserverdocs-pr.git

    cd windowsserverdocs-pr

    git remote add upstream https://<your_github_username>:<your_personal_access_token>@github.com/MicrosoftDocs/windowsserverdocs-pr.git

    git fetch upstream master
    ```

6. 运行此命令以确保正确设置远程：

    `git remote -v`

7. 应会看到类似于以下输出的内容：

    ```markdown
    $ git remote -v

    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (fetch)
    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (push)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (fetch)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (push)
    ```

    如果你的远程输出与此类似，你可以在首次运行`git remote remove upstream`时重试。

## <a name="create-a-branch-and-a-new-article"></a>创建分支和新文章

按照以下步骤创建项目。

### <a name="create-a-new-branch-and-a-new-file"></a>创建新分支和新文件

你必须在本地存储库中创建一个新分支，然后才能开始处理你的内容。

#### <a name="to-create-a-new-branch-in-git-bash"></a>在 Git Bash 中创建新分支

- 打开 Git Bash 并键入命令（一次一个）：

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >我们强烈建议将分支命名为明显且唯一，以便以后可以再次查找。

    命令完成后，你将进入新分支，并准备好创建新文件。 只需将每个 Git Bash 实例的 windowsserverdocs 存储库更改为一次。 如果关闭 Git Bash，则需要在打开后再次更改目录。

#### <a name="to-create-a-new-file-in-your-branch"></a>在分支中创建新文件

1. 打开 Windows 资源管理器，前往 GitHub 目录，并在文件夹结构中创建新的文本文件。 文件名必须全部小写并由连字符分隔。 例如， _what-is-windows-server.md_。

     还必须将文件扩展名从 .txt 更改为。 在出现的错误消息中，选择 **"是"** 以用新文件扩展名保存文件。

2. 打开 Visual Studio Code 并依次单击 "**文件**"、"**打开文件夹**"，然后跳到在步骤1中创建的文件的 GitHub 位置。

3. 从 "**资源管理器**" 窗格中，选择新文件。

4. 将文本添加到页面。 如果要创建新项目，请确保[将所需的元数据标记添加到与 Windows Server 相关的文章](metadata-requirements-for-articles.md)中。

5. 选择 "**文件** > " "**保存**"。

### <a name="preview-your-text"></a>预览文本

向新文件添加文本后，必须预览更改，以确保它们正确显示。

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

完成文章后，你必须从编写者处获得批准（对于此，留出一段时间）进行发布。

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
