---
title: 管理 Windows Server Essentials 用户的联机帐户
description: 描述如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.topic: article
ms.assetid: c09f4cf6-4d12-49fe-9ae4-e6cb14027b9d
author: nnamuhcs
ms.author: daveba
ms.openlocfilehash: dc3170c7d5267eef6f339dc229b1b9daaf9ac9ec
ms.sourcegitcommit: 2082335e1260826fcbc3dccc208870d2d9be9306
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2019
ms.locfileid: "69980273"
---
# <a name="manage-online-accounts-for-windows-server-essentials-users"></a>管理 Windows Server Essentials 用户的联机帐户

>适用于：Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

将 Windows Server Essentials 服务器与 Microsoft Office 365 集成时, 可以从仪表板管理联机帐户以及用户帐户。 在本主题中, 你将了解你可以通过从仪表板管理用户的 Microsoft Online Services 帐户来获得哪些内容、如何从仪表板创建和管理联机帐户, 以及如何管理 Exchange Online 的电子邮件地址和通讯组在仪表板中。  

  
> [!NOTE]
>  若要在 Windows Server Essentials 中管理你的 Microsoft Online Services 帐户, 你必须将你的服务器与 Office 365 集成。 有关说明, 请参阅[管理 Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)。  
  
> [!IMPORTANT]
>  如果你在 Windows Server Essentials 中管理联机帐户, 则你习惯查看称为*Office 365 帐户*的 Microsoft online Services 帐户。 在 Windows Server Essentials 中的仪表板上, 标签已更改为*Microsoft Online Services 帐户*或*microsoft online 帐户*。 这些帐户和过程都相同；仅标签发生了更改。 本主题中的大多数过程使用术语*联机帐户*。  
  
## <a name="in-this-topic"></a>本主题内容  
  
