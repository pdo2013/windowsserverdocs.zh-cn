---
title: "管理 Windows Server Essentials 中的用户帐户"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.openlocfilehash: 91175836e4453860b17d2655e6a5a831645de410
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="manage-user-accounts-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的用户帐户

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

Windows Server Essentials 仪表板的用户页集中了信息和任务，帮助你管理小型企业网络上的用户帐户。 有关用户仪表板概述，请参阅[仪表板概述](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md)。  
  
  
##  <a name="BKMK_ManageAccounts"></a>管理用户帐户  
 以下主题提供有关如何使用 Windows Server Essentials 仪表板管理服务器上的用户帐户的信息：  
  
-   [添加用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)  
  
-   [删除的用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove)  
  
-   [查看用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage3)  
  
-   [更改用户帐户的显示名称](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage4)  
  
-   [激活的用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage5)  
  
-   [停用的用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6)  
  
-   [了解用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage7)  
  
-   [管理用户帐户使用仪表板](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
###  <a name="BKMK_Manage1"></a>添加用户帐户  
 当你添加用户帐户时，分配的用户可以登录到该网络，并可让用户访问网络资源，如共享的文件夹以及远程 Web 访问网站的权限。 Windows Server Essentials 包括添加用户帐户向导，可帮助你：  
  
-   提供的用户帐户的用户名和密码。  
  
-   不论采取何管理员作为或标准用户定义的帐户。  
  
-   选择这共享的文件夹的用户帐户都可以访问。  
  
-   指定是否远程网络的访问权限的用户帐户。  
  
-   如果适用，请选择电子邮件选项。  
  
-   如果适用，请指定 （也称为 Windows Server Essentials 中的 Office 365 帐户） 是 Microsoft Online Services 帐户。  
  
-   分配用户组 (仅 Windows Server Essentials)。  
  
> [!NOTE]
>  -   Microsoft Azure Active Directory (Azure AD) 中的非 ASCII 字符不受支持。 如果你服务器与 Azure AD 集成，则不得使用任何非 ASCII 字符在你的密码。  
> -   电子邮件选项才可用，如果你安装了提供电子邮件服务的外接程序。  
  
##### <a name="to-add-a-user-account"></a>若要添加的用户帐户  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**用户**。  
  
3.  在**用户任务**窗格中，单击**添加用户帐户**。 此时将显示添加用户帐户向导。  
  
4.  按照说明来完成向导。  
  
###  <a name="BKMK_Remove"></a>删除的用户帐户  
 当你选择要从服务器中删除的用户帐户时，所选的帐户中删除向导。 出于此原因，你可以登录到该网络或访问网络资源的任何不再使用该帐户。 作为一个选项，你还可以删除你删除帐户的用户帐户在同一时间文件。 如果你不希望永久删除该用户的帐户，你可以停用的用户帐户改用暂停访问网络资源。  
  
> [!IMPORTANT]
>  如果用户帐户已 Microsoft 在线帐户时, 分配删除用户帐户、 在线帐户还会从 Microsoft Online Services，并且用户的数据，包括电子邮件、 遵循数据保留在 Microsoft Online Services 的策略。 如果你想要保留用户数据的在线帐户，停用的用户帐户，而不是删除它。 有关详细信息，请参阅[用户管理在线帐户](Manage-Online-Accounts-for-Users.md)。  
  
##### <a name="to-remove-a-user-account"></a>若要删除的用户帐户  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**用户**。  
  
3.  在用户帐户列表中，选择你想要删除的用户帐户。  
  
4.  在**< 用户 Account\ > 任务**窗格中，单击**用户帐户中删除**。 删除出现用户帐户向导。  
  
5.  在**你想要保留文件？**页面向导中，你可以选择要删除的用户的文件，包括文件历史记录备份和用户帐户重定向的文件夹。 若要防止用户的文件，将该复选框保留为空。 你所选内容后，单击**下一步**。  
  
6.  单击**删除帐户**。  
  
> [!NOTE]
>  删除的用户帐户后，该帐户将不再出现在列表中的用户帐户。 如果你选择要删除的文件，服务器中永久删除用户的文件夹**用户**服务器文件夹以及从**文件历史记录备份**服务器文件夹。  
>   
>  如果你有集成的电子邮件提供商，还将删除分配给用户帐户的电子邮件帐户。  
  
###  <a name="BKMK_Manage3"></a>查看用户帐户  
 **用户**Windows Server Essentials 仪表板部分显示的网络用户帐户的列表。 列表中还提供了有关每个帐户的其他信息。  
  
##### <a name="to-view-a-list-of-user-accounts"></a>若要查看列表中的用户帐户  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在主要导航栏中，单击**用户**。  
  
3.  仪表板中显示用户帐户的当前的列表。  
  
##### <a name="to-view-or-change-properties-for-a-user-account"></a>若要查看或更改用户帐户的属性  
  
1.  在用户帐户列表中，选择你想要查看或更改属性的帐户。  
  
2.  在**< 用户 Account\ > 任务**窗格中，单击**查看帐户属性**。 **属性**页面的显示的用户帐户。  
  
3.  单击选项卡显示有关该帐户功能属性。  
  
4.  若要保存所做的用户帐户属性任何更改，请单击**应用**。  
  
###  <a name="BKMK_Manage4"></a>更改用户帐户的显示名称  
 显示名称为的名称中出现**名称**上的列**用户**仪表板中的页面。 更改显示名称不会更改为用户帐户登录或登录的名称。  
  
##### <a name="to-change-the-display-name-for-a-user-account"></a>若要更改用户帐户的显示名称  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**用户**。  
  
3.  在用户帐户列表中，选择你想要更改用户帐户。  
  
4.  在**< 用户 Account\ > 任务**窗格中，单击**查看帐户属性**。 **属性**页面的显示的用户帐户。  
  
5.  在**常规**选项卡上，键入新**名字**和**姓氏**的用户帐户，然后单击**确定**。  
  
     新的显示名称将显示在列表中的用户帐户。  
  
###  <a name="BKMK_Manage5"></a>激活的用户帐户  
 当您激活的用户帐户时，分配的用户可以登录到的网络和访问网络资源向其帐户具有权限，如共享的文件夹以及远程网站访问的站点。  
  
> [!NOTE]
>  你只能激活停用的用户帐户。 之后你从服务器删除，你将无法激活的用户帐户。  
  
##### <a name="to-activate-a-user-account"></a>若要激活的用户帐户  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**用户**。  
  
3.  在列表视图中，选择你想要激活的用户帐户。  
  
4.  在**< 用户 Account\ > 任务**窗格中，单击**激活的用户帐户**。  
  
5.  在确认窗口中，单击**是**以确认你的操作。  
  
> [!NOTE]
>  激活的用户帐户后，将该帐户的状态显示**活动**。 用户帐户重新获得相同帐户停用之前分配的访问权限。  
>   
>  如果你有集成的电子邮件提供商，还将激活分配给用户帐户的电子邮件帐户。  
  
###  <a name="BKMK_Manage6"></a>停用的用户帐户  
 当停用的用户帐户时，被暂时暂停的帐户访问服务器。 出于此原因，分配的用户无法使用该帐户前激活帐户访问网络资源，如共享文件夹或远程网站访问的站点。  
  
 如果用户帐户具有分配 Microsoft 在线帐户，还停用在线帐户。 用户无法使用 Office 365 和其他您订阅，但用户的数据，包括电子邮件、 保留在 Microsoft Online Services 的在线服务中的资源。  
  
> [!NOTE]
>  你可以仅停用的当前处于活动状态的用户帐户。  
  
##### <a name="to-deactivate-a-user-account"></a>停用的用户帐户  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**用户**。  
  
3.  在列表视图中，选择你想要停用的用户帐户。  
  
4.  在**< 用户 Account\ > 任务**窗格中，单击**停用的用户帐户**。  
  
5.  在确认窗口中，单击**是**以确认你的操作。  
  
> [!NOTE]
>  停用的用户帐户后，将该帐户的状态显示**停用**。  
>   
>  如果你有集成的电子邮件提供商，将还会停用分配给用户帐户的电子邮件帐户。  
  
###  <a name="BKMK_Manage7"></a>了解用户帐户  
 用户帐户提供对 Windows Server Essentials，使人可以访问存储在服务器上，并使其可能为单个用户，若要创建和管理他们的文件和设置的信息的重要信息。 用户可以登录到该网络上的任何计算机如果他们拥有的 Windows Server Essentials 用户的帐户，并且他们有权访问计算机的步骤。 用户访问他们的用户帐户的用户名和密码。  
  
 有两个主要类型的用户帐户。 每种类型为用户提供了不同级别的控制计算机：  
  
-   **标准**帐户适用于日常计算。 标准帐户，有助于保护通过阻止用户进行了影响的其他用户，例如删除文件或更改网络设置更改你的网络。  
  
-   **管理员**帐户提供最控制计算机网络。 你应该指定管理员帐户类型，仅在必要时。  
  
###  <a name="BKMK_Manage8"></a>管理用户帐户使用仪表板  
 Windows Server Essentials 便可以通过使用 Windows Server Essentials 仪表板中执行常见的管理任务。 默认情况下，**用户**仪表板中的页面中包括两个选项卡**用户**和**用户和组**。  
  
> [!NOTE]
>  -   如果集成你运行的 Windows Server Essentials 与 Office 365 的服务器，在新选项卡称为**Distribution 组**还会添加内**用户**仪表板中的页面。  
> -   在 Windows Server Essentials，**用户**仪表板中的页面包括只能同一个选项卡的**用户**。  
  
 **用户**选项卡中包括以下：  
  
-   用户帐户，其中显示列表：  
  
    -   用户的名称。  
  
    -   登录的用户帐户名。  
  
    -   是否用户帐户有任何地方访问权限。 随时随地访问权限的用户帐户是**允许**或**不得**。  
  
    -   是否由运行 Windows Server Essentials 服务器管理此用户帐户文件历史记录。 用户帐户的文件历史记录状态是**管理**或**未管理**。  
  
    -   分配给用户帐户的访问权限的级别。 你可以分配**标准用户**访问或**管理员**用户帐户的访问权限。  
  
    -   用户帐户状态。 用户帐户可**活动**，**停用**，或**完整**。  
  
    -   在 Windows Server Essentials，如果该服务器集成与 Office 365 或 Windows Intune 将显示 Microsoft 在线帐户。  
  
    -   在 Windows Server Essentials，如果该服务器是已集成了 Microsoft Office 365、 将显示的用户帐户的 Office 365 帐户 （在 Windows Server Essentials 与 Microsoft 在线帐户中的已知） 的状态。  
  
-   有关的所选的用户帐户的其他信息的详细信息窗格。  
  
-   任务窗格中，其中包括：  
  
    -   一组的用户帐户等查看和删除的用户帐户，并更改密码的管理任务。  
  
    -   允许你全球设置或更改设置为网络中的所有用户帐户的任务。  
  
 下表介绍了各种从可用的用户帐户任务**用户**选项卡。 一些任务是用户帐户详细信息，并在列表中选择一个用户帐户时，它们是才可见。  
  
> [!NOTE]
>  如果你将 Office 365 集成与 Windows Server Essentials，其他任务将变为可用。 有关详细信息，请参阅[用户管理在线帐户](Manage-Online-Accounts-for-Users.md)。  
  
### <a name="user-account-tasks-in-the-dashboard"></a>用户帐户仪表板中的任务  
  
|任务名称|描述|  
|---------------|-----------------|  
|查看帐户属性|使您可查看和更改所选的用户帐户的属性，并指定文件夹的帐户的访问权限。|  
|停用的用户帐户|停用的用户帐户无法登录到如共享的文件夹，或者打印机的网络或访问网络资源。|  
|激活的用户帐户|已激活的用户帐户可以登录到该网络，并且可以访问网络资源，由帐户的权限。|  
|删除的用户帐户|使您能够删除选定的用户帐户。|  
|更改用户帐户密码|可以让你重置选定的用户帐户的网络密码。|  
|添加用户帐户|启动添加用户帐户向导中，这可以让你创建一个的新用户帐户具有标准用户访问或管理员进行访问。|  
|指定 Microsoft 在线帐户|将 Microsoft 在线帐户添加到所选的本地网络用户帐户。<br /><br /> 与 Microsoft 的联机服务，如 Office 365 集成服务器时，将显示此任务。|  
|添加 Microsoft 在线帐户|添加了 Microsoft 在线帐户并将它们与本地网络用户的帐户相关联。<br /><br /> 与 Microsoft 的联机服务，如 Office 365 集成服务器时，将显示此任务。|  
|设置密码策略|你可以更改密码的值为你的网络策略的支持。|  
|导入 Microsoft 在线帐户|来自 Microsoft 的联机服务的批量导入的帐户执行到本地网络。<br /><br /> 与 Microsoft 的联机服务，如 Office 365 集成服务器时，将显示此任务。|  
|恢复|刷新用户选项卡。<br /><br /> 此任务是适用于 Windows Server Essentials。|  
|更改设置文件历史记录|可以让你更改设置文件历史记录中的，如备份频率或备份持续时间。<br /><br /> 此任务是适用于 Windows Server Essentials。|  
|导出所有远程连接|创建。CSV 格式的所有远程连接到服务器过去 30 天内发生的文件。|  
  
##  <a name="BKMK_ManageAccess"></a>管理密码和访问  
 以下主题提供有关如何使用 Windows Server Essentials 仪表板管理用户帐户的密码和服务器上的共享文件夹的用户访问信息：  
  
-   [更改或重置为用户帐户的密码](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access1)  
  
-   [你应该提前了解的密码策略内容](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access3)  
  
-   [更改密码策略](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access4)  
  
-   [共享文件夹的访问权限的级别](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access5)  
  
-   [保留和管理已删除的用户帐户的文件的访问权限](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access6)  
  
-   [与网络管理员密码同步 DSRM 密码](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access7)  
  
-   [提供远程桌面权限的用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access8)  
  
-   [使用户可以访问在服务器上的资源](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access9)  
  
-   [更改远程访问权限的用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access10)  
  
-   [更改虚拟专用网络的用户帐户的权限](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access11)  
  
-   [更改访问内部共享文件夹的用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access12)  
  
-   [允许建立远程桌面会话向他们的计算机的用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access13)  
  
###  <a name="BKMK_Access1"></a>更改或重置为用户帐户的密码  
 若要更改或重置用户帐户密码，请按照以下步骤。  
  
##### <a name="to-reset-the-password-for-a-user-account"></a>重置为用户帐户的密码  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**用户**。  
  
3.  在用户帐户列表中，选择你想要重置的用户帐户。  
  
4.  在**< 用户 Account\ > 任务**窗格中，单击**更改用户帐户的密码**。 更改用户帐户的密码向导将显示。  
  
5.  键入新密码的用户帐户，然后再重新键入密码以确认它。  
  
6.  单击**更改密码**。  
  
7.  为用户提供新的密码。  
  
    > [!IMPORTANT]
    >  -   你可能无法更改你的密码，如果你的帐户的密码策略已设置为**密码永远不会到期**。  
    > -   Azure AD 中不支持非 ASCII 字符。 因此，如果你服务器与 Azure AD 集成，请不要使用任何非 ASCII 字符在你的密码。  
    > -   如果 （在 Windows Server Essentials 与 Office 365 帐户已知） 的 Microsoft 在线帐户分配给用户，密码同步在线帐户密码。 用户都将使用新密码登录在服务器上，或登录到 Office 365。 有关详细信息，请参阅[用户管理在线帐户](Manage-Online-Accounts-for-Users.md)。  
  
###  <a name="BKMK_Access3"></a>你应该提前了解的密码策略内容  
 密码策略是一套规则定义用户如何创建和使用密码。 该策略帮助防止对用户数据和其他信息存储在服务器上的未授权的访问。 密码策略为所有用户帐户访问网络的应用。  
  
 Windows Server Essentials 密码策略三个主要要素组成，如下所示：  
  
-   **密码长度**。  密码的时间越长，它是更加安全。 密码的空白都不安全。  
  
-   **密码复杂性**。  复杂密码包含多种大写并小写字母一次，A Z），基本数字 (0 到 9) 和非字母符号 (如; ！，@，#、 _，-)。 复杂的密码是更易受未经授权的访问。 含有用户姓名、 出生日期或其他个人信息的密码不提供足够的安全性。  
  
-   **密码年龄**。  Windows Server Essentials 需要用户更改其密码至少运行一次每 180 天。 作为一个选项，你可以选择使用密码永远不会到期。  
  
 若要使其更轻松地在你的计算机的网络上实现密码策略，Windows Server Essentials 提供了一个简单的工具，允许你设置或更改密码策略任何下面的四个预定义的策略配置文件：  
  
-   **弱**。  用户可以指定不是空白任何密码。  
  
-   **中等**。  这些密码必须包含至少 5 个字符。 不需要一个复杂的密码。  
  
-   **中等强**。  这些密码至少 5 个字符，，并且必须包括字母、 数字和符号。  
  
-   **强**。  这些密码 7 至少个字符，，并且必须包括字母、 数字和符号。 这些密码更安全，但可能更难以能够记起的用户。  
  
    > [!NOTE]
    >  密码包含不能用户名或电子邮件地址。  
    >   
    >  如果你与 Office 365 集成，集成强制**强**密码策略和更新的策略以包含以下要求：  
    >   
    >  -   密码必须包含 8 16 个字符。  
    > -   密码无法包含一个空格或 Office 365 电子邮件名称。  
  
 默认情况下，安装服务器将默认密码的策略设置为**强**选项。  
  
###  <a name="BKMK_Access4"></a>更改密码策略  
 使用以下步骤来设置或更改密码策略任何四个预定义的策略配置文件。  
  
##### <a name="to-change-the-password-policy"></a>若要更改密码策略  
  
1.  打开 Windows Server Essentials 仪表板，然后单击**用户**。  
  
2.  在**用户任务**窗格中，单击**设置密码策略**。  
  
3.  在**更改密码策略**屏幕上，通过移动滑块来设置的密码的强度级别。  
  
     Microsoft 会建议你将密码的强度设置为**强**。  
  
    > [!NOTE]
    >  作为一个选项，你可以选择**密码永远不会到期**。 此设置不太安全，并因此不建议。  
  
4.  单击**更改策略**。  
  
###  <a name="BKMK_Access5"></a>共享文件夹的访问权限的级别  
 作为最佳做法，你应指定的仍然允许用户，若要执行需要执行的任务的限制最多权限。  
  
 有三个访问设置适用于在服务器上的共享文件夹：  
  
-   **读取/写入**。  如果你想要允许用户帐户的权限创建、 更改，并删除共享文件夹中的任何文件，请选择该设置。  
  
-   **仅阅读**。  如果你想要允许用户的帐户权限仅阅读共享文件夹中的文件，请选择该设置。 仅阅读访问权限的用户帐户无法创建、 更改或删除任何在共享文件夹的文件。  
  
-   **不能访问**。  如果你不想要访问的任何文件共享文件夹中的用户帐户，请选择该设置。  
  
###  <a name="BKMK_Access6"></a>保留和管理已删除的用户帐户的文件的访问权限  
 网络管理员可以取出用户帐户，然后选择要保存供以后使用的文件的用户。 在此情况下，已删除的用户帐户不会再用于登录到该网络;但是，该用户的文件将保存在共享文件夹，其中可以与其他用户共享。  
  
> [!IMPORTANT]
>  请注意，如果删除了 Microsoft 在线帐户分配的用户帐户时，还会删除在线帐户，并且用户数据，包括电子邮件、 均遵守数据保留在 Microsoft Online Services 的策略。 若要保留的用户数据的在线帐户，停用的用户帐户，而不是删除它。 有关详细信息，请参阅[用户管理在线帐户](Manage-Online-Accounts-for-Users.md)。  
  
##### <a name="to-remove-a-user-account-but-retain-access-to-the-user-s-files"></a>若要删除的用户帐户，但保留用户的文件的访问权限  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**用户**。  
  
3.  在用户帐户列表中，选择你想要删除的用户帐户。  
  
4.  在**< 用户 Account\ > 任务**窗格中，单击**用户帐户中删除**。 删除出现用户帐户向导。  
  
5.  在**你想要保留文件？**页上，请确保**删除的文件，包括文件历史记录备份和重定向的文件夹，对此用户帐户**复选框处于清除状态，，然后单击**下一步**。  
  
     确认页面显示警告你要删除的帐户，但保存的文件。  
  
6.  单击**删除帐户**要删除的用户帐户。  
  
 用户帐户删除后，管理员可以允许其他用户帐户访问共享的文件夹。  
  
##### <a name="to-give-a-user-account-permission-to-access-a-shared-folder"></a>若要提供访问共享的文件夹的用户帐户权限  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**存储**，然后单击**服务器文件夹**选项卡。  
  
3.  在列表中的文件夹，请选择**用户**文件夹。  
  
4.  在**用户任务**窗格中，单击**打开该文件夹**。 Windows 资源管理器将打开并显示的内容**用户**文件夹。  
  
5.  右键单击你想要共享，然后单击用户帐户的文件夹**属性**。  
  
6.  在**< 用户 Account\ > 属性**，单击**共享**选项卡，然后单击**共享**。  
  
7.  在**文件共享**窗口中，键入，或选择对其你想要共享的文件夹，然后单击的用户帐户名**添加**。  
  
8.  选择**权限级别**您希望的用户帐户进行，然后单击**共享**。  
  
###  <a name="BKMK_Access7"></a>与网络管理员密码同步 DSRM 密码  
 目录服务还原模式 (DSRM) 是一种特殊启动模式来修复或恢复 Active Directory。 操作系统用于 DSRM 如果 Active Directory 失败，或需要还原登录到计算机。 如果你的网络管理员密码和 DSRM 密码不同，将不会加载 DSRM。  
  
 全新的、 首次在安装期间 Windows Server Essentials，该程序将 DSRM 密码设置为你指定在安装过程或迁移答案文件中的网络管理员帐户密码。 更改你的网络管理员密码 （如推荐通常提高的服务器安全每隔 60 天） 时，不为 DSRM 转发密码更改。 这将导致密码不匹配。 如果发生这种情况，可以使用下面的解决方案到手动或自动将你的网络管理员的密码同步 DSRM 密码。  
  
##### <a name="to-manually-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>若要手动同步到的网络管理员帐户 DSRM 密码  
  
1.  在命令提示符下运行`ntdsutil.exe`打开 ntdsutil 工具。  
  
2.  若要重置 DSRM 密码，请键入**设置 dsrm 密码**。  
  
3.  若要同步的当前的网络管理员 s 帐户该域控制器上的 DSRM 密码，键入：  
  
     **从帐户同步** *< current_network_administrator_account >*，然后按 Enter。  
  
 因为您会定期更改的网络管理员帐户，以确保 DSRM 密码始终是与网络管理员联系的当前密码相同，我们建议你自动创建的日程安排任务的密码同步到的网络管理员密码 DSRM 密码每天。  
  
##### <a name="to-automatically-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>若要自动同步到的网络管理员帐户 DSRM 密码  
  
1.  在服务器上，打开**管理工具**，然后双击**任务计划程序**。  
  
2.  在任务计划程序**操作**窗格中，单击**创建任务**。  
  
3.  在**名称**文本框中键入一个名称，如任务**真实 DSRM 密码**，然后选择**运行使用最高权限**选项。  
  
4.  定义何时运行该任务：  
  
    1.  在**创建任务**对话框中，单击**触发器**选项卡，然后单击**新建**。  
  
    2.  在**新触发器**对话框中，选择定期选项，指定定期的时间间隔，然后选择开始时间。  
  
        > [!NOTE]
        >  作为最佳做法，你应该将运行每天在非商业时间任务设置。  
  
    3.  单击**确定**以保存所做的更改并返回到**创建任务**对话框。  
  
5.  定义任务操作：  
  
    1.  单击**操作**选项卡，然后单击**新建**。 **新操作**显示对话框。  
  
    2.  在**操作**列表中，单击**启动程序**，然后浏览到**C:\WINDOWS\SYSTEM32\ntdsutil.exe**。  
  
    3.  在**添加参数**（可选） 的文本框中，键入以下命令 （你必须包括引号）：**域帐户 SBS_network_administrator_account q q 从设置 dsrm 密码同步**位置*SBS_network_administrator_account*是当前的网络管理员的帐户名称。  
  
6.  单击**确定**两次，保存任务，并关闭**创建任务**对话框。 新的任务将显示在**活动任务**部分**任务计划**。  
  
###  <a name="BKMK_Access8"></a>提供远程桌面权限的用户帐户  
 在 Windows Server Essentials 的默认安装，网络用户没有建立远程连接到计算机或其他网络上的资源的权限。  
  
 网络用户可以建立远程连接到网络资源之前，你必须首次设置安装任何地方访问。 你设置的任何位置访问权限后，用户可以从连接到 Internet 的任何位置设备访问文件、 应用和 office 网络中的计算机。  
  
 随时随地访问向导设置允许你启用的远程访问两种方法：  
  
-   虚拟专用网络 (VPN)  
  
-   Web 远程访问  
  
 该向导运行时，还可以选择允许任何地方访问所有的当前和新添加的用户帐户。  
  
 设置任何地方访问权限，请打开仪表板**家庭**页上，单击**设置**，然后单击**设置任何地方访问**。  
  
 有关任何地方访问的详细信息，请参阅[管理任何地方访问](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)。  
  
###  <a name="BKMK_Access9"></a>使用户可以访问在服务器上的资源  
  本部分适用于运行 Windows Server Essentials 或 Windows Server Essentials，服务器或安装了 Windows Server Essentials 体验角色与运行 Windows Server 2012 R2 标准版或 Windows Server 2012 R2 Datacenter server。  
  
 如果你希望用户使用远程访问，以及/或者具有每个用户帐户，完连接到服务器的计算机后，你可以创建新的网络联网计算机的用户的用户帐户在服务器上通过仪表板。 有关创建用户帐户的详细信息，请参阅[添加用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)。 后创建用户帐户，你必须网络用户的名称和密码信息用户提供的客户端计算机，以便他们可以通过使用启动栏访问服务器上的资源。  
  
 为你创建的每个用户帐户可以设置的以下通过用户访问属性帐户：  
  
-   **共享文件夹**。  默认情况下，网络管理员已**读取/写入**权限的所有共享的文件夹，并标准用户帐户已**只读**公司文件夹的权限。 启用媒体流式传输时，你可以指定文件夹的每个标准用户帐户的访问权限，为以下共享文件夹：**音乐**，**图片**，**录制的电视**，并**视频**。 若要访问在共享的文件夹的用户帐户的权限可以设置**共享文件夹**选项卡上的用户帐户属性。  
  
-   **随时随地访问**。  默认情况下，网络管理员可以访问服务器资源使用 VPN 或远程访问 Web。 对于标准用户帐户，必须上设置用户帐户的权限**任何地方访问**选项卡。  
  
-   **计算机的访问权限**。  默认情况下，网络管理员可以访问该网络中的所有计算机。 但是，对于标准用户帐户可以设置访问网络上的计算机上的单个用户帐户权限**计算机的访问权限**选项卡上的用户帐户属性。  
  
##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012-r2"></a>若要编辑在 Windows Server Essentials 2012 R2 的用户帐户属性  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**用户**。  
  
3.  在用户帐户列表中，选择你想要编辑的用户帐户。  
  
4.  在**< 用户 Account\ > 任务**窗格中，单击**查看帐户属性**。  
  
5.  在**< 用户 Account\ > 属性**，请执行以下操作：  
  
    1.  在**共享文件夹**选项卡上，根据需要设置相应的文件夹权限的每个共享的文件夹。  
  
    2.  在**任何地方访问**选项卡：  
  
        1.  若要使用户可以使用 VPN 连接到服务器，请选择**允许虚拟专用网络 (VPN)**复选框。  
  
        2.  若要使用户可以通过使用远程连接到服务器，请选择**允许远程网站访问和 web 服务的应用程序访问**复选框。  
  
    3.  在**计算机的访问权限**选项卡上，选择你想要用户可以拥有访问权限的网络计算机。  
  
##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012"></a>若要编辑在 Windows Server Essentials 2012 的用户帐户属性  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**用户**。  
  
3.  在用户帐户列表中，选择你想要编辑的用户帐户。  
  
4.  在**< 用户 Account\ > 任务**窗格中，单击**属性**。  
  
5.  在**< 用户 Account\ > 属性**，请执行以下操作：  
  
    1.  在**常规**选项卡上，选择**用户可以查看网络健康警报**如果用户帐户必须访问网络健康报告。  
  
    2.  在**共享文件夹**选项卡上，根据需要设置相应的文件夹权限的每个共享的文件夹。  
  
    3.  在**任何地方访问**选项卡：  
  
        1.  若要使用户可以使用 VPN 连接到服务器，请选择**允许虚拟专用网络 (VPN)**复选框。  
  
        2.  若要使用户可以通过使用远程连接到服务器，请选择**允许远程网站访问和 web 服务的应用程序访问**复选框。  
  
    4.  在**计算机的访问权限**选项卡上，选择你想要用户可以拥有访问权限的网络计算机。  
  
###  <a name="BKMK_Access10"></a>更改远程访问权限的用户帐户  
 用户可以访问位于远程服务器使用虚拟专用网络 (VPN)、 远程 Web 访问或其他 web 服务的应用程序的资源。 默认情况下，远程访问权限的网络用户时打开 Windows Server Essentials 中的任何地方访问配置通过仪表板。  
  
##### <a name="to-change-remote-access-permissions-for-a-user-account"></a>若要更改远程访问权限的用户帐户  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**用户**。  
  
3.  在用户帐户列表中，选择你想要更改用户帐户。  
  
4.  在**< 用户 Account\ > 任务**窗格中，单击**查看帐户属性**。 **属性**页面的显示的用户帐户。  
  
5.  在**任何地方访问**选项卡上，请执行以下操作：  
  
    -   选择**允许虚拟专用网络 (VPN)** ，以使用户可以通过使用 VPN 连接到服务器的复选框。  
  
    -   选择**允许远程网站访问和 web 服务的应用程序访问**，以使用户可以通过使用远程连接到服务器的复选框。  
  
6.  单击**应用**，然后单击**确定**。  
  
###  <a name="BKMK_Access11"></a>更改虚拟专用网络的用户帐户的权限  
 虚拟专用网络 (VPN) 可用于连接到 Windows Server Essentials 和访问存储在服务器的你所有资源。 这是非常有用，如果你有可用于连接到 VPN 连接通过托管的 Windows Server Essentials 服务器的网络帐户与设置 client 计算机。 托管 Windows Server Essentials 服务器上的所有新创建的用户帐户必须使用 VPN 可首次登录到 client 计算机。  
  
##### <a name="to-change-vpn-permissions-for-network-users"></a>若要更改的网络用户的 VPN 权限  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**用户**。  
  
3.  在用户帐户列表中，选择你想要授予权限才能远程访问桌面的用户帐户。  
  
4.  在**< 用户 Account\ > 任务**窗格中，单击**属性**。  
  
5.  在**< 用户 Account\ > 属性**，单击**任何地方访问**选项卡。  
  
6.  在**任何地方访问**选项卡上，可使用户可以通过使用 VPN 连接到服务器选择**允许虚拟专用网络 (VPN)**复选框。  
  
7.  单击**应用**，然后单击**确定**。  
  
###  <a name="BKMK_Access12"></a>更改访问内部共享文件夹的用户帐户  
 你可以使用任务上管理任何服务器上的共享文件夹的访问**服务器文件夹**仪表板中的选项卡。 默认情况下，当你安装了 Windows Server Essentials 创建服务器以下文件夹：  
  
-   **客户端计算机备份**。  用来存储由 Windows Server 备份创建的客户端计算机备份。 此服务器文件夹将不会共享。  
  
-   **公司**。  用来存储和访问与你的组织网络用户的相关文档。  
  
-   **文件历史记录备份**。  默认情况下，Windows Server Essentials 存储文件备份创建的使用文件历史记录。 此服务器文件夹将不会共享。  
  
-   **文件夹重定向**。  用来存储和访问已设置为文件夹重定向网络用户的文件夹。 此服务器文件夹将不会共享。  
  
-   **音乐**。  用来存储和访问网络用户的音乐文件。 当你打开 media 共享创建该文件夹。  
  
-   **图片**。  用来存储和访问网络用户的图片。 当你打开 media 共享创建该文件夹。  
  
-   **录制的电视节目**。  用来存储和访问网络用户录制的电视节目。 当你打开 media 共享创建该文件夹。  
  
-   **视频**。  用来存储和访问网络用户的视频。 当你打开 media 共享创建该文件夹。  
  
-   **用户**。  用来存储和访问网络用户的文件。 用户特定文件夹将自动在生成**用户**服务器为你创建的每个网络用户帐户的文件夹。  
  
##### <a name="to-change-access-to-a-shared-folder-for-a-user-account"></a>若要更改用户帐户的共享文件夹的访问权限  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  单击**存储**，然后单击**服务器文件夹**。  
  
3.  导航到，然后选择服务器文件夹您要修改的权限。  
  
4.  在任务窗格中，单击**查看文件夹属性**。  
  
5.  在**< FolderName\ > 属性**，单击**共享**，并选择列出的用户帐户适当的用户的访问权限级别，然后单击**应用**。  
  
    > [!NOTE]
    >  你无法修改的共享权限**文件历史记录备份**，**文件夹重定向**，并**用户**服务器文件夹。 因此，这些服务器文件夹的文件夹属性不包括**共享**选项卡。  
  
###  <a name="BKMK_Access13"></a>允许建立远程桌面会话向他们的计算机的用户帐户  
  本部分适用于运行 Windows Server Essentials 或 Windows Server Essentials，服务器或安装了 Windows Server Essentials 体验角色与运行 Windows Server 2012 R2 标准版或 Windows Server 2012 R2 Datacenter server。  
  
 网络管理员可以允许远程访问他们的网络计算机的网络用户授予权限。  
  
##### <a name="to-enable-users-to-access-their-network-computers-from-a-remote-location"></a>若要使用户可以从远程位置访问他们的网络计算机  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**用户**。  
  
3.  在用户帐户列表中，选择你想要为远程访问桌面授予权限的用户帐户。  
  
4.  在**< 用户 Account\ > 任务**窗格中，单击**属性**。  
  
5.  在**< 用户 Account\ > 属性**，单击**计算机的访问权限**选项卡。  
  
6.  选择你想要将无法访问远程，，然后单击此用户帐户的计算机**确定**。  
  
## <a name="see-also"></a>请参阅  
  
-   [管理用户在线帐户](Manage-Online-Accounts-for-Users.md)  
  
-   [连接](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
