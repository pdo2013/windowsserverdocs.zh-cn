---
title: 管理 Windows Server Essentials 中的用户帐户
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0d115697-532b-48c2-a659-9f889e235326
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1cce047c45279f7116e0e8a256633df06344e13c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433139"
---
# <a name="manage-user-accounts-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的用户帐户

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

Windows Server Essentials 仪表板的“用户”页面可集中管理信息和任务，用于帮助你管理小型企业网络上的用户帐户。 有关用户仪表板的概述，请参阅[仪表板概述](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md)。  
  
  
##  <a name="BKMK_ManageAccounts"></a> 管理用户帐户  
 以下主题提供了有关如何使用 Windows Server Essentials 仪表板来管理服务器上的用户帐户的信息：  
  
-   [添加用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)  
  
-   [删除用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove)  
  
-   [查看用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage3)  
  
-   [更改用户帐户的显示名称](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage4)  
  
-   [激活用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage5)  
  
-   [停用用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6)  
  
-   [了解用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage7)  
  
-   [使用仪表板的用户帐户管理](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
###  <a name="BKMK_Manage1"></a> 添加用户帐户  
 添加用户帐户时，指定的用户可以登录到网络，并且你可以向用户提供对网络资源（例如共享文件夹和远程 Web 访问站点）的访问权限。 Windows Server Essentials 包括可帮助你实现以下目的的“添加用户帐户”向导：  
  
-   为用户帐户提供名称和密码。  
  
-   将该帐户定义为管理员或标准用户。  
  
-   选择用户可以访问的共享文件夹。  
  
-   指定用户帐户是否具有对网络的远程访问。  
  
-   选择电子邮件选项（如果适用）。  
  
-   如果适用，将分配 （也称为 Windows Server Essentials 中的 Office 365 帐户） 的 Microsoft Online Services 帐户。  
  
-   分配用户组 (仅限 Windows Server Essentials)。  
  
> [!NOTE]
> - 在 Microsoft Azure Active Directory (Azure AD) 中不支持非 ASCII 字符。 如果你的服务器与 Azure AD 集成，则不使用你的密码中的任何非 ASCII 字符。  
>   -   电子邮件选项仅在安装了可提供电子邮件服务的加载项时才可用。  
  
##### <a name="to-add-a-user-account"></a>添加用户帐户  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏上，单击“用户”  。  
  
3.  在“用户任务”   窗格中，单击“添加用户帐户”  。 将出现“添加用户帐户”向导。  
  
4.  按照说明完成向导。  
  
###  <a name="BKMK_Remove"></a> 删除用户帐户  
 当你选择从服务器中删除用户帐户时，某个向导将删除选定的帐户。 因此，你不可以再使用该帐户登录网络或访问任何网络资源。 作为选项，你还可以在删除用户帐户的同时删除该帐户的文件。 如果你不希望永久删除用户帐户，则可以改为停用用户帐户，从而挂起对网络资源的访问。  
  
> [!IMPORTANT]
>  如果用户帐户具有一个 Microsoft 联机帐户就会分配，则删除用户帐户、 联机帐户也从 Microsoft Online Services、 删除和用户的数据，包括电子邮件，将遵循数据 Microsoft Online Services 中的保留策略时。 如果你想要保留联机帐户的用户数据，请停用用户帐户，而非删除它。 有关详细信息，请参阅[管理的用户的联机帐户](Manage-Online-Accounts-for-Users.md)。  
  
##### <a name="to-remove-a-user-account"></a>删除用户帐户  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏上，单击“用户”  。  
  
3.  在用户帐户列表中，选择要删除的用户帐户。  
  
4.  在中 **< 用户帐户\>任务**窗格中，单击**删除用户帐户**。 这将显示“删除用户帐户”向导。  
  
5.  上**你想要保留这些文件？** 页的向导中，您可以选择删除用户的文件，包括文件历史记录备份和用户帐户的重定向的文件夹。 若要保留用户的文件，将复选框保留为空。 做出选择后，单击“下一步”  。  
  
6.  单击“删除帐户”  。  
  
> [!NOTE]
>  删除用户帐户后，该帐户将不再显示在用户帐户列表中。 如果你选择要删除的文件，在服务器中永久删除用户的文件夹从**用户**服务器文件夹和从**文件历史记录备份**服务器文件夹。  
>   
>  如果你有一个集成的电子邮件提供商，还将删除分配给用户帐户的电子邮件帐户。  
  
###  <a name="BKMK_Manage3"></a> 查看用户帐户  
 Windows Server Essentials 仪表板的“用户”  部分将显示网络用户帐户列表。 该列表还提供了有关每个帐户的其他信息。  
  
##### <a name="to-view-a-list-of-user-accounts"></a>查看用户帐户列表  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在主导航栏上，单击“用户”  。  
  
3.  仪表板将显示用户帐户的当前列表。  
  
##### <a name="to-view-or-change-properties-for-a-user-account"></a>查看或更改用户帐户的属性  
  
1.  在用户帐户列表中，选择要查看或更改其属性的帐户。  
  
2.  在中 **< 用户帐户\>任务**窗格中，单击**查看帐户属性**。 这将显示用户帐户的“属性”  页面。  
  
3.  单击选项卡以显示该帐户功能的属性。  
  
4.  若要保存对用户帐户属性所做的任何更改，请单击“应用”  。  
  
###  <a name="BKMK_Manage4"></a> 更改用户帐户的显示名称  
 显示名称是在仪表板的“用户”  页面上显示在“名称”  列中的名称。 更改显示名称不会更改用户帐户的登录名称。  
  
##### <a name="to-change-the-display-name-for-a-user-account"></a>更改用户帐户的显示名称  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏上，单击“用户”  。  
  
3.  在用户帐户列表中，选择要更改的用户帐户。  
  
4.  在中 **< 用户帐户\>任务**窗格中，单击**查看帐户属性**。 这将显示用户帐户的“属性”  页面。  
  
5.  在“常规”  选项卡上，键入用户帐户的新的“名字”  和“姓氏”  ，然后单击“确定”  。  
  
     新的显示名称将显示在用户帐户列表中。  
  
###  <a name="BKMK_Manage5"></a> 激活用户帐户  
 激活用户帐户时，指定的用户可以登录到网络，并且可以访问该帐户有权访问的网络资源（例如共享文件夹和远程 Web 访问站点）。  
  
> [!NOTE]
>  你只能激活已停用的用户帐户。 从服务器中删除用户帐户后无法再对其进行激活。  
  
##### <a name="to-activate-a-user-account"></a>激活用户帐户  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏上，单击“用户”  。  
  
3.  在列表视图中，选择要激活的用户帐户。  
  
4.  在中 **< 用户帐户\>任务**窗格中，单击**激活用户帐户**。  
  
5.  在确认窗口中，单击“是”  以确认你的操作。  
  
> [!NOTE]
>  激活用户帐户后，该帐户的状态将显示为“活动”  。 用户帐户将重新获得已在帐户停用之前分配的相同访问权限。  
>   
>  如果你有一个集成的电子邮件提供商，还将激活分配给用户帐户的电子邮件帐户。  
  
###  <a name="BKMK_Manage6"></a> 停用用户帐户  
 停用用户帐户后，将临时挂起对服务器的帐户访问。 因此，直到激活帐户，指定的用户才可以使用该帐户访问网络资源（例如共享文件夹或远程 Web 访问站点）。  
  
 如果用户帐户分配了 Microsoft 联机帐户，也将停用该联机帐户。 用户不能使用 Office 365 和其他联机服务的订阅，但用户的数据，包括电子邮件，保留在 Microsoft Online Services 中的资源。  
  
> [!NOTE]
>  仅可以停用当前处于活动状态的用户帐户。  
  
##### <a name="to-deactivate-a-user-account"></a>停用用户帐户  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏上，单击“用户”  。  
  
3.  在列表视图中，选择要停用的用户帐户。  
  
4.  在中 **< 用户帐户\>任务**窗格中，单击**停用用户帐户**。  
  
5.  在确认窗口中，单击“是”  以确认你的操作。  
  
> [!NOTE]
>  停用用户帐户后，该帐户的状态将显示为“停用”  。  
>   
>  如果你有一个集成的电子邮件提供商，还将停用分配给用户帐户的电子邮件帐户。  
  
###  <a name="BKMK_Manage7"></a> 了解用户帐户  
 用户帐户向 Windows Server Essentials 提供了重要信息，用于使单个用户访问存储在服务器上的信息，以及使单个用户可以创建并管理其文件和设置。 如果用户具有 Windows Server Essentials 用户帐户并且有权访问计算机，则他们可以登录到网络上的任何计算机。 用户使用其用户名和密码访问用户帐户。  
  
 有两种主要类型的用户帐户。 每种类型都向用户提供了不同的计算机控制级别：  
  
-   **标准**帐户适用于日常计算。 标准帐户通过防止用户做出会影响其他用户的更改（例如删除文件或更改网络设置），可帮助保护你的网络。  
  
-   **管理员**帐户提供了对计算机网络的最大控制。 你应仅在需要时分配管理员帐户类型。  
  
###  <a name="BKMK_Manage8"></a> 使用仪表板的用户帐户管理  
 通过 Windows Server Essentials，可以使用 Windows Server Essentials 仪表板执行常见管理任务。 默认情况下**用户**仪表板的页面包含两个选项卡**用户**并**用户组**。  
  
> [!NOTE]
> - 如果集成与 Office 365 运行 Windows Server Essentials 的服务器，名为的新选项卡**通讯组**还新增了内**用户**仪表板页。  
>   -   在 Windows Server Essentials**用户**仪表板页面仅包含单个选项卡-**用户**。  
  
 “用户”  选项卡包含以下内容：  
  
- 可显示以下内容的用户帐户列表：  
  
  -   用户名。  
  
  -   用户帐户的登录名。  
  
  -   用户帐户是否具有“随处访问”权限。 用户帐户的“随处访问”权限为“允许”  或“不允许”  。  
  
  -   此用户帐户的文件历史记录是否由运行 Windows Server Essentials 的服务器托管。 用户帐户的文件历史记录状态为“托管”  或“非托管”  。  
  
  -   分配给用户帐户的访问级别。 你可以为用户帐户分配“标准用户”  访问权限或“管理员”  访问权限。  
  
  -   用户帐户状态。 用户帐户可以是“活动”  、“非活动”  或“不完整”  。  
  
  -   在 Windows Server Essentials 中，如果服务器与 Office 365 或 Windows Intune 集成将显示 Microsoft 联机帐户。  
  
  -   在 Windows Server Essentials 中，如果服务器与 Microsoft Office 365 集成的用户帐户的 Office 365 帐户 （称为 Microsoft 联机帐户的 Windows Server Essentials 中） 的状态将显示。  
  
- 具有有关选定用户帐户的其他信息的详细信息窗格。  
  
- 包含以下内容的任务窗格：  
  
  -   一组用户帐户管理任务，例如查看和删除用户帐户，以及更改密码。  
  
  -   允许你全局设置或更改网络中所有用户帐户的设置的任务。  
  
  下表介绍了在“用户”  选项卡中可用的各种用户帐户任务。某些任务特定于用户帐户，它们仅在选择列表中的某个用户帐户时才可见。  
  
> [!NOTE]
>  如果将 Office 365 集成与 Windows Server Essentials，其他任务将变为可用。 有关详细信息，请参阅[管理的用户的联机帐户](Manage-Online-Accounts-for-Users.md)。  
  
### <a name="user-account-tasks-in-the-dashboard"></a>仪表板中的用户帐户任务  
  
|任务名称|描述|  
|---------------|-----------------|  
|查看帐户属性|使你能够查看和更改选定用户帐户的属性，以及为该帐户指定文件夹访问权限。|  
|停用用户帐户|停用的用户帐户无法登录到网络或访问网络资源（例如共享文件夹或打印机）。|  
|激活用户帐户|激活的用户帐户可以登录到网络，也可以访问网络资源，如帐户权限所定义。|  
|删除用户帐户|使你能够删除选定用户帐户。|  
|更改用户帐户密码|使你能够重置选定用户帐户的网络密码。|  
|添加用户帐户|启动“添加用户帐户向导”，该向导使你能够创建一个具有标准用户访问或管理员访问的新用户帐户。|  
|分配 Microsoft 联机帐户|将 Microsoft 联机帐户添加到选定的本地网络用户帐户。<br /><br /> 当你的服务器与 Microsoft Online Services（例如 Office 365）集成时，将显示此任务。|  
|添加 Microsoft 联机帐户|添加 Microsoft 联机帐户，并将其关联到本地网络用户帐户。<br /><br /> 当你的服务器与 Microsoft Online Services（例如 Office 365）集成时，将显示此任务。|  
|设置密码策略|使你能够更改网络的密码策略值。|  
|导入 Microsoft 联机帐户|执行将帐户从 Microsoft Online Services 批量导入到本地网络中的操作。<br /><br /> 当你的服务器与 Microsoft Online Services（例如 Office 365）集成时，将显示此任务。|  
|刷新|刷新“用户”选项卡。<br /><br /> 此任务是适用于 Windows Server Essentials。|  
|更改文件历史记录设置|使你能够更改文件历史记录设置，例如备份频率或备份持续时间。<br /><br /> 此任务是适用于 Windows Server Essentials。|  
|导出所有远程连接|针对在过去 30 天内发生的所有到服务器的远程连接，创建 .CSV 格式的文件。|  
  
##  <a name="BKMK_ManageAccess"></a> 管理密码和访问  
 以下主题提供了有关如何使用 Windows Server Essentials 仪表板来管理用户帐户密码和对服务器上共享文件夹的用户访问的信息：  
  
-   [更改或重置用户帐户的密码](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access1)  
  
-   [您应了解的有关密码策略](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access3)  
  
-   [更改密码策略](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access4)  
  
-   [对共享文件夹的访问级别](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access5)  
  
-   [保留和访问管理对删除的用户帐户的文件](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access6)  
  
-   [将 DSRM 密码与网络管理员密码同步](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access7)  
  
-   [授予远程桌面权限的用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access8)  
  
-   [使用户能够访问服务器上的资源](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access9)  
  
-   [更改用户帐户的远程访问权限](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access10)  
  
-   [更改用户帐户的虚拟专用网络权限](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access11)  
  
-   [更改用户帐户的内部共享文件夹的访问](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access12)  
  
-   [允许用户帐户建立到他们的计算机的远程桌面会话](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access13)  
  
###  <a name="BKMK_Access1"></a> 更改或重置用户帐户的密码  
 若要更改或重置用户帐户密码，请按照以下步骤操作。  
  
##### <a name="to-reset-the-password-for-a-user-account"></a>重置用户帐户的密码  
  
1. 打开 Windows Server Essentials 仪表板。  
  
2. 在导航栏上，单击“用户”  。  
  
3. 在用户帐户列表中，选择要重置的用户帐户。  
  
4. 在中 **< 用户帐户\>任务**窗格中，单击**更改用户帐户密码**。 这将显示“更改用户帐户密码”向导。  
  
5. 为用户帐户键入一个新密码，然后再次键入该密码以进行确认。  
  
6. 单击“更改密码”  。  
  
7. 向用户提供新密码。  
  
   > [!IMPORTANT]
   > - 如果你的帐户的密码策略已设置为“密码永不过期”  ，则你可能无法更改密码。  
   >   -   在 Azure AD 中不支持非 ASCII 字符。 因此，如果你的服务器与 Azure AD 集成，请不使用任何非 ASCII 字符在密码中。  
   >   -   如果 Microsoft 联机帐户 （在 Windows Server Essentials 与 Office 365 帐户中称为） 分配给用户，密码与联机帐户密码同步。 用户将使用新密码登录服务器或 Office 365。 有关详细信息，请参阅[管理的用户的联机帐户](Manage-Online-Accounts-for-Users.md)。  
  
###  <a name="BKMK_Access3"></a> 您应了解的有关密码策略  
 密码策略是一组用于定义用户如何创建和使用密码的规则。 该策略有助于防止在未经授权的情况下访问用户数据和存储在服务器上的其他信息。 密码策略适用于可访问网络的所有用户帐户。  
  
 Windows Server Essentials 密码策略包含三个主要元素，如下所示：  
  
- **密码长度**。  密码越长越安全。 空白密码是不安全的。  
  
- **密码复杂性**。  复杂密码包含大写字母和小写字母（a-z、A-Z）、基本数字 (0-9) 以及非字母符号（例如 ;、!、@、#、_、-）组合。 复杂密码不易遭到未经授权的访问。 包含用户名、出生日期或其他个人信息的密码不能提供足够的安全性。  
  
- **密码使用期限**。  Windows Server Essentials 要求用户至少每隔 180 天更改一次密码。 作为选项，你可以选择将密码设置为永不过期。  
  
  若要更加轻松地在你的计算机网络上实现密码策略，Windows Server Essentials 提供了一个简单工具，你可以使用该工具将密码策略设置或更改为以下四个预定义的策略配置文件之一：  
  
- **弱**。  用户可以指定任何不为空的密码。  
  
- **中**。  这些密码必须包含至少 5 个字符。 无需使用复杂密码。  
  
- **中强**。  这些密码必须包含至少 5 个字符，并且必须包含字母、数字和符号。  
  
- **强**。  这些密码必须包含至少 7 个字符，并且必须包含字母、数字和符号。 这些密码更安全，但用户可能更难以记住它们。  
  
  > [!NOTE]
  >  密码不能包含用户名或电子邮件地址。  
  > 
  >  如果与 Office 365 集成，则该集成将强制使用“强”  密码策略，并更新该策略来包含以下要求：  
  > 
  > - 密码必须包含 8 16 个字符。  
  >   -   密码不能包含空格或 Office 365 电子邮件名称。  
  
  默认情况下，服务器安装将默认密码策略设置为**强**选项。  
  
###  <a name="BKMK_Access4"></a> 更改密码策略  
 使用以下过程，将密码策略设置或更改为四个预定义策略配置文件中的任意一个。  
  
##### <a name="to-change-the-password-policy"></a>更改密码策略  
  
1.  打开 Windows Server Essentials 仪表板，然后单击“用户”  。  
  
2.  在“用户任务”  窗格中，单击“设置密码策略”  。  
  
3.  在“更改密码策略”  屏幕上，通过移动滑块设置密码强度的级别。  
  
     Microsoft 建议你将密码强度设置为“强”  。  
  
    > [!NOTE]
    >  作为选项，你可以选择“密码永不过期”  。 此设置不太安全，因此不建议使用。  
  
4.  单击“更改策略”  。  
  
###  <a name="BKMK_Access5"></a> 对共享文件夹的访问级别  
 最佳做法是，你应该分配仍然允许用户执行所需任务的最严格权限。  
  
 你拥有三种可用于服务器上共享文件夹的访问设置：  
  
-   **读/写**。  如果你想要允许此用户帐户具有创建、更改和删除共享文件夹中的任何文件的权限，则选择此设置。  
  
-   **只读**。  如果你想要允许此用户帐户仅具有读取共享文件夹中的文件的权限，则选择此设置。 具有只读访问权限的用户帐户不能创建、更改或删除共享文件夹中的任何文件。  
  
-   **无访问权限**。  如果你不希望此用户帐户访问共享文件夹中的任何文件，则选择此设置。  
  
###  <a name="BKMK_Access6"></a> 保留和访问管理对删除的用户帐户的文件  
 网络管理员可以删除用户帐户，并选择保留供将来使用的文件的用户。 在此情况下，不能再使用已删除的用户帐户登录网络；但是，此用户的文件将保存在可与其他用户共享的共享文件夹中。  
  
> [!IMPORTANT]
>  请注意，如果删除分配了 Microsoft 联机帐户的用户帐户，则还将删除该联机帐户，并且用户数据（包括电子邮件）受 Microsoft Online Services 中的数据保留策略限制。 若要保留联机帐户的用户数据，请停用用户帐户，而非删除它。 有关详细信息，请参阅[管理的用户的联机帐户](Manage-Online-Accounts-for-Users.md)。  
  
##### <a name="to-remove-a-user-account-but-retain-access-to-the-user-s-files"></a>若要删除用户帐户，但保留对用户的文件的访问  
  
1. 打开 Windows Server Essentials 仪表板。  
  
2. 在导航栏上，单击“用户”  。  
  
3. 在用户帐户列表中，选择要删除的用户帐户。  
  
4. 在中 **< 用户帐户\>任务**窗格中，单击**删除用户帐户**。 这将显示“删除用户帐户”向导。  
  
5. 在“是否保留这些文件？”  页面上，确保清除“删除此用户帐户的文件(包括文件历史记录备份和重定向文件夹)”  复选框，然后单击“下一步”  。  
  
    将显示一个确认页面，警告你删除帐户，但保留这些文件。  
  
6. 单击“删除帐户”  以删除用户帐户。  
  
   删除用户帐户后，管理员可以向其他用户帐户提供对共享文件夹的访问权限。  
  
##### <a name="to-give-a-user-account-permission-to-access-a-shared-folder"></a>提供用户帐户权限以访问共享文件夹  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏上，单击“存储”  ，然后单击“服务器文件夹”  选项卡。  
  
3.  在文件夹列表中，选择“用户”  文件夹。  
  
4.  在“用户任务”  窗格中，单击“打开文件夹”  。 Windows 资源管理器将打开并显示“用户”  文件夹的内容。  
  
5.  右键单击要共享的用户帐户的文件夹，然后单击“属性”  。  
  
6.  在中 **< 用户帐户\>属性**，单击**共享**选项卡，然后依次**共享**。  
  
7.  在“文件共享”  窗口中，键入或选择要与其共享文件夹的用户帐户名称，然后单击“添加”  。  
  
8.  选择用户帐户所需的“权限级别”  ，然后单击“共享”  。  
  
###  <a name="BKMK_Access7"></a> 将 DSRM 密码与网络管理员密码同步  
 目录服务还原模式 (DSRM) 是一种用于修复或恢复 Active Directory 的特殊启动模式。 如果 Active Directory 失败或需要还原，则操作系统将使用 DSRM 登录计算机。 如果网络管理员密码和 DSRM 密码不同，则不会加载 DSRM。  
  
 在 Windows Server Essentials 的首次干净安装期间，该程序会将 DSRM 密码设置为你在安装期间或在迁移应答文件中指定的网络管理员帐户密码。 当你更改网络管理员密码（根据建议，随着服务器安全性的增加，通常每隔 60 天更改一次）时，不会将密码更改转发到 DSRM。 这会导致密码不匹配。 如果发生这种情况，您可以使用以下解决方案手动或自动同步 DSRM 密码与网络管理员的密码。  
  
##### <a name="to-manually-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>手动将 DSRM 密码同步到网络管理员帐户  
  
1. 在命令提示符下，运行 `ntdsutil.exe` 打开 ntdsutil 工具。  
  
2. 若要重置 DSRM 密码，请键入 **set dsrm password**。  
  
3. 若要与当前网络管理员的帐户同步的域控制器上的 DSRM 密码，请键入：  
  
    **从域帐户同步** *< current_network_administrator_account >* ，然后按 Enter。  
  
   因为你将定期更改网络管理员帐户的密码，以确保 DSRM 密码始终与网络管理员的当前密码相同，因此我们建议你创建一个定期任务，以便每日自动将 DSRM 密码同步到网络管理员密码。  
  
##### <a name="to-automatically-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>自动将 DSRM 密码同步到网络管理员帐户  
  
1.  从服务器中，打开“管理工具”  ，然后双击“任务计划程序”  。  
  
2.  在任务计划程序“操作”  窗格中，单击“创建任务”  。  
  
3.  在“名称”  文本框中，键入任务的名称（例如 **自动同步 DSRM 密码**），然后选择“使用最高权限运行”  选项。  
  
4.  定义该任务应在何时运行：  
  
    1.  在“创建任务”  对话框中，单击“触发器”  选项卡，然后单击“新建”  。  
  
    2.  在“新建触发器”  对话框中，选择重复出现的选项、指定重复出现的时间间隔，然后选择开始时间。  
  
        > [!NOTE]
        >  最佳做法是，你应该将任务设置为每天在非工作时间期间运行。  
  
    3.  单击“确定”  以保存更改，然后返回到“创建任务”  对话框。  
  
5.  定义任务操作：  
  
    1.  单击“操作”  选项卡，然后单击“新建”  。 这将显示“新建操作”  对话框。  
  
    2.  在“操作”  列表中，单击“启动程序”  ，然后浏览到 **C:\WINDOWS\SYSTEM32\ntdsutil.exe**。  
  
    3.  在中**添加自变量**（可选） 文本框框中，键入的以下内容 （必须包含引号）：**设置 dsrm 密码同步 SBS_network_administrator_account q q 的域帐户从**其中*SBS_network_administrator_account*是当前网络管理员的帐户名。  
  
6.  单击“确定”  两次以保存任务，然后关闭“创建任务”  对话框。 新任务将显示在“任务计划”  的“活动任务”  部分中。  
  
###  <a name="BKMK_Access8"></a> 授予远程桌面权限的用户帐户  
 在 Windows Server Essentials 的默认安装中，网络用户无权建立到计算机或网络上其他资源的远程连接。  
  
 你必须先设置“随处访问”，网络用户才可以建立到网络资源的远程连接。 设置“随处访问”后，用户可以通过 Internet 连接从任意位置的设备访问办公网络中的文件、应用程序和计算机。  
  
 “设置随处访问”向导允许你启用远程访问的两种方法：  
  
- 虚拟专用网络 (VPN)  
  
- 远程 Web 访问  
  
  运行该向导时，你还可以选择允许将“随处访问”用于所有当前和新添加的用户帐户。  
  
  若要设置“随处访问”，请打开仪表板“主页”  页面、单击“安装”  ，然后单击“设置随处访问”  。  
  
  有关随处访问的详细信息，请参阅[Manage Anywhere Access](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)。  
  
###  <a name="BKMK_Access9"></a> 使用户能够访问服务器上的资源  
  本部分适用于运行 Windows Server Essentials 或 Windows Server Essentials 的服务器或到安装了 Windows Server Essentials 体验角色运行 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 的服务器。  
  
 如果你希望用户使用远程访问和/或具有单个用户帐户，则在完成将计算机连接到服务器的操作后，你可以使用仪表板为服务器上联网计算机的用户创建新的网络用户帐户。 有关创建用户帐户的详细信息，请参阅 [Add a user account](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)。 创建用户帐户后，你必须向客户端计算机的用户提供网络用户名和密码信息，以便他们可以使用快速启动板访问服务器上的资源。  
  
 对于你创建的每个用户帐户，你可以通过用户帐户属性设置对以下内容的访问权限：  
  
-   **共享文件夹**。  默认情况下，网络管理员具有对所有共享文件夹的“读/写”  权限，标准用户帐户具有对公司文件夹的“只读”  权限。 如果启用了媒体流，则可以为以下共享文件夹分配单个标准用户帐户的文件夹访问权限：“音乐”  、“图片”  、“录制的电视”  和“视频”  。 你可以设置用户帐户权限，以在用户帐户属性的“共享文件夹”  选项卡上访问共享文件夹。  
  
-   **随处访问**。  默认情况下，网络管理员可以使用 VPN 或远程 Web 访问来访问服务器资源。 对于标准用户帐户，必须在“随处访问”  选项卡上设置用户帐户权限。  
  
-   **计算机访问**。  默认情况下，网络管理员可以访问网络中的所有计算机。 但是，对于标准用户帐户，可以设置单个用户帐户权限，以便在用户帐户属性的“计算机访问”  选项卡上访问网络上的计算机。  
  
##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012-r2"></a>编辑 Windows Server Essentials 2012 R2 中的用户帐户属性  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏上，单击“用户”  。  
  
3.  在用户帐户列表中，选择要编辑的用户帐户。  
  
4.  在中 **< 用户帐户\>任务**窗格中，单击**查看帐户属性**。  
  
5.  在中 **< 用户帐户\>属性**，执行以下操作：  
  
    1.  在“共享文件夹”  选项卡上，根据需要为每个共享文件夹设置相应的文件夹权限。  
  
    2.  在“随处访问”  选项卡上：  
  
        1.  若要允许用户通过 VPN 连接到服务器，请选中“允许虚拟专用网络 (VPN)”  复选框。  
  
        2.  若要允许用户通过远程 Web 访问连接到服务器，请选中“允许远程 Web 访问和对 Web 服务应用程序的访问”  复选框。  
  
    3.  在“计算机访问”  选项卡上，选择你希望用户有权访问的网络计算机。  
  
##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012"></a>编辑 Windows Server Essentials 2012 中的用户帐户属性  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏上，单击“用户”  。  
  
3.  在用户帐户列表中，选择要编辑的用户帐户。  
  
4.  在中 **< 用户帐户\>任务**窗格中，单击**属性**。  
  
5.  在中 **< 用户帐户\>属性**，执行以下操作：  
  
    1.  如果用户帐户需要访问网络运行状况报告，则在“常规”  选项卡上，选择“用户可以查看网络运行状况警报”  。  
  
    2.  在“共享文件夹”  选项卡上，根据需要为每个共享文件夹设置相应的文件夹权限。  
  
    3.  在“随处访问”  选项卡上：  
  
        1.  若要允许用户通过 VPN 连接到服务器，请选中“允许虚拟专用网络 (VPN)”  复选框。  
  
        2.  若要允许用户通过远程 Web 访问连接到服务器，请选中“允许远程 Web 访问和对 Web 服务应用程序的访问”  复选框。  
  
    4.  在“计算机访问”  选项卡上，选择你希望用户有权访问的网络计算机。  
  
###  <a name="BKMK_Access10"></a> 更改用户帐户的远程访问权限  
 通过使用虚拟专用网络 (VPN)、远程 Web 访问或其他 Web 服务应用程序，用户可以从远程位置访问位于服务器上的资源。 默认情况下，当你使用仪表板在 Windows Server Essentials 中配置“随处访问”时，将为网络用户启用远程访问权限。  
  
##### <a name="to-change-remote-access-permissions-for-a-user-account"></a>更改用户帐户的远程访问权限  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏上，单击“用户”  。  
  
3.  在用户帐户列表中，选择要更改的用户帐户。  
  
4.  在中 **< 用户帐户\>任务**窗格中，单击**查看帐户属性**。 这将显示用户帐户的“属性”  页面。  
  
5.  在“随处访问”  选项卡上，执行以下操作：  
  
    -   选中“允许虚拟专用网络 (VPN)”  复选框，以允许用户通过使用 VPN 连接到服务器。  
  
    -   选中“允许远程 Web 访问和访问 Web 服务应用程序”  复选框，以允许用户通过使用远程 Web 访问连接到服务器。  
  
6.  单击“应用”  ，然后单击“确定”  。  
  
###  <a name="BKMK_Access11"></a> 更改用户帐户的虚拟专用网络权限  
 你可以使用虚拟专用网络 (VPN) 连接到 Windows Server Essentials 并访问存储在服务器上的所有资源。 如果你有一台通过网络帐户设置的客户端计算机，并且该网络帐户可通过 VPN 连接来连接到托管的 Windows Server Essentials 服务器，则上述方法尤其有用。 在托管 Windows Server Essentials 服务器上新创建的所有用户帐户均必须在首次使用时通过 VPN 登录到客户端计算机。  
  
##### <a name="to-change-vpn-permissions-for-network-users"></a>更改网络用户的 VPN 权限  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏上，单击“用户”  。  
  
3.  在用户帐户列表中，选择你要向其授予远程访问桌面的权限的用户帐户。  
  
4.  在中 **< 用户帐户\>任务**窗格中，单击**属性**。  
  
5.  在中 **< 用户帐户\>属性**，单击**随处访问**选项卡。  
  
6.  在“随处访问”  选项卡上，若要允许用户通过 VPN 连接到服务器，请选中“允许虚拟专用网络 (VPN)”  复选框。  
  
7.  单击“应用”  ，然后单击“确定”  。  
  
###  <a name="BKMK_Access12"></a> 更改用户帐户的内部共享文件夹的访问  
 通过使用仪表板的“服务器文件夹”  选项卡上的任务，你可以管理对服务器上任何共享文件夹的访问。 默认情况下，在安装 Windows Server Essentials 时创建以下服务器文件夹：  
  
-   “客户端计算机备份”  。  用于存储由 Windows Server Backup 创建的客户端计算机备份。 不共享此服务器文件夹。  
  
-   “公司”  。  用于供网络用户存储和访问与你的组织相关的文档。  
  
-   “文件历史记录备份”  。  默认情况下，Windows Server Essentials 存储通过使用文件历史记录创建的文件备份。 不共享此服务器文件夹。  
  
-   “文件夹重定向”  。  用于供网络用户存储和访问为文件夹重定向设置的文件夹。 不共享此服务器文件夹。  
  
-   “音乐”  。  用于供网络用户存储和访问音乐文件。 在你打开媒体共享时创建此文件夹。  
  
-   “图片”  。  用于供网络用户存储和访问图片。 在你打开媒体共享时创建此文件夹。  
  
-   “录制的电视”  。  用于供网络用户存储和访问录制的电视节目。 在你打开媒体共享时创建此文件夹。  
  
-   “视频”  。  用于供网络用户存储和访问视频。 在你打开媒体共享时创建此文件夹。  
  
-   “用户”  。  用于供网络用户存储和访问文件。 针对你创建的每个网络用户帐户，特定于用户的文件夹将在“用户”  服务器文件夹中自动生成。  
  
##### <a name="to-change-access-to-a-shared-folder-for-a-user-account"></a>更改对用户帐户的共享文件夹的访问  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  单击“存储”  ，然后单击“服务器文件夹”  。  
  
3.  导航到并选择你要为其修改权限的服务器文件夹。  
  
4.  在任务窗格中，单击“查看文件夹属性”  。  
  
5.  在中 **< 文件夹名\>属性**，单击**共享**，并选择列出的用户帐户的相应的用户访问级别，然后单击**应用**。  
  
    > [!NOTE]
    >  不能修改“文件历史记录备份”  、“文件夹重定向”  和“用户”  服务器文件夹的共享权限。 因此，这些服务器文件夹的文件夹属性不包含“共享”  选项卡。  
  
###  <a name="BKMK_Access13"></a> 允许用户帐户建立到他们的计算机的远程桌面会话  
  本部分适用于运行 Windows Server Essentials 或 Windows Server Essentials 的服务器或到安装了 Windows Server Essentials 体验角色运行 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 的服务器。  
  
 网络管理员可以向网络用户授予允许他们从远程位置访问网络计算机的权限。  
  
##### <a name="to-enable-users-to-access-their-network-computers-from-a-remote-location"></a>使用户能够从远程位置访问他们的网络计算机  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏上，单击“用户”  。  
  
3.  在用户帐户列表中，选择你要授予用于远程访问桌面权限的用户帐户。  
  
4.  在中 **< 用户帐户\>任务**窗格中，单击**属性**。  
  
5.  在中 **< 用户帐户\>属性**，单击**计算机访问**选项卡。  
  
6.  选择你希望此用户帐户能够远程访问的计算机，然后单击“确定”  。  
  
## <a name="see-also"></a>请参阅  
  
-   [管理用户的联机帐户](Manage-Online-Accounts-for-Users.md)  
  
-   [获取连接](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
