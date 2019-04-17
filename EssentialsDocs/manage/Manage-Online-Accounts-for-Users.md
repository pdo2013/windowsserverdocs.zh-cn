---
title: "对于 Windows Server Essentials 用户管理在线帐户"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c09f4cf6-4d12-49fe-9ae4-e6cb14027b9d
8author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 95f401bbec9bb503d19e2d9918a05851c04f7ef4
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="manage-online-accounts-for-windows-server-essentials-users"></a>对于 Windows Server Essentials 用户管理在线帐户

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

当您使用 Microsoft Office 365 集成 Windows Server Essentials 服务器时，你可以从仪表板管理用户帐户以及您在线帐户。 在本主题中，您将了解能够通过从仪表板中管理您的用户 Microsoft Online 服务帐户、如何创建和管理在线帐户仪表板中，从和如何用于在仪表板中联机 Exchange 管理电子邮件地址和 distribution 组。  

  
> [!NOTE]
>  若要管理 Microsoft Online 服务帐户中的 Windows Server Essentials，必须具有 Office 365 集成服务器。 有关说明，请参阅[管理 Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)。  
  
> [!IMPORTANT]
>  如果你管理 Windows Server Essentials 中的在线帐户，你看到称为 Microsoft Online 服务帐户习惯*Office 365 帐户*。 在 Windows Server Essentials 仪表板上变为标签*Microsoft Online 服务帐户*，或*Microsoft 在线帐户*简称。 帐户和过程都相同。仅限更改标签。 本主题中的大多数过程使用术语*在线帐户*。  
  
## <a name="in-this-topic"></a>在本主题  
  
