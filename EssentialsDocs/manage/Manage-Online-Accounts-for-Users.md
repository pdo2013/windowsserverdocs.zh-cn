---
title: 管理 Windows Server Essentials 用户的联机帐户
description: 介绍如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824088"
---
# <a name="manage-online-accounts-for-windows-server-essentials-users"></a>管理 Windows Server Essentials 用户的联机帐户

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

将与 Microsoft Office 365 集成在 Windows Server Essentials 服务器，您可以从仪表板管理你的联机帐户以及用户帐户。 在本主题中，您将了解通过从仪表板管理你的 Microsoft Online Services 帐户用户可以获得哪些优势、 如何创建和管理联机帐户仪表板中，以及如何管理 Exchange Online 的电子邮件地址和通讯组从仪表板。  

  
> [!NOTE]
>  若要管理你在 Windows Server Essentials 中的 Microsoft Online Services 帐户，必须与 Office 365 集成您的服务器。 有关说明，请参阅[管理 Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)。  
  
> [!IMPORTANT]
>  如果您使用 Windows Server Essentials 中管理联机帐户，你习惯于看到将 Microsoft Online Services 帐户称为*Office 365 帐户*。 在 Windows Server Essentials 中仪表板，标签已更改为*Microsoft Online Services 帐户*，或*Microsoft 联机帐户*简称。 这些帐户和过程都相同；仅标签发生了更改。 本主题中的大多数过程使用术语*联机帐户*。  
  
## <a name="in-this-topic"></a>本主题内容  
  
