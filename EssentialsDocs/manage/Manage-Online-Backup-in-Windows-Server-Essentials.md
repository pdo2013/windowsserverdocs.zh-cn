---
title: 管理 Windows Server Essentials 中的联机备份
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95a9f593-fad7-4335-bd4d-c7bb8c033efb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 37d59d500a2de1e2b98c848e7484ae13639d09b7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890718"
---
# <a name="manage-online-backup-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的联机备份

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
  
 使用 Microsoft Azure Backup 集成后**联机备份**管理页面将出现在 Windows Server Essentials 仪表板。 **“联机备份”** 页面使执行常见的管理任务成为可能。 联机备份页上的功能包括：  
  
-   下面的子部分页面：  
  
    -   **“联机备份”** 注册服务器以进行联机备份后，此部分将显示当前的备份状态、存储状态和帐户信息。  
  
    -   **受保护的文件夹**本部分列出了所有共享的文件夹和在服务器上的所有文件历史记录文件夹以及您已选择在 Azure 中备份的任何其他文件夹。 列表包括每个共享文件夹的文件夹名称、文件夹路径以及状态。  
  
    -   **“备份历史记录”** 本节显示最近的联机备份的列表。 该列表包含操作类型，以及每个备份的时间和状态。  
  
-   具有有关选定的备份作业或还原作业的其他信息的详细信息窗格。  
  