-   [为什么应该从仪表板管理我的联机帐户？](#BKMK_WhyManageOnlineAccounts)  
  
-   [创建联机帐户](#BKMK_SECTION_CreateOnlineAccounts)  
  
-   [管理联机帐户](#BKMK_SECTION_ManageOnlineAccounts)  
  
-   [管理 Exchange Online 的电子邮件地址](#BKMK_SECTION_ManageEmailAddresses)  
  
-   [管理 Exchange Online 的通讯组](#BKMK_SECTION_ManageDistributionGroups)  
  
##  <a name="BKMK_WhyManageOnlineAccounts"></a>为什么应该从仪表板管理我的联机帐户？  
 使用仪表板将 Microsoft Online Services 帐户分配给用户帐户时, 帐户密码将自动同步, 并且你可以在整个用户帐户生命周期内同时维护这两个帐户。  
  
 它可方便用户使用相同的密码来访问服务器和 Office 365 中的资源。 并且, 你可以对在 Office 365 中访问资源所需的资源进行相同的密码要求。  
  
### <a name="how-does-password-synchronization-work"></a>如何进行密码同步？  
 使用仪表板将 Microsoft Online Services 帐户分配给用户帐户时, 用户帐户密码将与用户的联机帐户自动同步。 这意味着用户只需一个密码即可访问服务器上和 Office 365 中的资源。 此外, 你可以为用户帐户和用户联机 ID 使用相同的名称。  
  
 当用户从加入域的计算机或使用远程 Web 访问更改其用户帐户的密码时，将立即自动同步密码。  
  
> [!IMPORTANT]
>  如果 Office 365 与 Windows Server Essentials 集成, 则用户不应从 Office 365 门户更改其 Microsoft 联机帐户的密码。 这样做将会中断密码同步。  
  
### <a name="simplified-account-creation"></a>简化的帐户创建过程  
 在从仪表板创建初始联机帐户时, 还有其他优势。 你可以通过一个操作为所有用户创建联机帐户。 另一方面, 如果你的员工已在使用 Office 365, 并且你设置了新的 Windows Server Essentials 服务器, 则你可以通过一个操作从联机帐户创建所有用户帐户。 有关详细信息，请参阅 [创建联机帐户](#BKMK_SECTION_CreateOnlineAccounts)。  
  
### <a name="manage-email-addresses-and-distribution-groups-from-the-dashboard"></a>从仪表板管理电子邮件地址和通讯组  
 你将能够从仪表板管理 Exchange Online 的电子邮件地址和通讯组。 可以在电子邮件地址中使用组织的 Internet 域。 你可以从仪表板执行所有这些操作, 而无需登录到 Office 365。 (需要使用 Windows Server Essentials 从仪表板管理通讯组。 Windows Server Essentials 不支持此功能。)有关详细信息，请参阅[管理 Exchange Online 的电子邮件地址](#BKMK_SECTION_ManageEmailAddresses)和[管理 Exchange Online 的通讯组](#BKMK_SECTION_ManageDistributionGroups)。  
  
### <a name="manage-the-user-account-and-online-account-together"></a>同时管理用户帐户和联机帐户  
 你可以在帐户的整个生命周期中, 同时管理联机帐户和用户帐户。 如果停用用户帐户，则联机帐户也将在 Microsoft Online Services 中停用。 如果删除用户帐户，则联机帐户也将随之删除。 有关详细信息，请参阅[管理联机帐户](#BKMK_SECTION_ManageOnlineAccounts)。  
  
##  <a name="BKMK_SECTION_CreateOnlineAccounts"></a>创建联机帐户  
 将服务器与 Office 365 集成后, 可以从仪表板为用户创建 Microsoft Online Services 帐户。 在创建联机帐户时, 您将有很大的灵活性。 如果你有新的 Office 365 订阅, 你可以为所有用户批量创建联机帐户。 如果已在 Office 365 中创建了联机帐户, 请不要担心。 如果设置新服务器, 则可以通过导入联机帐户在服务器上创建用户帐户。 您可以在创建单个用户帐户时或将联机帐户添加到现有用户帐户时分配新的或现有的联机帐户。  
  
 **许可证要求**对于创建的每个联机帐户, 你将需要一个用户许可证。 检查仪表板上的 " **Office 365** " 页面, 查看通过 office 365 订阅提供的用户许可证数量。 如果需要添加更多的用户许可证, 你可以在 Office 365 中打开 Office 365 订阅来完成此操作。  
  
 **用户跟进**为用户帐户添加联机帐户后, 用户需要在下次登录时更改其用户帐户的密码。 新密码将立即与其联机帐户同步。 之后, 他们可以使用该密码通过其联机 ID 登录到 Office 365。  
  
> [!IMPORTANT]
>  向用户强调, 他们绝不应在 Office 365 中更改其联机帐户密码。 每当用户更改其用户帐户的密码时，该密码都会自动更改。 如果他们更改了 Office 365 中的联机密码, 则密码同步将会中断。  
  
 使用本部分中的过程以执行以下操作：  
  
-   [为现有用户帐户批量创建联机帐户](#BKMK_ToBulkCreateOnlineAccounts)  
  
-   [从 Microsoft Online Services 帐户导入用户帐户](#BKMK_ToImportUserAccounts)  
  
-   [创建一个新的用户帐户, 并为其分配联机帐户](#BKMK_ToCreateaNewUserAccount)  
  
-   [将联机帐户分配给用户帐户](#BKMK_ToAssignAnOnlineAccount)  
  
> [!NOTE]
>  如果你使用的是 Windows Server Essentials, 则在这些过程中, 你将看到*Office 365 帐户*, 而不是*Microsoft Online Services 帐户*。 此过程是相同的, 但在 Windows Server Essentials 中更改了术语。  
  
###  <a name="BKMK_ToBulkCreateOnlineAccounts"></a>为现有用户帐户批量创建联机帐户  
  
1.  以管理员身份登录服务器, 然后打开 Windows Server Essentials 仪表板。  
  
2.  在仪表板上，打开“用户”页。  
  
3.  在“用户任务”中，单击“添加 Microsoft 联机帐户”。  
  
     向导的“添加 Microsoft Online Services 帐户” 页将显示没有 Microsoft 联机帐户的所有用户帐户。 默认情况下，所有帐户都处于选中状态，并会为 Microsoft 联机 ID 推荐用户名。 如果已将自定义 Internet 域链接到 Office 365 订阅, 则默认情况下将使用该域。  
  
4.  在“添加 Microsoft Online Services 帐户” 页上，查看将创建的帐户。 例如，检查已具有带有不同联机 ID 的联机帐户的用户，确保已选择你要在电子邮件地址中使用的域。 完成任何所需更改后，请单击“下一步”。  
  
5.  在 "**分配 Microsoft Online Services 许可证**" 页上, 选择你的用户将使用的 Office 365 服务。 在单击“下一步”时，将开始创建帐户。  
  
    > [!NOTE]
    >  对于每个联机帐户, 你必须在 Office 365 中具有服务许可证。 你可以检查可用的许可证，并可打开你的订阅以从仪表板上的“Office 365” 页添加许可证（如有必要）。  
  
6.  通知用户现在他们已有一个 Microsoft 联机帐户。 用户必须先更改其网络用户帐户密码, 然后才能登录到 Office 365。 有关说明，请参阅[开始使用新的 Microsoft 联机帐户](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
###  <a name="BKMK_ToBeginUsingAnOnlineAccount"></a>开始使用新的 Microsoft 联机帐户  
  
1.  使用网络用户帐户登录计算机。  
  
2.  更改用户帐户的密码。 若要开始，请按 Ctrl+Alt+Delete，然后单击“更改密码”。  
  
     更改密码后，密码将与新的联机帐户同步。 你现在可以使用相同的密码登录到 Office 365。  
  
3.  使用你的新的联机 ID 和你的用户帐户密码[登录 Office 365](https://login.microsoftonline.com/login.srf?wa=wsignin1.0&rpsnv=3&ct=1398981834&rver=6.1.6206.0&wp=MBI_SSL&wreply=https:%2F%2Foutlook.office365.com%2Fowa%2F&id=260563&CBCXT=out) 。  
  
    > [!IMPORTANT]
    >  请勿在 Office 365 中更改联机帐户密码。 否则会中断密码同步。 每次更改网络用户帐户的密码时，联机密码都将随之更新。  
  
###  <a name="BKMK_ToImportUserAccounts"></a>从现有的联机帐户导入用户帐户  
  
1.  在仪表板上，打开“用户”页。  
  
2.  在“用户任务”中，单击“从 Office 365 导入帐户”。  
  
     下一页显示了在服务器上没有用户帐户的 Office 365 订阅的所有联机帐户。 所有的帐户默认都处于选中状态，建议将联机 ID 用作用户名。  
  
3.  创建用户帐户的步骤：  
  
    1.  对推荐的用户帐户作出所有必需的更改。  
  
    2.  还可以选择单击链接以查看将分配给用户帐户的临时密码。 你将需要为用户提供临时密码以及新的帐户名。  
  
         (创建帐户后, 可以在此文件中找到以下所列的密码:*SystemDrive*\Users\\*Office365admin*NewServerUser, 其中 Office365admin 是用于在服务器上管理 Office 365 的网络帐户, 而 NewServerUser 是新的\\用户帐户名称。)  
  
    3.  单击“下一步” ，创建用户帐户。  
  
4.  让用户知道自己的新用户帐户和他们将用于首次登录服务器的临时密码, 并且他们在登录后将需要更改密码。 有关面向用户的说明，请参阅 [开始使用新的 Microsoft 联机帐户](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
     请确保他们知道, 其联机帐户的密码将与其用户帐户同步, 他们不应更改 Office 365 中的联机密码。  
  
###  <a name="BKMK_ToCreateaNewUserAccount"></a>创建新的用户帐户并向其分配联机帐户  
  
1.  在仪表板上单击“用户”。  
  
2.  在“用户任务”中，单击“添加用户帐户”。 将出现“添加用户帐户”向导。  
  
3.  按照说明创建用户帐户。  
  
4.  在“分配 Microsoft Online Services 帐户”页上，为用户创建一个新的联机帐户或分配一个现有联机帐户：  
  
    -   若要创建新的联机帐户，请单击“创建新的 Microsoft Online Services 帐户并将其分配给此用户帐户”，并键入该 Microsoft Online Services 帐户的名称（默认情况下，将用户名用于联机 ID）。 然后，单击“下一步”。  
  
    -   若要分配现有的 Microsoft 联机帐户，请单击“将现有 Microsoft Online Services 帐户分配给此用户帐户”，并从下拉列表中选择一个现有帐户。 然后，单击“下一步”。  
  
    > [!NOTE]
    >  在 Windows Server Essentials 中, Microsoft Online Services 帐户在向导和仪表板标签中称为 Office 365 帐户。  
  
5.  按照说明完成向导。  
  
6.  请通知用户，他们必须先更改其用户帐户的密码，才可以使用新的联机帐户登录到 Office 365。 有关说明，请参阅[开始使用新的 Microsoft 联机帐户](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
#### <a name="BKMK_ToAssignAnOnlineAccount"></a>将联机帐户分配给用户帐户  
  
1.  在仪表板上单击“用户”。  
  
2.  右键单击列表中的用户帐户，然后单击“分配 Microsoft 联机帐户”。 将出现“分配 Microsoft Online Services 帐户”向导。  
  
3.  分配一个现有联机帐户或为用户创建一个新帐户。 新帐户的默认联机 ID 为用户名。 然后单击“下一步” ，将联机帐户添加到用户帐户。  
  
4.  查看向导最后一页上的信息，然后单击“关闭”。  
  
5.  请通知用户，他们必须先更改其用户帐户的密码，才可以使用新的联机帐户登录到 Office 365。 有关说明，请参阅[开始使用新的 Microsoft 联机帐户](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
##  <a name="BKMK_SECTION_ManageOnlineAccounts"></a>管理联机帐户  
 当你在 Windows Server Essentials 中将联机帐户添加到用户帐户时, 可以在帐户的整个生命周期内同时管理这两个帐户。  
  
###  <a name="BKMK_UnderstandingAccountStatus"></a>了解联机帐户状态  
 将 Microsoft Online Services 帐户分配给用户帐户后，该帐户的电子邮件地址将出现在仪表板的“用户” 页上的“Microsoft 联机帐户” 列中。 (在 Windows Server Essentials 中, 列标签为**Office 365 帐户**。)  
  
-   电子邮件地址旁边的蓝色图标表示该联机帐户处于活动状态。 即, 该帐户具有当前的 Office 365 许可证, 并且用户可以使用联机 ID 登录到 Office 365。  
  
-   电子邮件地址旁边的灰色图标表示该联机帐户处于非活动状态–, 因为许可证不再处于活动状态或未分配联机帐户。 当你取消分配用户的联机帐户时, 将删除该许可证, 并阻止用户使用该帐户登录到 Office 365。 但是, 服务器将保留用户帐户名和 Office 365 电子邮件地址之间的映射。  
  
###  <a name="BKMK_UnassignOnlineAccount"></a>限制对联机帐户的访问  
 如果用户离开你的组织, 或者你希望限制用户访问你的 Office 365 服务, 该怎么办？ 如果你在 Windows Server Essentials 中管理用户的联机帐户及其用户帐户, 则可以使用三个选项:  
  
-   **取消分配联机帐户**？如果希望在不阻止用户访问服务器上的资源的情况下防止用户使用 Office 365, 则应该取消分配联机帐户。 将发布 Office 365 许可证, 并阻止用户登录到 Office 365。 但是, 服务器将保留用户帐户名和 Office 365 电子邮件地址之间的映射。 有关说明, 请参阅[从用户帐户中取消分配联机帐户](#BKMK_ToUnassignAnOnlineAccount)。  
  
-   **停用用户帐户**？如果停用用户帐户, 因为某位员工暂时或永久性地离开, 则该用户的联机帐户也将停用。 虽然无法使用联机帐户，但用户数据（包括电子邮件）会保留在 Microsoft Online Services 中。 有关说明, 请参阅 "[管理用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md)" 中[的 "停用用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6)"。  
  
-   **删除用户帐户**？如果删除用户帐户, 还会从 Microsoft Online Services 中删除联机帐户。 有关说明, 请参阅 "[管理用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md)" 中[的 "删除用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove)"。  
  
    > [!WARNING]
    >  请注意，在删除联机帐户后，用户数据将遵循 Microsoft Online Services 的数据保留策略。 如果在员工离开后需要保留用户的用户数据, 请停用用户帐户, 而不是删除它。  
  
####  <a name="BKMK_ToUnassignAnOnlineAccount"></a>从用户帐户中取消分配联机帐户  
  
1.  在仪表板上单击“用户”。  
  
2.  右键单击列表中的用户帐户, 然后单击 "**取消分配 Microsoft 联机帐户**" (在 Windows Server Essentials 中, 单击 "**取消分配 Office 365 帐户**")。  
  
3.  在出现确认提示时，单击“是”。  
  
##  <a name="BKMK_SECTION_ManageEmailAddresses"></a>管理 Exchange Online 的电子邮件地址  
 通过在 Windows Server Essentials 中将电子邮件地址添加到用户的联机帐户, 你可以允许用户在 Exchange Online 中通过多个电子邮件地址接收电子邮件。  
  
###  <a name="BKMK_PROC_AddEmailAliases"></a>将其他电子邮件地址添加到用户的 Microsoft 联机帐户  
  
1.  在 Windows Server Essentials 仪表板上, 单击 "**用户**"。  
  
2.  右键单击列表中的用户帐户，然后单击“查看帐户属性”。  
  
3.  在帐户属性的 " **Microsoft 联机**" 选项卡 (或 Windows Server Essentials 中的 " **Office 365** " 选项卡) 上, 单击 "**添加**"。  
  
4.  键入新的电子邮件别名，然后选择电子邮件域。  
  
5.  单击“确定”两次 。  
  
##  <a name="BKMK_SECTION_ManageDistributionGroups"></a>管理 Exchange Online 的通讯组 (仅限 Windows Server Essentials)  
 将 Windows Server Essentials 服务器与 Office 365 集成后, 可以从 Windows Server Essentials 仪表板创建和管理 Exchange Online 的通讯组。 你将在添加到 "**用户**" 页的 "**通讯组**" 选项卡上执行此操作。 如果你有 Exchange Online 订阅，你将仅看到此选项卡。 此功能在 Windows Server Essentials 中不可用。  
  
 使用下面的过程，以：  
  
-   [添加通讯组](#BKMK_PROCEDURE_AddDistGroup)  
  
-   [更改通讯组的成员](#BKMK_ChangeGroupMembers)  
  
-   [更改用户的通讯组成员身份](#BKMK_EditUserMemberships)  
  
-   [删除通讯组](#BKMK_RemoveDistributionGroup)  
  
###  <a name="BKMK_PROCEDURE_AddDistGroup"></a>添加通讯组  
  
1.  在 Windows Server Essentials 中的仪表板上, 单击 "**用户**", 然后单击 "**通讯组**" 选项卡。  
  
2.  在“通讯组任务”中单击“添加通讯组”。  
  
     此时将显示“添加新的通讯组”向导。  
  
3.  在“添加新的通讯组”页上，输入以下信息，然后单击“下一步”：  
  
    -   输入新通讯组的组名、可选描述和电子邮件别名。  
  
    -   默认情况下，通讯组可以接收来自组织以外的人的电子邮件。 如果不想允许此操作，请清除该选项。  
  
4.  在“添加组成员”页上，使用“添加”按钮将活动的用户帐户（已为其分配联机帐户）和其他通讯组添加到新的通讯组。 然后，单击“下一步”。  
  
     已在 Exchange Online 中创建新的通讯组。  
  
###  <a name="BKMK_ChangeGroupMembers"></a>更改通讯组的成员  
  
1.  在仪表板上单击“用户”，然后单击“通讯组”选项卡。  
  
2.  右键单击列表中的通讯组，然后单击“更改组成员身份”。  
  
3.  使用“添加” 和“删除” 按钮从通讯组添加或删除活动的联机帐户。 然后单击“下一步”更新 Exchange Online 中的通讯组成员身份。  
  
###  <a name="BKMK_EditUserMemberships"></a>更改用户的通讯组成员身份  
  
1.  在仪表板上单击“用户”。  
  
2.  右键单击列表中的用户帐户，然后单击“查看帐户属性”。  
  
3.  在用户帐户属性中，单击“通讯组”选项卡，然后单击“编辑”。  
  
4.  在“编辑组成员身份” 框中，使用“添加” 和“删除” 按钮从用户帐户添加或删除通讯组，然后单击“关闭”。  
  
5.  单击“确定”以保存更新后的用户帐户属性。  
  
###  <a name="BKMK_RemoveDistributionGroup"></a>删除通讯组  
  
1.  在仪表板上单击“用户”，然后单击“通讯组”选项卡。  
  
2.  右键单击列表中的通讯组，然后单击“删除该组”。  
  
3.  在出现确认提示时，单击“删除组”。  
  
     通讯组将从 Exchange Online 中删除。  
  
## <a name="see-also"></a>请参阅  
  
-   [管理用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  
-   [管理 Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [管理 Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
