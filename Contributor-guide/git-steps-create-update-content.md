<properties pageTitle="Git 命令可用于创建新文章或更新现有项目" description="创建和更新 WindowsServerDocs pr 中的项目的步骤。" metaKeywords="" services="" solutions="" documentationCenter="" authors="Kathy Davies" videoId="" scriptId="" manager="dongill" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="08/24/16" ms.author="kathydav" />

# <a name="git-commands-to-create-or-update-an-article"></a>若要创建或更新项目的 Git 命令

>!注意：这些命令假设你已配置 Github，以指定默认存储库中的文件中提取。 在 Github 术语中，请求中的文件的位置是你*上游*。 推送到的文件的位置是你*原点*。 根据我们的存储库和工作流的设计方式，在上游应设置为我们的存储库，这是在 Microsoft 的组织-下 https://github.com/Microsoft/WindowsServerDocs-pr和源应为你自己的 Github 帐户下此存储库的分叉。 例如，Kathy 的是 https://github.com/KBDAzure/WindowsServerDocs-pr 

>若要检查你的设置，请键入```git config -l```。 查看以确保它们引用你预期的位置的 Url。

## <a name="add-or-update-an-article"></a>添加或更新项目

下面介绍了如何创建一个本地分支，保存所做的更改，并将其推送到远程分支。

1. 启动 Git Bash （或对于 Git 使用的命令行工具）。

2. 将更改为 WindowsServerDocs 拉取请求：

        cd WindowsServerDocs-pr

3. 它是让你的本地主分支和远程主分支执行分支操作保持同步与存储库的 Master 分支中。 这可帮助节省你大量不满意的程度和丢失时间--您更有可能及早发现问题并保持在正常且已知的工作状态。 运行：

        git checkout master
        git pull upstream master
        git push origin master

4. 现在您已准备好创建一个分支以执行你每日或基于可交付结果的工作。 从发布分支创建一个本地工作分支的最佳方法是使用运行：

        git checkout upstream/upstream-branch-name -b your-local-branch-name

   这将从上游分支直接创建本地分支并可帮助您避免将错误文件合并到新本地分支。 例如，若要创建一个根据 ga 阈值分支的工作分支，无法运行如下命令：
      
        git checkout upstream/master -b working-11-18

   如果您正在处理应合并到不是分支的内容         

5. 将本地工作分支添加到分支：

        git push origin <working branch>

6. 创建新项目或更改现有项目。 使用 Windows 资源管理器中打开 markdown 文件和 markdown 编辑器来创建和编辑文件。 有关基本格式设置的帮助，请参阅此[一文](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/)在 Github 中。

7. 添加必需的元数据和版本信息，根据[元数据和产品版本控制](metadata-OSversioning-and-trademarks.md)。

8. 检查状态，然后添加并提交所做的更改：

        git status

   查看结果，以确保列出的文件的更改。 如果所有文件看都起来正确，运行以下命令来添加的所有文件：

        git add .
        git commit –m "<comment>"

   若要添加仅将特定文件 (例如，如果```git status```列出了不想要提交的文件)，而是必须运行：

        git add <file path>
        git commit –m "<comment>"

   >[!IMPORTANT]
   >该命令```git add .```将添加所有挂起的更改报告的```git status```。 这意味着，如果```git status```中显示不想要添加，请使用未受跟踪的更新```git add <file path>```相反。  

9. 使用从上游的更改更新本地工作分支：

        git pull upstream master

10. GitHub 上将更改推送到分支：

        git push origin <working branch>

## <a name="submit-your-changes"></a>提交所做的更改

如果你已准备好提交暂存、 验证和/或发布你的内容，使用 GitHub UI 创建拉取请求。 

打开拉取请求 (PR) 时，这将触发测试流程、 生成项目并将发布到我们内部的临时站点。 它是可以打开尚未准备好已合并，因为这是如何获取测试流程并检查你的暂存站点上的更新的拉取请求。 生成详细信息和暂存链接以注释的形式发布到 pr。 

它负责执行以下**提问，使合并更改之前先**:
  - 查看生成详细信息，以确保它不包含任何错误。 
  - 查看你的暂存站点上的更新。

完成此操作后，指示它通过以下任一方法：
- 将"准备合并"标签添加到 pr。 \(单击**标签**或在 PR 中的评论流右侧的齿轮图标)
- 添加已准备合并为一个注释，并将电子邮件发送到 WSSC 请求审阅者别名： wssc pra

拉取请求审阅者检查所做的更改，并接受拉取请求，除非有问题或疑问。 作为注释添加反馈或请求来解决问题。 查看[质量标准的拉取请求评审](contributor-guide-pr-criteria.md)若要了解预期。

## <a name="publishing"></a>Publishing

- 文章发布大约上午 10:00 和下午 3:00 点太平洋时间星期一至星期五。 请记住，拉取请求审阅者需要时间来查看并接受所做的更改，然后才可以合并。 必须合并的更改提取下一个计划的发布周期中。 如果您需要为特定发布周期发表的一篇，让拉取请求审阅者提前得知。 接受你的拉取请求后，所做的更改合并到存储库，并将包含在下一次运行的时间发布。 它可能需要 30 分钟，文章发布后出现在网上。 