-   包含一组可执行的管理任务的任务窗格。  
  
 作为最佳实践，应备份最重要的业务和用户数据。 例如，你应该备份包含重要数据文件的服务器文件夹。 你还应该备份包含重要信息的网络计算机的文件历史记录。  
  
 在决定要联机备份的数据时，请考虑你想要实现的备份频率和保留策略。 然后查看你的预算并确定可以承受的存储空间量。 根据你的需求平衡成本和存储量，然后配置联机备份以保护尽可能多的重要数据。 有关定价的信息，请参阅 [Azure 备份的定价详细信息](https://azure.microsoft.com/pricing/details/backup/)。  
  
 以下部分介绍可以在 Windows Server Essentials 仪表板中显示的各种联机备份任务。  
  
## <a name="online-backup-tasks-in-the-dashboard"></a>仪表板中的联机备份任务  
  
-   [将证书上传到 Azure 备份保管库](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [配置联机备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [开始联机备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [从联机备份还原文件和文件夹](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [注册此服务器以进行备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [取消注册服务器](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
###  <a name="BKMK_1"></a> 将证书上传到 Azure 备份保管库  
 对于 Windows Server Essentials 中的联机备份可以使用 Azure 备份之前，必须上载公共证书以注册到备份保管库。 使用证书进行身份验证并代表 Microsoft Online Services 订阅所有者来管理资源与订阅关联的 Azure 备份部署 （代理）。  
  
> [!NOTE]
>  在上载证书之前，必须完成 [Sign up for Azure Backup Service](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)中的过程。  
  
##### <a name="to-upload-a-certificate-to-use-with-the-azure-backup-service"></a>上载用于 Azure 备份服务的证书  
  
1.  使用管理员帐户登录到 Windows Server Essentials 仪表板。  
  
2.  在仪表板的 **“主页”** 页面上，单击 **“联机备份”**。  
  
3.  在 **“联机备份”** 区域中，单击 **“将证书上载到 Azure 备份保管库”**。  
  
     这将打开**恢复服务**Azure 管理门户中。 如果你计划运行 t 已登录到 Azure，你将需要使用你的 Microsoft 帐户登录。  
  
4.  单击你将用于联机备份的备份保管库的名称以打开该备份保管库的“快速启动”  页面。  
  
5.  单击 **“管理证书”**。  
  
6.  在 **“管理证书”** 对话框中，粘贴 Windows Server Essentials 生成的公用证书的路径。 若要获取的公用证书的路径，请执行以下操作：  
  
    1.  在 Windows Server Essentials 仪表板上，单击 **“联机备份”** 选项卡。  
  
    2.  在 **“联机备份”** 页面上，复制生成的证书的路径。  
  
    3.  切换到 Azure 管理门户，然后在**管理证书**对话框框中，粘贴该路径，将生成的公用证书上传。  
  
    > [!NOTE]
    >  你还可以使用自己的公共证书。 若要了解需要哪些证书，请在 **“快速启动”** 页面上，单击 **“获取证书”** 链接。  
  
    > [!NOTE]
    >   Azure 需要带有公钥的 x.509 v2 证书。 有关详细信息，请参阅 [管理保管库证书](https://msdn.microsoft.com/library/azure/dn169036.aspx)。  
  
7.  选择证书后，请单击 **“确定”** （复选标记）。  
  
8.  你将返回到 **“快速启动”** 页。 单击 **“仪表板”**，并验证该证书已成功上载。 成功上载证书后，仪表板显示证书指纹和过期日期。  
  
 完成此过程后，请执行以下操作：  
  
1.  将服务器注册 Azure 备份服务。 有关详细信息，请参阅[注册此服务器用于备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)。  
  
2.  配置服务器的联机备份。 有关详细信息，请参阅 [Configure online backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
###  <a name="BKMK_2"></a> 配置联机备份  
 向 Azure 备份注册服务器后，你可以在 Windows Server Essentials 中配置联机备份设置。  
  
##### <a name="to-configure-online-backup"></a>配置联机备份  
  
1.  从注册你的服务器向导或从 Windows Server Essentials 仪表板的 **“联机备份”** 页面，单击 **“配置联机备份”**。 配置联机备份向导将显示。  
  
2.  上**配置联机备份**页的向导中，选择你想要备份到 Azure 备份每个服务器文件夹的复选框。 清除不希望包括在备份中的每个服务器文件夹的复选框。 若要添加未显示在列表中的文件夹，请单击 **“添加文件夹”**。 当你完成选择时，请单击 **“下一步”**。  
  
    > [!NOTE]
    >  不能选择不在本地服务器上的文件夹，或格式化为 ReFS 的驱动器上的文件夹。  
  
3.  在向导的 **“添加文件历史记录备份”** 页面上，你可以选择包含用于支持文件历史记录功能的网络计算机的文件历史记录备份。 然后单击 **“下一步”** 以继续。  
  
4.  在向导的 **“指定备份计划”** 页面上，配置以下各项，然后单击 **“下一步”** 以继续：  
  
    -   选择或接受一天中你希望运行联机备份的时间。 默认时间是 10:00 PM。 你还可能指定运行第二个备份的时间。  
  
    -   选择或接受你希望运行联机备份的日期。 默认情况下，联机备份从星期一运行至星期五。  
  
5.  在向导的 **“指定备份保留策略”** 页面上，选择你想要保留联机备份的天数，然后单击 **“下一步”**。 默认值为 7 天。 还可以选择保留联机备份 15 或 30 天。  
  
    > [!NOTE]
    >   Azure 备份始终保留你最近的备份。 如果备份目标没有足够可用空间来存储备份，则备份过程将不会成功。 若要避免这种情况，请购买额外的存储空间或缩短数据保留期。 有关定价信息，请参阅[定价详细信息](https://azure.microsoft.com/pricing/details/backup/)Microsoft Azure 备份。  
  
6.  在向导的 **“选择带宽使用量”** 页面上，如果你想要限制分配到联机备份中的 Internet 带宽量，则选择 **“启用 Internet 带宽的使用”**。 使用页面上的选项来指定允许联机备份在工作时间和非工作时间内使用的 Internet 带宽量。 然后，定义你的工作时间和工作日。  
  
    > [!NOTE]
    >   Azure 备份允许你自定义集成软件利用网络带宽进行备份或还原信息时的方式。 使用方法通常称为限制，可以控制是可供备份使用的网络带宽量，并在特定日期和时间间隔期间还原过程。 在配置联机备份向导中选择 **“启用 Internet 带宽的使用”** 复选框后，你可以指定应用工作时间带宽限制的工作日和工作时间。 在所有其他时间使用非工作时间限制。 这两个限制的有效带宽范围都是从 256 Kbps 到无限。  
  
7.  联机备份配置完成后，单击 **“关闭”**。  
  
    > [!TIP]
    >  成功地配置备份后， **“联机备份”** 页显示最近的联机备份的状态和由备份保管库使用的存储空间量。 若要查看上一个备份的状态，请单击 **“备份历史记录”**。  
  
###  <a name="BKMK_3"></a> 开始联机备份  
  
> [!NOTE]
>  开始联机备份之前，你必须首先[注册此服务器用于备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)，然后[配置联机备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
##### <a name="to-start-an-online-backup-immediately"></a>立即开始联机备份  
  
1.  以管理员身份登录到仪表板。  
  
2.  在导航栏上，单击 **“联机备份”**。  
  
3.  在 **“联机备份任务”** 窗格中，单击 **“立即开始备份”**。  
  
###  <a name="BKMK_4"></a> 从联机备份还原文件和文件夹  
 还原文件和文件夹向导指导你完成查找、选择和还原已联机备份的文件和文件夹。 当你继续执行向导时，将执行以下任务：  
  
1.  **选择从中还原文件和文件夹的联机备份**  
  
     当你运行还原文件和文件夹向导时，必须执行的第一个操作是指定你是要从你当前登录到的服务器的联机备份还原文件，还是要从另一台服务器的备份还原文件。  
  
2.  **选择从中还原文件和文件夹的服务器**  
  
     如果你选择从另一台服务器还原文件和文件夹，则在可用的服务器列表中选择你想要还原的服务器，然后单击 **“下一步”**。  
  
3.  **选择包含的文件和文件夹还原的卷**  
  
     联机备份包含对应于备份源服务器上的卷的卷。 选择该卷后，选择从中还原备份的日期和时间，然后单击 **“下一步”**。  
  
4.  **选择要还原文件和文件夹**  
  
     在向导的 **“选择要还原的项目”** 页面上，可以打开不同的文件夹位置并选择每个要还原的文件或文件夹的复选框。 当你完成选择文件的操作时，请单击 **“下一步”**。  
  
5.  **指定用于还原的文件和文件夹的目标位置**  
  
     向导的 **“指定恢复选项”** 页面使你可以指定还原文件和文件夹的位置。 有两个选项：  
  
    -   **还原到原始位置**。 选择此选项以将文件和文件夹还原到发起备份的确切位置。  
  
    -   **还原到其他位置**。  选择此选项以在服务器上指定还原文件的其他位置。  
  
         在默认安装中，如果具有与还原文件相同名称的文件已在目标位置上存在，则向导将创建原始文件的副本。 此外，更新访问控制列表 (ACL) 以反映当前的文件权限。 可以单击 **“高级”** 自定义默认设置。  
  
6.  **确认还原信息**  
  
     **“确认还原信息”** 页提供了你已指定的还原说明的摘要。 若要继续进行文件还原，必须为你的联机备份帐户键入正确的密码。  
  
###  <a name="BKMK_5"></a> 注册此服务器以进行备份  
 若要备份或还原到 Azure 备份的文件、 文件夹和 Windows Server Essentials 服务器上的文件历史记录，必须首先向 Microsoft Azure 备份服务注册服务器。  
  
> [!NOTE]
>  注册你的服务器之前，必须上载证书以用于将存储联机备份的备份保管库。 有关详细信息，请参阅[将证书上载到 Azure 备份保管库中](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)。  
  
##### <a name="to-register-your-server-with-azure-backup"></a>向 Azure Backup 注册服务器  
  
1.  以管理员身份登录到服务器，然后打开仪表板。  
  
2.  在导航栏上，单击 **“联机备份”**。  
  
3.  在 **“联机备份”** 子节中，单击 **“注册”**。  
  
4.  为你的联机备份选择要用于备份保管库的证书。 默认情况下，选择默认证书；如果要选择其他证书，则使用 **“浏览”**。 然后，单击“下一步”。  
  
5.  按照该向导中的说明创建密码，然后完成注册。  
  
###  <a name="BKMK_6"></a> 取消注册服务器  
  
> [!CAUTION]
>  如果取消注册你的 Windows Server Essentials 服务器从 Microsoft Azure 备份服务，Azure 备份不能再可以备份服务器。 此外，将删除以前已上载的服务器数据。 若要继续联机备份，必须再次注册服务器。  
  
##### <a name="to-unregister-your-server-with-azure-backup"></a>从 Azure Backup 注销服务器  
  
1.  登录到 [Azure 管理门户](https://manage.windowsazure.com)。  
  
2.  单击 **“恢复服务”**。  
  
3.  在 **“恢复服务”** 中，单击备份保管库的名称。  
  
4.  从该保管库的 **“快速启动”** 页面上，单击 **“服务器”**。  
  
5.  选择服务器，单击 **“删除”**，然后在确认提示中单击 **“是”** 。  
  
## <a name="related-tasks"></a>相关的任务  
  
-   [更改联机备份策略](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [查看联机备份的存储使用情况](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [在联机备份中包括新的文件夹](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [删除或从联机备份策略中排除文件历史记录备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [禁用或重新启用联机服务器备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [停止正在进行中的联机服务器备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [查看联机备份状态](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [查看和管理联机备份警报](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [联机备份重置为默认设置](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [注册 Azure 备份服务](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [将 Azure 备份与 Windows Server Essentials 集成](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [保护 Windows Server Essentials 中的联机备份的文件夹](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_18)  
  
-   [Windows Server Essentials 中的联机备份历史记录](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_19)  
  
###  <a name="BKMK_7"></a> 更改联机备份策略  
 通过使用 Windows Server Essentials 仪表板对联机备份策略进行更改很容易。  
  
##### <a name="to-change-the-online-backup-policy"></a>更改联机备份策略  
  
1.  以管理员身份登录到仪表板。  
  
2.  在导航栏上，单击 **“联机备份”**。  
  
3.  在 **“联机备份任务”** 窗格中，单击 **“立即开始备份”**。  
  
4.  按照向导中的说明自定义联机备份策略。  
  
 有关你可以自定义的设置的详细信息，请参阅 [Configure online backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
###  <a name="BKMK_8"></a> 查看联机备份的存储使用情况  
  
##### <a name="to-view-the-amount-of-storage-space-that-online-backup-uses"></a>查看联机备份使用的存储空间量  
  
1.  以管理员身份登录到仪表板。  
  
2.  在导航栏上，单击 **“联机备份”**。  
  
3.  在 **“联机备份”** 选项卡上的信息窗格中显示 **“存储状态”** 。  
  
###  <a name="BKMK_9"></a> 在联机备份中包括新的文件夹  
  
##### <a name="to-include-a-new-folder-in-the-online-backup-policy"></a>在联机备份策略中包括新的文件夹  
  
1.  以管理员身份登录到仪表板。  
  
2.  在导航栏上，单击 **“联机备份”**。  
  
3.  在 **“联机备份任务”** 窗格中，单击 **“立即开始备份”**。  
  
4.  在 **“更改或删除备份策略”** 页面上，单击 **“自定义联机备份策略”**。  
  
5.  在 **“配置联机备份”** 页面上，如果要包括的文件夹未出现在列表中，则单击 **“添加文件夹”**。  
  
6.  在 **“添加文件夹”** 窗口中，浏览到并选择要包括在联机备份中的文件夹，然后单击 **“确定”**。  
  
7.  在 **“配置联机备份”** 页面上，根据需要选择要包括的其他文件夹，然后单击 **“下一步”**。  
  
8.  按照向导中的说明完成自定义联机备份策略。  
  
 有关可自定义的其他设置的详细信息，请参阅 [Configure online backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
###  <a name="BKMK_10"></a> 删除或从联机备份策略中排除文件历史记录备份  
  
##### <a name="to-remove-or-exclude-a-folder-from-the-online-backup-policy"></a>从联机备份策略中删除或排除某个文件夹  
  
1.  以管理员身份登录到仪表板。  
  
2.  在导航栏上，单击 **“联机备份”**。  
  
3.  单击 **“受保护的文件夹”** 选项卡。在详细信息窗格中，显示文件夹列表及其联机备份状态。  
  
4.  选择要从联机备份策略中排除的文件夹，然后在任务窗格中，单击 **“从联机备份中删除文件夹”**。  
  
###  <a name="BKMK_11"></a> 禁用或重新启用联机服务器备份  
 有关如何使用 Azure 备份来备份或还原服务器数据的说明，请参阅[注册此服务器用于备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)。  
  
 有关如何停止使用 Azure 备份来备份或还原服务器数据的说明，请参阅[注销服务器](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6)。  
  
###  <a name="BKMK_12"></a> 停止正在进行中的联机服务器备份  
  
##### <a name="to-stop-an-online-server-backup-in-progress"></a>停止正在进行的联机服务器备份  
  
1.  以管理员身份登录到仪表板。  
  
2.  在导航栏上，单击 **“联机备份”**。  
  
3.  在 **“联机备份任务”** 窗格中，单击 **“立即开始备份”**。  
  
     停止备份后，为 **“备份历史记录”** 列表中的备份显示 **“已取消”** 状态。  
  
###  <a name="BKMK_13"></a> 查看联机备份状态  
  
##### <a name="to-view-the-backup-status"></a>查看备份状态  
  
1.  以管理员身份登录到仪表板。  
  
2.  在导航栏上，单击 **“联机备份”**。  
  
3.  单击 **“备份历史记录”** 选项卡。列表视图显示每个备份作业的状态。 选择备份作业以查看有关该作业的详细信息。  
  
###  <a name="BKMK_14"></a> 查看和管理联机备份警报  
 像许多其他的警报，警报查看器中显示的 Azure 备份的警报。  
  
##### <a name="to-view-online-backup-alerts-in-the-alert-viewer"></a>在警报查看器中查看联机备份警报  
  
1.  打开“仪表板”。  
  
2.  执行下列操作之一：  
  
      Windows Server Essentials:在导航窗格中，单击警报图标\(可能是严重、 警告还是信息\)。 这将打开警报查看器。  
  
      Windows Server Essentials:在 **“主页”** 页面上，单击 **“运行状况监视”** 选项卡。  
  
3.  查看与 Azure 备份相关的问题的警报的列表。  
  
 有关使用警报查看器或运行状况监视选项卡管理警报的详细信息，请参阅[管理系统运行状况](Manage-System-Health-in-Windows-Server-Essentials.md)。  
  
###  <a name="BKMK_15"></a> 联机备份重置为默认设置  
 Windows Server Essentials 提供了帮助你配置联机备份的设置的向导。 如果要还原默认设置，则运行 **“配置联机备份”** 任务，并选择 **“删除联机备份策略”** 选项。 然后，再次运行 **“配置联机备份”** 任务。 你之前上载的数据保持不变。  
  
###  <a name="BKMK_16"></a> 注册 Azure 备份服务  
 若要准备将与 Windows Server Essentials 集成 Microsoft Azure 备份，将使用你的 Microsoft Online Services 帐户，登录到 Azure 管理门户，然后创建备份保管库在 Azure 中存储联机备份。 然后，将下载 Azure 备份集成模块，并使用下载的文件以在 Windows Server Essentials 服务器上安装 Azure 备份外接程序。 如果你没有 Microsoft 帐户，则可以注册免费试用版。  
  
 若要执行此安装，请完成以下任务：  
  
1.  注册 Microsoft Online Services 帐户和备份预览。  
  
2.  创建备份保管库以存储联机备份。  
  
3.  下载 Azure 备份代理。  
  
4.  在服务器上安装 Azure 备份外接程序。  
  
####  <a name="BKMK_SignupforaMicrosoftOnlineServiceAccount"></a> 注册 Microsoft Online Services 帐户和备份预览  
  
1.  登录到 Windows Server Essentials 仪表板。  
  
2.  在仪表板 **“主页”** 页面上，依次单击 **“外接程序”** 类别、**“与 Azure 备份集成”** 和 **“单击以注册 Azure 备份”**。  
  
3.  在 Azure 上**恢复服务**页上，在**备份 （预览）** 部分中，查看详细信息。  
  
4.  如果没有 Azure 订阅，请单击**免费试用版**，然后按照说明操作以获取 Azure 订阅。  
  
     如果已有 Azure 订阅，请单击**门户**在 web 页后，可以转到 Azure 管理门户的右上角。  
  
5.  在 Azure 管理门户页上，你将看到**恢复服务**的左窗格中。 S 您将在其中管理备份保管库的存储联机备份从 Windows Server Essentials。  
  
####  <a name="BKMK_Createabackupvaulttostoreonlinebackups"></a> 创建备份保管库以存储联机备份  
  
1.  从 Windows Server Essentials 服务器上的 Web 浏览器登录到 [Azure 管理门户](https://manage.windowsazure.com)。  
  
2.  在 Azure 管理门户中，单击**新建**，单击**Data Services**，单击**恢复服务**，单击**备份保管库**，，然后单击**快速创建**。  
  
3.  输入你的备份保管库的名称，选择你希望将备份存储到的区域，然后单击 **“快速创建”**。  
  
     **“恢复服务”** 区域打开时会显示你的新备份保管库。  
  
####  <a name="BKMK_DownloadtheWindowsAzureBackupAgent"></a> 下载 Azure 备份代理  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在仪表板 **“主页”** 页面上，依次单击 **“开始”** 选项卡、 **“外接程序”** 类别、 **“与 Azure 备份集成”** 和 **“单击以下载 Azure 备份集成模块”**。  
  
####  <a name="BKMK_InstalltheWindowsAzureBackupAddIn"></a> 在服务器上安装 Azure 备份外接程序  
  
1.  使用管理员帐户登录到服务器，然后运行你在上一步中下载的 **“OnlineBackupAddin.wssx”** 文件。  
  
2.  显示 **“软件许可条款”** 。 如果你同意条款和条件，则单击 **“接受”** 以继续安装。  
  
3.  在 **“安装外接程序”** 页面上，单击 **“安装外接程序”**。  
  
    > [!NOTE]
    >  你可能需要重新启动服务器，以安装任何必备软件。  
  
     显示 **“安装”** 页。 进度指示器指示安装何时开始并显示安装的进度。 安装完成后，您将收到一条消息，Azure 备份外接程序已成功安装。  
  
4.  单击 **“完成”**。  
  
5.  关闭并重新打开仪表板。  
  
     将新的选项卡 **“联机备份”** 添加到仪表板。 在此选项卡，可以注册你的服务器、 配置备份设置，并打开**恢复服务**Azure 管理门户来管理你的服务器的备份保管库中。  
  
 完成这些步骤后，请执行以下操作：  
  
1.  在 Windows Server Essentials 中，上载证书以用于联机备份。 有关详细信息，请参阅[将证书上载到 Azure 备份保管库中](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)。  
  
2.  Azure 备份保管库注册服务器。 有关详细信息，请参阅[注册此服务器用于备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)。  
  
3.  配置服务器的联机备份。 有关详细信息，请参阅 [Configure online backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
###  <a name="BKMK_17"></a> 将 Azure 备份与 Windows Server Essentials 集成  
 Microsoft Azure 备份集成模块是可用于加密和备份文件和文件夹从服务器到由 Microsoft 提供的 Azure 托管存储系统的 Windows Server Essentials 的新功能。 通过使用 Azure 备份进行加密和服务器上备份数据，可以帮助防止由于火灾、 水灾、 盗窃或其他灾难的关键业务数据的灾难性损失。 使用 Azure 备份来备份服务器数据，当信息进行加密使用你的密码，然后将其上载到 Internet 上的安全数据中心。 若要从联机备份访问数据，你必须有一台已由证书进行身份验证的服务器，而且必须提供密码。  
  
 集成并向 Azure 备份注册服务器之后, 可以配置联机备份设置以执行定期计划的备份。 你还可以随时通过单击联机备份仪表板中的 **“立即启动备份”** 任务启动联机备份。  
  
 联机备份设置涉及以下步骤：  
  
1.  [注册 Azure 备份服务](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)-在你注册备份服务后，将创建备份保管库、 下载和安装 Azure 备份集成模块。  
  
2.  [将证书上传到 Azure 备份保管库](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
3.  [注册此服务器以进行备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
4.  [配置联机备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
> [!NOTE]
>   Azure 备份使用通行短语来加密文件和文件夹以进行联机备份。 更改加密密码将替换你注册服务器时指定的密码。 密码仅接受 ASCII 编码的字符。  
  
###  <a name="BKMK_18"></a> 保护 Windows Server Essentials 中的联机备份的文件夹  
 仪表板的联机备份部分的 **“受保护文件夹”** 子部分中显示服务器上所有共享文件夹的列表。 下表描述列表中包含的信息。  
  
|列|描述|  
|------------|-----------------|  
|**文件夹名称：**|联机备份中包含的文件夹的名称。<br /><br /> 若要添加或排除文件夹，请运行 **“配置联机备份”** 任务。|  
|**文件夹路径：**|文件夹位置。|  
|**状态：**|有三种类型的状态**受保护**，**不受保护**，并**未知**。|  
  
###  <a name="BKMK_19"></a> Windows Server Essentials 中的联机备份历史记录  
 仪表板的联机备份部分中的 **“备份历史记录”** 子部分显示最近的联机备份的列表。 你可以使用成功的备份还原文件和文件夹。 下表描述列表中包含的信息。  
  
|列|描述|  
|------------|-----------------|  
|**操作：**|有两种类型的操作 - **“备份”** 和 **“还原”**。|  
|**时间：**|这是最新状态的记录时间。|  
|**状态：**|有五种类型的状态**成功**，**正在进行**，**已取消**，**警告**，和**未成功**.|  
  
## <a name="see-also"></a>请参阅  
  
-   [管理备份和还原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Manage Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