-   [为什么要从仪表板管理我的联机帐户？](#BKMK_WhyManageOnlineAccounts)  
  
-   [创建联机帐户](#BKMK_SECTION_CreateOnlineAccounts)  
  
-   [管理联机帐户](#BKMK_SECTION_ManageOnlineAccounts)  
  
-   [管理 Exchange Online 的电子邮件地址](#BKMK_SECTION_ManageEmailAddresses)  
  
-   [管理 Exchange Online 的通讯组](#BKMK_SECTION_ManageDistributionGroups)  
  
##  <a name="BKMK_WhyManageOnlineAccounts"></a> 为什么要从仪表板管理我的联机帐户？  
 当你使用仪表板将 Microsoft Online Services 帐户分配给用户帐户时，帐户密码将自动同步，而且你可以保留一起在用户帐户 s 生命周期的两个帐户。  
  
 它方便用户，他们可以使用相同的密码来访问资源的服务器上并在 Office 365 中的 s。 并可应用于需要针对内部资源提出的 Office 365 中的资源的访问权限的相同密码要求。  
  
### <a name="how-does-password-synchronization-work"></a>如何进行密码同步？  
 当你使用仪表板将 Microsoft Online Services 帐户分配给用户帐户时，用户帐户密码是自动同步的用户 s 联机帐户。 这意味着用户只需一个密码即可访问的服务器上并在 Office 365 中的资源。 此外，您可以使用相同的名称的用户帐户和用户 s 联机 id。  
  
 当用户从加入域的计算机或使用远程 Web 访问更改其用户帐户的密码时，将立即自动同步密码。  
  
> [!IMPORTANT]
>  如果 Office 365 与 Windows Server Essentials 集成，用户不应从 Office 365 门户更改其 Microsoft 联机帐户的密码。 这样做将会中断密码同步。  
  
### <a name="simplified-account-creation"></a>简化的帐户创建过程  
 此处 s 从仪表板创建初始联机帐户时的另一个优点。 你可以通过一个操作为所有用户创建联机帐户。 另一方面，如果你的员工使用的 Office 365，和你设置新的 Windows Server Essentials 服务器，您可以创建的所有用户帐户从联机帐户的一次操作。 有关详细信息，请参阅 [创建联机帐户](#BKMK_SECTION_CreateOnlineAccounts)。  
  
### <a name="manage-email-addresses-and-distribution-groups-from-the-dashboard"></a>从仪表板管理电子邮件地址和通讯组  
 你将能够从仪表板管理 Exchange Online 的电子邮件地址和通讯组。 并且可以使用你的组织的 s Internet 域中的电子邮件地址。 您可以从仪表板中，执行所有这些都无需登录到 Office 365。 （您将需要使用 Windows Server Essentials 仪表板管理通讯组。 此功能不支持在 Windows Server Essentials 中。）有关详细信息，请参阅[管理 Exchange Online 的电子邮件地址](#BKMK_SECTION_ManageEmailAddresses)和[管理 Exchange Online 的通讯组](#BKMK_SECTION_ManageDistributionGroups)。  
  
### <a name="manage-the-user-account-and-online-account-together"></a>同时管理用户帐户和联机帐户  
 还可以管理联机帐户和用户帐户在帐户 s 生命周期。 如果停用用户帐户，则联机帐户也将在 Microsoft Online Services 中停用。 如果删除用户帐户，则联机帐户也将随之删除。 有关详细信息，请参阅[管理联机帐户](#BKMK_SECTION_ManageOnlineAccounts)。  
  
##  <a name="BKMK_SECTION_CreateOnlineAccounts"></a> 创建联机帐户  
 将你的服务器与 Office 365 集成后它向你最好创建 Microsoft Online Services 帐户为你从仪表板的用户。 您将有很多灵活地创建联机帐户。 如果你有新的 Office 365 订阅，您可以批量创建联机帐户的所有用户。 如果你已在 Office 365 中创建你的联机帐户，别无需担心。 如果设置新服务器，您可以创建你的用户帐户在服务器上通过导入联机帐户。 并创建单独的用户帐户时，或向现有的用户帐户添加联机帐户时，可以分配新的或现有的联机帐户。  
  
 **许可证要求**必须为你创建每个联机帐户的用户许可证。 检查**Office 365**仪表板以查看用户许可证数量是可通过 Office 365 订阅上的页。 如果你需要添加更多用户许可证，您将能够在 Office 365 中为此，打开你的 Office 365 订阅。  
  
 **用户跟进**添加用户帐户的联机帐户后，用户需要他们在下次登录的时更改其用户帐户的密码。 新密码将立即与其联机帐户同步。 此后，用户可以使用该密码以登录到 Office 365 和其联机 id。  
  
> [!IMPORTANT]
>  向用户强调，他们应永远不会更改其在 Office 365 中的联机帐户密码。 每当用户更改其用户帐户的密码时，该密码都会自动更改。 如果他们更改 Office 365 中的联机密码，密码同步将会中断。  
  
 使用本部分中的过程以执行以下操作：  
  
-   [现有用户帐户的批量创建联机帐户的](#BKMK_ToBulkCreateOnlineAccounts)  
  
-   [从 Microsoft Online Services 帐户导入用户](#BKMK_ToImportUserAccounts)  
  
-   [为其分配联机帐户创建新的用户帐户](#BKMK_ToCreateaNewUserAccount)  
  
-   [将联机帐户分配给用户帐户](#BKMK_ToAssignAnOnlineAccount)  
  
> [!NOTE]
>  如果您使用 Windows Server Essentials，你将看到*Office 365 帐户*而不是*Microsoft Online Services 帐户*全程。 此过程相同，但在 Windows Server Essentials 中更改了术语。  
  
###  <a name="BKMK_ToBulkCreateOnlineAccounts"></a> 若要批量创建联机帐户为现有用户帐户  
  
1.  作为管理员，在服务器上登录并打开 Windows Server Essentials 仪表板。  
  
2.  在仪表板上，打开“用户”页。  
  
3.  在“用户任务”中，单击“添加 Microsoft 联机帐户”。  
  
     向导的“添加 Microsoft Online Services 帐户”  页将显示没有 Microsoft 联机帐户的所有用户帐户。 默认情况下，所有帐户都处于选中状态，并会为 Microsoft 联机 ID 推荐用户名。 如果已将自定义 Internet 域链接到你的 Office 365 订阅，将默认使用该域。  
  
4.  在“添加 Microsoft Online Services 帐户”  页上，查看将创建的帐户。 例如，检查已具有带有不同联机 ID 的联机帐户的用户，确保已选择你要在电子邮件地址中使用的域。 完成任何所需更改后，请单击“下一步” 。  
  
5.  上**分配 Microsoft Online Services 许可证**页上，选择你的用户将使用的 Office 365 服务。 在单击“下一步”时，将开始创建帐户。  
  
    > [!NOTE]
    >  每个在线帐户，必须在 Office 365 中具有服务许可证。 你可以检查可用的许可证，并可打开你的订阅以从仪表板上的“Office 365”  页添加许可证（如有必要）。  
  
6.  通知用户现在他们已有一个 Microsoft 联机帐户。 然后可以登录到 Office 365，它们必须更改其网络用户帐户密码。 有关说明，请参阅[开始使用新的 Microsoft 联机帐户](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
###  <a name="BKMK_ToBeginUsingAnOnlineAccount"></a> 若要开始使用新的 Microsoft 联机帐户  
  
1.  使用网络用户帐户登录计算机。  
  
2.  更改用户帐户的密码。 若要开始，请按 Ctrl+Alt+Delete，然后单击“更改密码” 。  
  
     更改密码后，密码将与新的联机帐户同步。 现在可以使用相同的密码登录到 Office 365。  
  
3.  使用你的新的联机 ID 和你的用户帐户密码[登录 Office 365](https://login.microsoftonline.com/login.srf?wa=wsignin1.0&rpsnv=3&ct=1398981834&rver=6.1.6206.0&wp=MBI_SSL&wreply=https:%2F%2Foutlook.office365.com%2Fowa%2F&id=260563&CBCXT=out) 。  
  
    > [!IMPORTANT]
    >  不更改你在 Office 365 中的联机帐户密码。 否则会中断密码同步。 每次更改网络用户帐户的密码时，联机密码都将随之更新。  
  
###  <a name="BKMK_ToImportUserAccounts"></a> 若要从现有的联机帐户导入用户  
  
1.  在仪表板上，打开“用户”页。  
  
2.  在“用户任务” 中，单击“从 Office 365 导入帐户” 。  
  
     下一步的页将显示所有联机帐户将 Office 365 订阅不具有在服务器上的用户帐户。 所有的帐户默认都处于选中状态，建议将联机 ID 用作用户名。  
  
3.  创建用户帐户的步骤：  
  
    1.  对推荐的用户帐户作出所有必需的更改。  
  
    2.  还可以选择单击链接以查看将分配给用户帐户的临时密码。 你将需要为用户提供临时密码以及新的帐户名。  
  
         (您将创建帐户后，找到此文件中列出这些密码：*SystemDrive*\Users\\*Office365admin*\\*NewServerUser*.txt 其中*Office365admin*是网络帐户用于管理服务器上的 Office 365 和*NewServerUser*是新用户帐户名称。)  
  
    3.  单击“下一步”  ，创建用户帐户。  
  
4.  让用户知道他们的新用户帐户和临时密码，它们将使用在服务器上为第一个时间 œ 登录和他们将需要在登录后更改密码。 有关面向用户的说明，请参阅 [开始使用新的 Microsoft 联机帐户](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
     请确保用户了解其联机帐户密码将会与从长远看，其用户帐户同步，它们不应更改联机密码在 Office 365 中。  
  
###  <a name="BKMK_ToCreateaNewUserAccount"></a> 若要为其分配联机帐户创建新的用户帐户  
  
1.  在仪表板上单击“用户”。  
  
2.  在“用户任务”中，单击“添加用户帐户”。 将出现“添加用户帐户”向导。  
  
3.  按照说明创建用户帐户。  
  
4.  在“分配 Microsoft Online Services 帐户”页上，为用户创建一个新的联机帐户或分配一个现有联机帐户：  
  
    -   若要创建新的联机帐户，请单击“创建新的 Microsoft Online Services 帐户并将其分配给此用户帐户” ，并键入该 Microsoft Online Services 帐户的名称（默认情况下，将用户名用于联机 ID）。 然后，单击“下一步”。  
  
    -   若要分配现有的 Microsoft 联机帐户，请单击“将现有 Microsoft Online Services 帐户分配给此用户帐户” ，并从下拉列表中选择一个现有帐户。 然后，单击“下一步”。  
  
    > [!NOTE]
    >  在 Windows Server Essentials 中，Microsoft Online Services 帐户称为向导和仪表板标签中的 Office 365 帐户。  
  
5.  按照说明完成向导。  
  
6.  请通知用户，他们必须先更改其用户帐户的密码，才可以使用新的联机帐户登录到 Office 365。 有关说明，请参阅[开始使用新的 Microsoft 联机帐户](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
#### <a name="BKMK_ToAssignAnOnlineAccount"></a>若要将联机帐户分配到用户帐户  
  
1.  在仪表板上单击“用户”。  
  
2.  右键单击列表中的用户帐户，然后单击“分配 Microsoft 联机帐户” 。 将出现“分配 Microsoft Online Services 帐户”向导。  
  
3.  分配一个现有联机帐户或为用户创建一个新帐户。 新帐户的默认联机 ID 为用户名。 然后单击“下一步”  ，将联机帐户添加到用户帐户。  
  
4.  查看向导最后一页上的信息，然后单击“关闭” 。  
  
5.  请通知用户，他们必须先更改其用户帐户的密码，才可以使用新的联机帐户登录到 Office 365。 有关说明，请参阅[开始使用新的 Microsoft 联机帐户](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
##  <a name="BKMK_SECTION_ManageOnlineAccounts"></a> 管理联机帐户  
 当到 Windows Server Essentials 中的用户帐户添加联机帐户时，可以在帐户 s 生命周期一起管理这两个帐户。  
  
###  <a name="BKMK_UnderstandingAccountStatus"></a> 了解联机帐户状态  
 将 Microsoft Online Services 帐户分配给用户帐户后，该帐户的电子邮件地址将出现在仪表板的“用户”  页上的“Microsoft 联机帐户”  列中。 (在 Windows Server Essentials 中，列标签是指**Office 365 帐户**。)  
  
-   电子邮件地址旁边的蓝色图标表示该联机帐户处于活动状态。 这就是说，帐户具有当前 Office 365 许可证，而用户可以使用联机 ID 登录到 Office 365。  
  
-   电子邮件地址旁边的灰色的图标指示该联机帐户也是 œ 处于非活动状态，因为许可证不再处于活动状态或已取消分配联机帐户。 当你取消分配用户 s 联机帐户时，将删除许可证，并阻止用户在登录到 Office 365 使用的帐户。 但是，服务器将保留的用户帐户名称和 Office 365 电子邮件地址之间的映射。  
  
###  <a name="BKMK_UnassignOnlineAccount"></a> 限制对联机帐户的访问  
 如果某位用户离开你的组织，或者你想要限制对 Office 365 服务的用户的访问，则不要做什么？ 如果你管理用户联机帐户及其用户帐户在 Windows Server Essentials 中，您有三个选项：  
  
-   **取消分配联机帐户**？如果你想要防止用户使用 Office 365，而不阻止对服务器上的资源的访问，则应该取消分配联机帐户。 Office 365 许可证将被释放，并阻止用户在登录到 Office 365。 但是，服务器将保留的用户帐户名称和 Office 365 电子邮件地址之间的映射。 有关说明，请参阅[从用户帐户中取消分配联机帐户](#BKMK_ToUnassignAnOnlineAccount)。  
  
-   **停用用户帐户**？如果因为员工离职而暂时或永久性地停用用户帐户的用户 s 联机帐户也将停用。 虽然无法使用联机帐户，但用户数据（包括电子邮件）会保留在 Microsoft Online Services 中。 有关说明，请参阅[停用用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6)中[管理用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md)。  
  
-   **删除用户帐户**？如果删除用户帐户，请联机帐户也将删除从 Microsoft Online Services。 有关说明，请参阅[删除用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove)中[管理用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md)。  
  
    > [!WARNING]
    >  请注意，在删除联机帐户后，用户数据将遵循 Microsoft Online Services 的数据保留策略。 如果你需要将 person s 用户数据保留在某员工离职后，停用用户帐户而不是将其删除。  
  
####  <a name="BKMK_ToUnassignAnOnlineAccount"></a> 若要从用户帐户中取消分配联机帐户  
  
1.  在仪表板上单击“用户”。  
  
2.  右键单击列表中中的用户帐户，然后单击**取消分配 Microsoft 联机帐户**(在 Windows Server Essentials 中，单击**取消分配 Office 365 帐户**)。  
  
3.  在出现确认提示时，单击“是” 。  
  
##  <a name="BKMK_SECTION_ManageEmailAddresses"></a> 管理 Exchange Online 的电子邮件地址  
 通过将电子邮件地址添加到 Windows Server Essentials 中的用户 s 联机帐户，可以允许用户以联机 Exchange 中接收从多个电子邮件地址的电子邮件。  
  
###  <a name="BKMK_PROC_AddEmailAliases"></a> 若要将其他电子邮件地址添加到用户的 Microsoft 联机帐户  
  
1.  在 Windows Server Essentials 仪表板中，单击**用户**。  
  
2.  右键单击列表中的用户帐户，然后单击“查看帐户属性” 。  
  
3.  上**Microsoft 在线**帐户属性的选项卡 (或**Office 365** Windows Server Essentials 中的选项卡)，单击**添加**。  
  
4.  键入新的电子邮件别名，然后选择电子邮件域。  
  
5.  单击“确定”两次  。  
  
##  <a name="BKMK_SECTION_ManageDistributionGroups"></a> 管理 Exchange Online (仅限 Windows Server Essentials) 的分发组  
 将 Windows Server Essentials 服务器与 Office 365 集成后，可以创建和管理的 Windows Server Essentials 仪表板从 Exchange Online 通讯组。 您将执行此操作上**通讯组**选项卡添加到**用户**页。 如果你有 Exchange Online 订阅，你将仅看到此选项卡。 此功能不是 Windows Server Essentials 中可用。  
  
 使用下面的过程，以：  
  
-   [添加通讯组](#BKMK_PROCEDURE_AddDistGroup)  
  
-   [更改通讯组的成员](#BKMK_ChangeGroupMembers)  
  
-   [更改用户的通讯组成员身份](#BKMK_EditUserMemberships)  
  
-   [删除通讯组](#BKMK_RemoveDistributionGroup)  
  
###  <a name="BKMK_PROCEDURE_AddDistGroup"></a> 若要添加通讯组  
  
1.  在 Windows Server Essentials 中仪表板，单击**用户**，然后单击**通讯组**选项卡。  
  
2.  在“通讯组任务”中单击“添加通讯组”。  
  
     此时将显示“添加新的通讯组”向导。  
  
3.  在“添加新的通讯组”页上，输入以下信息，然后单击“下一步”：  
  
    -   输入新通讯组的组名、可选描述和电子邮件别名。  
  
    -   默认情况下，通讯组可以接收来自组织以外的人的电子邮件。 如果不想允许此操作，请清除该选项。  
  
4.  在“添加组成员”页上，使用“添加”按钮将活动的用户帐户（已为其分配联机帐户）和其他通讯组添加到新的通讯组。 然后，单击“下一步”。  
  
     已在 Exchange Online 中创建新的通讯组。  
  
###  <a name="BKMK_ChangeGroupMembers"></a> 若要更改分发组的成员  
  
1.  在仪表板上单击“用户”，然后单击“通讯组”选项卡。  
  
2.  右键单击列表中的通讯组，然后单击“更改组成员身份” 。  
  
3.  使用“添加”  和“删除”  按钮从通讯组添加或删除活动的联机帐户。 然后单击“下一步”更新 Exchange Online 中的通讯组成员身份。  
  
###  <a name="BKMK_EditUserMemberships"></a> 若要更改用户的通讯组成员身份  
  
1.  在仪表板上单击“用户”。  
  
2.  右键单击列表中的用户帐户，然后单击“查看帐户属性” 。  
  
3.  在用户帐户属性中，单击“通讯组”选项卡，然后单击“编辑”。  
  
4.  在“编辑组成员身份”  框中，使用“添加”  和“删除”  按钮从用户帐户添加或删除通讯组，然后单击“关闭” 。  
  
5.  单击“确定”以保存更新后的用户帐户属性。  
  
###  <a name="BKMK_RemoveDistributionGroup"></a> 若要删除通讯组  
  
1.  在仪表板上单击“用户”，然后单击“通讯组”选项卡。  
  
2.  右键单击列表中的通讯组，然后单击“删除该组”。  
  
3.  在出现确认提示时，单击“删除组” 。  
  
     通讯组将从 Exchange Online 中删除。  
  
## <a name="see-also"></a>请参阅  
  
-   [管理用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  
-   [管理 Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [Manage Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
