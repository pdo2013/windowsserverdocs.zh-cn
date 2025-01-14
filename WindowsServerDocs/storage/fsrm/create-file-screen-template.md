---
title: 创建文件屏蔽模板
description: 本文介绍如何创建文件屏蔽模板
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 63824f016180ce5a92d9a16b9ee0d26a46e5db72
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394207"
---
# <a name="create-a-file-screen-template"></a>创建文件屏蔽模板

> 适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2

*文件屏蔽模板*定义要屏蔽的一组文件组、要执行的屏蔽类型（主动或被动）以及在用户保存或尝试保存未授权文件时将自动生成的一组通知（可选）。

文件服务器资源管理器可发送电子邮件消息至管理员或特定用户、记录事件、执行命令或脚本，或者生成报告。 你可以为一个文件屏蔽事件配置多个通知类型。

如果完全通过模板创建文件屏蔽，则可以通过更新模板来集中管理文件屏蔽，而不必复制每个文件屏蔽中的更改。 此功能通过提供一个可进行所有更新的中心点，简化存储策略更改的实现。

> [!Important]
> 若要发送电子邮件通知和使用适用于服务器环境的参数配置存储报告，则必须首先设置文件服务器资源管理器常规选项。 有关详细信息，请参阅[设置文件服务器资源管理器选项](setting-file-server-resource-manager-options.md)。

## <a name="to-create-a-file-screen-template"></a>创建文件屏蔽模板

1.  在**文件屏蔽管理**中，单击**文件屏蔽模板**节点。

2.  右键单击**文件屏蔽模板**，然后单击**创建文件屏蔽模板**（或者从**操作**窗格中选择**创建文件屏蔽模板**）。 此操作将打开**创建文件屏蔽模板**对话框。

3.  如果要复制现有模板的属性以用作新模板基础，请从**从模板复制属性**下拉列表中选择模板，然后单击**复制**。

    无论你选择使用现有模板的属性，还是创建新模板，都应修改或设置“设置”选项卡上的以下值：

4.  在**模板名称**文本框中，输入新模板的名称。

5.  在**屏蔽类型**下，单击**主动屏蔽**或**被动屏蔽**选项。 （主动屏蔽会禁止用户保存被阻止的文件组中的文件，并且会在用户尝试保存未授权文件时生成通知。 被动屏蔽会发送配置的通知，但不会阻止用户保存文件）。

6.  若要指定要屏蔽的文件组：

    在**文件组**下，选择要包括的所有文件组。 （若要选中文件组复选框，请双击文件组标签。）

    若要查看文件组包含和排除的文件类型，请单击 "文件组" 标签，然后单击 " **编辑**"。 若要创建新的文件组，请单击 " **创建**"。

    此外，可以设置**电子邮件消息**、**事件日志**、**命令**和**报告**选项卡上的以下选项，从而将文件服务器资源管理器配置为生成一个或多个通知。

7.  若要配置电子邮件通知：

    在**电子邮件消息**选项卡上，设置以下选项：

    -   若要在用户或应用程序尝试保存未授权文件时通知管理员，请选中**将电子邮件发送至下列管理员**复选框，然后输入将收到通知的管理帐户名称。 请使用 *account*@*domain* 格式，并使用分号隔开多个帐户。
    -   若要发送电子邮件至尝试保存文件的用户，请选中**将电子邮件发送至尝试保存未经授权文件的用户**复选框。
    -   若要配置邮件，请编辑所提供的默认主题行和邮件正文。 方括号中的文本会插入关于导致通知的文件屏蔽事件的变量信息。 例如，@no__t 0**源 Io 所有者**\] 变量插入试图保存未经授权的文件的用户的名称。 若要在文本中插入其他变量，请单击**插入变量**。
    -   若要配置其他标题（包括发件人、抄送、密件抄送和回复），请单击**其他电子邮件标题**。

8.  若要在用户尝试保存未授权文件时将错误记录到事件日志中：

    在**事件日志**选项卡上，请选中**将警告发送至事件日志**复选框，然后编辑默认日志条目。

9.  若要在用户尝试保存未授权文件时运行命令或脚本：

    在**命令**选项卡上，请选中**运行该命令或脚本**复选框。 然后键入命令，或单击**浏览**以搜索脚本的存储位置。 还可以输入命令参数、选择命令或脚本的工作目录，或者修改命令安全设置。

10. 若要在用户尝试保存未授权文件时生成一个或多个存储报告：

    在**报告**选项卡上，请选中**生成报告**复选框，然后选择要生成的报告。 （可以为报告选择一个或多个管理电子邮件收件人，或者通过电子邮件将报告发送至尝试保存文件的用户。）

    报告将被保存在事件报告的默认位置，你可以在“文件服务器资源管理器选项”对话框中修改该位置。

11. 选择所有要使用的文件模板属性后，单击**确定**以保存模板。

## <a name="see-also"></a>请参阅

-   [文件屏蔽管理](file-screening-management.md)
-   [设置文件服务器资源管理器选项](setting-file-server-resource-manager-options.md)
-   [编辑文件屏蔽模板属性](edit-file-screen-template-properties.md)