-   [为什么要仪表板中管理我在线帐户？](#BKMK_WhyManageOnlineAccounts)  
  
-   [创建在线帐户](#BKMK_SECTION_CreateOnlineAccounts)  
  
-   [管理在线帐户](#BKMK_SECTION_ManageOnlineAccounts)  
  
-   [有关 Exchange Online 管理电子邮件地址](#BKMK_SECTION_ManageEmailAddresses)  
  
-   [有关 Exchange Online 管理 distribution 组](#BKMK_SECTION_ManageDistributionGroups)  
  
##  <a name="BKMK_WhyManageOnlineAccounts"></a>为什么要仪表板中管理我在线帐户？  
 当你使用仪表板将 Microsoft Online 服务帐户分配给用户帐户时，帐户密码，将自动同步，并且你可以维护一起整个用户帐户的 s 生命周期两个帐户。  
  
 很方便，可以使用相同的密码访问资源服务器和 Office 365 中的用户。 并且你可以访问的同一密码要求适用到所需的你的内部资源的 Office 365 中的资源。  
  
### <a name="how-does-password-synchronization-work"></a>密码同步如何工作？  
 当你使用仪表板将 Microsoft Online 服务帐户分配给用户帐户时，自动与用户 s 在线帐户同步的用户帐户的密码。 这意味着用户只需访问这两个资源服务器和 Office 365 中的一个密码。 此外，你可以使用相同的名称用户帐户和用户 s 在线 id。  
  
 密码同步时立即并自动时用户通过远程访问 Web 或已加入域的计算机中更改给他们的帐户的密码。  
  
> [!IMPORTANT]
>  Office 365 与 Windows Server Essentials 集成，如果用户不应从 Office 365 门户更改他们的 Microsoft 在线帐户的密码。 这样做会让密码同步。  
  
### <a name="simplified-account-creation"></a>简化的帐户创建  
 没有其他利用时从仪表板中创建初始在线帐户。 你可以为您的所有用户的一项操作都创建在线帐户。 另一方面，如果你的员工使用的 Office 365 已，并且你设置新的 Windows Server Essentials 服务器，你可以创建的所有用户帐户的一项操作在线帐户中。 有关详细信息，请参阅[创建在线帐户](#BKMK_SECTION_CreateOnlineAccounts)。  
  
### <a name="manage-email-addresses-and-distribution-groups-from-the-dashboard"></a>管理电子邮件地址和 distribution 组从仪表板  
 你将能够管理你的电子邮件地址和 distribution 组用于在仪表板中联机 Exchange。 并且你可以使用你的组织的 Internet 域中的电子邮件地址。 你可以从仪表板上，执行所有这些无需登录到 Office 365。 （你将需要在使用 Windows Server Essentials 从仪表板中管理 distribution 组。 不是支持此功能在 Windows Server Essentials。）有关详细信息，请参阅[管理电子邮件地址的 Exchange Online](#BKMK_SECTION_ManageEmailAddresses)和[管理 Exchange Online distribution 组](#BKMK_SECTION_ManageDistributionGroups)。  
  
### <a name="manage-the-user-account-and-online-account-together"></a>管理用户帐户和网上帐户一起  
 并且你可以管理的帐户的 s 生命周期的用户帐户以及在线帐户。 停用的用户帐户时，如果在线帐户也已停用的 Microsoft Online 服务中。 如果你要删除的用户帐户，联机也被删除帐户。 有关详细信息，请参阅[管理在线帐户](#BKMK_SECTION_ManageOnlineAccounts)。  
  
##  <a name="BKMK_SECTION_CreateOnlineAccounts"></a>创建在线帐户  
 与 Office 365 集成服务器后，它是为您从仪表板中创建 Microsoft Online 服务的您的用户的帐户的优势。 你将有大量灵活地创建在线帐户。 如果你有一个全新的 Office 365 订阅，你可以批量-创建在线帐户的所有用户。 如果您在 Office 365 中已经创建在线帐户，也不用担心。 如果你设置新的服务器时，你可以创建用户帐户在服务器上通过导入在线帐户。 当您创建的单个用户帐户或在线帐户添加到现有的用户帐户时，从而将新的或现有的在线帐户。  
  
 **许可要求**你将需要为每个你创建的在线帐户的用户许可证。 检查**Office 365**仪表板多少用户许可证有可通过你的 Office 365 订阅上的页面。 如果你需要添加更多的用户许可证时，你将能够在若要执行该操作的 Office 365 中打开你的 Office 365 订阅。  
  
 **对于用户跟进**添加用户帐户在线帐户后，需要用户他们在下次登录的时更改它们的用户帐户的密码。 立即与他们的在线帐户同步的新密码。 在此之后，他们可以使用密码登录到 Office 365 与他们的在线 id。  
  
> [!IMPORTANT]
>  为你的用户强调他们应永远不会更改其 Office 365 中的在线帐户密码。 这些更改用户帐户的密码时，会自动更改密码。 如果他们更改 Office 365 中的联机密码，密码同步会中断。  
  
 使用本部分中的步骤：  
  
-   [批量创建在线帐户的现有的用户帐户](#BKMK_ToBulkCreateOnlineAccounts)  
  
-   [导入 Microsoft Online 服务帐户的用户帐户](#BKMK_ToImportUserAccounts)  
  
-   [通过向它分配在线帐户创建新的用户帐户](#BKMK_ToCreateaNewUserAccount)  
  
-   [将在线帐户分配给用户帐户](#BKMK_ToAssignAnOnlineAccount)  
  
> [!NOTE]
>  如果你使用 Windows Server Essentials 时，你将看到*Office 365 帐户*而不是*Microsoft Online 服务帐户*整个这些步骤。 该进程是相同，但术语更改在 Windows Server Essentials。  
  
###  <a name="BKMK_ToBulkCreateOnlineAccounts"></a>批量创建在线帐户的现有的用户帐户  
  
1.  登录到服务器以 administrator 身份，并打开对 Windows Server Essentials 仪表板。  
  
2.  在仪表板上，打开**用户**页面。  
  
3.  在**用户任务**，单击**添加 Microsoft 在线帐户**。  
  
     **将 Microsoft Online 服务的帐户添加**向导页面显示没有 Microsoft 在线帐户中的所有用户帐户。 默认情况下，选择的所有帐户，并用户名建议的 Microsoft 在线 id。 如果已链接到你的 Office 365 订阅的自定义 Internet 域，将默认情况下使用此域。  
  
4.  在**将 Microsoft Online 服务的帐户添加**页上，查看将创建的帐户。 例如，用户已有在线帐户使用不同的联机 ID，并确保选择你想要使用的电子邮件地址域是检查。 当你完成进行任何所需的更改，请单击**下一步**。  
  
5.  在**指定 Microsoft Online 服务许可证**页上，选择您的用户将使用的 Office 365 服务。 在你单击时**下一步**、创建帐户将开始。  
  
    > [!NOTE]
    >  你必须服务许可 Office 365 中的每个在线帐户。 你可以检查可用的许可证并根据需要可打开你的订阅，若要添加的许可证**Office 365**仪表板上的页面。  
  
6.  通知用户它们现在有了 Microsoft 在线帐户。 可以登录到 Office 365 前，他们必须更改其网络用户帐户的密码。 有关说明，请参阅[开始使用新的 Microsoft 在线帐户](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
###  <a name="BKMK_ToBeginUsingAnOnlineAccount"></a>若要开始使用新的 Microsoft 在线帐户  
  
1.  登录你的计算机使用你的网络的用户帐户。  
  
2.  更改你的用户帐户的密码。 若要开始使用，请按 Ctrl + Alt + Delete，然后单击**更改密码**。  
  
     更改你的密码时，密码同步通过新的在线帐户。 你现在可以使用相同的密码登录到 Office 365 中。  
  
3.  [登录到 Office 365](https://login.microsoftonline.com/login.srf?wa=wsignin1.0&rpsnv=3&ct=1398981834&rver=6.1.6206.0&wp=MBI_SSL&wreply=https:%2F%2Foutlook.office365.com%2Fowa%2F&id=260563&CBCXT=out)使用您的新在线 ID 和您用户帐户的密码。  
  
    > [!IMPORTANT]
    >  不要更改你的 Office 365 中的在线帐户密码。 这会让密码同步。 将更改你的网络的用户帐户的密码的每次更新你的联机密码。  
  
###  <a name="BKMK_ToImportUserAccounts"></a>若要导入你现有的在线帐户中的用户帐户  
  
1.  在仪表板上，打开**用户**页面。  
  
2.  在**用户任务**，单击**从 Office 365 导入帐户**。  
  
     下一步页面显示所有在线帐户为你的 Office 365 订阅不具有服务器上的用户帐户。 默认情况下，选择的所有帐户，并在线 ID 建议的用户名。  
  
3.  若要创建用户帐户：  
  
    1.  进行任何更改所需的提出的用户帐户。  
  
    2.  （可选），请单击链接以查看将分配给用户帐户的临时密码。 你将需要为你的用户提供以及他们新的帐户名称其临时密码。  
  
         (你创建的帐户后，你将找到此文件中列出的这些密码：*系统驱动器*\Users\\*Office365admin*\\*NewServerUser*.txt 了*Office365admin*是用于管理服务器上的 Office 365 的网络帐户和*NewServerUser*为新的用户帐户名。)  
  
    3.  单击**下一步**创建用户帐户。  
  
4.  让用户知道新用户帐户并将使用它们登录到的第一次 œ 服务器，他们将需要更改密码后，他们登录的临时密码。 你的用户的说明，请参阅[开始使用新的 Microsoft 在线帐户](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
     请确保他们知道将与他们接下来的用户帐户同步的他们在线帐户的密码，并它们应该不会更改其 Office 365 中的联机密码。  
  
###  <a name="BKMK_ToCreateaNewUserAccount"></a>若要通过向它分配在线帐户创建新的用户帐户  
  
1.  在仪表板中，单击**用户**。  
  
2.  在**用户任务**，单击**添加用户帐户**。 此时将显示添加用户帐户向导。  
  
3.  按照说明创建用户帐户。  
  
4.  在**指定 Microsoft Online 服务帐户**页上，可以创建一个新联机用户帐户或现有的在线帐户分配：  
  
    -   若要创建新的在线帐户，请单击**创建新 Microsoft Online 服务帐户并将其分配给用户帐户**，然后键入的 Microsoft Online 服务的帐户名称（默认情况下的用户名用于联机 ID）。 然后单击**下一步**。  
  
    -   若要指定现有的 Microsoft 在线帐户，请单击**分配给用户帐户的现有的 Microsoft Online 服务帐户**，然后从下拉列表中选择现有帐户。 然后单击**下一步**。  
  
    > [!NOTE]
    >  在 Windows Server Essentials Microsoft Online 服务的帐户被称为向导和标签仪表板中的 Office 365 帐户。  
  
5.  按照说明来完成向导。  
  
6.  通知的用户，他们需要更改其用户帐户的密码，然后他们可以登录到 Office 365 与他们的新在线帐户。 有关说明，请参阅[开始使用新的 Microsoft 在线帐户](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
#### <a name="BKMK_ToAssignAnOnlineAccount"></a>若要将在线帐户分配给用户帐户  
  
1.  在仪表板中，单击**用户**。  
  
2.  用户帐户在列表中，右键单击，然后单击**指定 Microsoft 在线帐户**。 Microsoft Online 服务的帐户向导将显示分配。  
  
3.  分配现有在线帐户，或创建一个新的用户。 一个新帐户默认联机 ID 为用户名。 然后单击**下一步**在线帐户添加到用户帐户。  
  
4.  查看该向导的最后一页上的信息，然后单击**关闭**。  
  
5.  通知的用户，他们需要更改其用户帐户的密码，然后他们可以登录到 Office 365 与他们的新在线帐户。 有关说明，请参阅[开始使用新的 Microsoft 在线帐户](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
##  <a name="BKMK_SECTION_ManageOnlineAccounts"></a>管理在线帐户  
 当你添加到 Windows Server Essentials 中的用户帐户的在线帐户时，你可以在帐户的 s 生命周期一起管理这两个帐户。  
  
###  <a name="BKMK_UnderstandingAccountStatus"></a>了解在线帐户状态  
 当你将 Microsoft Online 服务帐户分配给用户帐户时，该帐户的电子邮件地址将显示在**Microsoft 在线帐户**上的列**用户**仪表板中的页面。 (在 Windows Server Essentials 是列标签**Office 365 帐户**。)  
  
-   与电子邮件地址旁边的蓝色图标指示在线帐户处于活动状态。 也就是说，帐户具有的当前的 Office 365 许可证和用户可以使用联机 ID 登录到 Office 365 中。  
  
-   电子邮件地址旁边显示为灰色的图标表示在线帐户可能是处于非活动状态 œ，因为该许可证不再处于活动状态，或者在线帐户已被未分配。 当你取消分配给用户 s 在线帐户时，该许可证将被删除，并会阻止用户在登录到 Office 365 使用该帐户。 但是，服务器维护之间的用户帐户名的 Office 365 电子邮件地址的映射。  
  
###  <a name="BKMK_UnassignOnlineAccount"></a>限制在线帐户访问  
 如果用户都离开你的组织中，或你想要与你的 Office 365 服务限制用户 s 访问权限，则该怎么办？ 如果你管理你的用户在线帐户以及 Windows Server Essentials 他们用户的帐户，你有三个选项：  
  
-   **取消分配的在线帐户**？如果你想要保留的服务器上的资源阻止访问的情况下使用 Office 365 的用户，你应该取消分配的在线帐户。 Office 365 许可证将发布，并会阻止用户在登录到 Office 365。 但是，服务器维护之间的用户帐户名的 Office 365 电子邮件地址的映射。 有关说明，请参阅[要取消分配给用户帐户在线帐户](#BKMK_ToUnassignAnOnlineAccount)。  
  
-   **停用的用户帐户**？如果你停用的用户帐户，因为员工离开暂时或永久用户 s 在线帐户还已停用。 不能使用在线帐户，但用户数据，包括电子邮件、保留在 Microsoft Online 服务。 有关说明，请参阅[停用的用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6)中[管理用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md)。  
  
-   **用户帐户中删除**？如果你要删除的用户帐户，在线帐户已经将删除从 Microsoft Online 服务。 有关说明，请参阅[用户帐户中删除](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove)中[管理用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md)。  
  
    > [!WARNING]
    >  请注意，当移除在线帐户时，用户数据遵循数据保留策略 Microsoft Online Services。 如果你需要保留用户的用户数据的员工离开后，停用的用户帐户，而不是删除它。  
  
####  <a name="BKMK_ToUnassignAnOnlineAccount"></a>若要取消在线帐户分配给用户帐户  
  
1.  在仪表板中，单击**用户**。  
  
2.  用户帐户在列表中，右键单击，然后单击**取消分配的 Microsoft 在线帐户**(在 Windows Server Essentials、单击**取消分配的 Office 365 帐户**)。  
  
3.  在确认提示符下，单击**是**。  
  
##  <a name="BKMK_SECTION_ManageEmailAddresses"></a>有关 Exchange Online 管理电子邮件地址  
 通过在 Windows Server Essentials 用户 s 在线帐户中添加电子邮件地址，您可以允许用户联机在 Exchange 接收电子邮件，在多个电子邮件地址。  
  
###  <a name="BKMK_PROC_AddEmailAliases"></a>若要向用户 Microsoft 在线帐户的 s 添加其他电子邮件地址  
  
1.  在 Windows Server Essentials 仪表板中，单击**用户**。  
  
2.  用户帐户在列表中，右键单击，然后单击**查看帐户属性**。  
  
3.  在**Microsoft 联机**选项卡上的帐户属性 (或**Office 365** Windows Server Essentials 中的选项卡)，请单击**添加**。  
  
4.  键入新的电子邮件别名，，然后选择电子邮件域。  
  
5.  单击**确定**两次。  
  
##  <a name="BKMK_SECTION_ManageDistributionGroups"></a>有关 Exchange Online (仅适用于 Windows Server Essentials) 管理 distribution 组  
 你的 Office 365 集成到 Windows Server Essentials 服务器后，你可以创建和管理 Exchange Online 从 Windows Server Essentials 仪表板 distribution 组。 将你执行此操作上**分发组**添加到的选项卡**用户**页面。 如果你有一份 Exchange Online 订阅，你将仅看到此选项卡。 不适用于 Windows Server Essentials 此功能。  
  
 使用到以下过程：  
  
-   [添加 distribution 组](#BKMK_PROCEDURE_AddDistGroup)  
  
-   [更改 distribution 组成员](#BKMK_ChangeGroupMembers)  
  
-   [更改用户 s distribution 组成员资格](#BKMK_EditUserMemberships)  
  
-   [删除 distribution 组](#BKMK_RemoveDistributionGroup)  
  
###  <a name="BKMK_PROCEDURE_AddDistGroup"></a>若要添加 distribution 组  
  
1.  在 Windows Server Essentials 仪表板上，单击**用户**，然后单击**分发组**选项卡。  
  
2.  在**Distribution 组任务**，单击**添加 distribution 组**。  
  
     添加新的 Distribution 组向导显示。  
  
3.  在**添加新 distribution 组**页上，输入以下信息，然后单击**下一步**:  
  
    -   输入新的 distribution 组组的名称、可选说明，并电子邮件别名。  
  
    -   默认情况下，distribution 组可以从你组织外部人脉接收电子邮件。 如果你不希望允许此操作，请清除该选项。  
  
4.  在**添加的组成员**页上，使用**添加**按钮以添加具有联机分配给这些帐户的活动的用户帐户和其他 distribution 分组到新 distribution 组。 然后单击**下一步**。  
  
     分发创建新组在 Exchange Online。  
  
###  <a name="BKMK_ChangeGroupMembers"></a>若要更改 distribution 组成员  
  
1.  在仪表板中，单击**用户**，然后单击**分发组**选项卡。  
  
2.  通讯组在列表中，右键单击，然后单击**更改组成员**。  
  
3.  使用**添加**和**删除**按钮来添加或删除 distribution 组中的在线活动的帐户。 然后单击**下一步**更新分发组成员 Exchange Online 中的。  
  
###  <a name="BKMK_EditUserMemberships"></a>若要更改用户 s distribution 组成员资格  
  
1.  在仪表板中，单击**用户**。  
  
2.  用户帐户在列表中，右键单击，然后单击**查看帐户属性**。  
  
3.  在用户帐户属性中，单击**分发组**选项卡，然后单击**编辑**。  
  
4.  在**编辑组成员**框中，使用**添加**和**删除**以添加或删除 distribution 组的用户帐户，然后单击的按钮**关闭**。  
  
5.  单击**确定**保存更新后的用户帐户属性。  
  
###  <a name="BKMK_RemoveDistributionGroup"></a>若要删除 distribution 组  
  
1.  在仪表板中，单击**用户**，然后单击**分发组**选项卡。  
  
2.  通讯组在列表中，右键单击，然后单击**删除该组**。  
  
3.  在确认提示符下，单击**删除组**。  
  
     Distribution 组已从 Exchange Online 中删除。  
  
## <a name="see-also"></a>请参阅  
  
-   [管理用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  
-   [管理 Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [管理 Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
