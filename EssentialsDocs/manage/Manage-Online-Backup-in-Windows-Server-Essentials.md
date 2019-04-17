---
title: "管理 Windows Server Essentials 中的 Online Backup"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="manage-online-backup-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的 Online Backup

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
  
 使用 Microsoft Azure 备份，集成后**Online Backup**管理页面，将显示在 Windows Server Essentials 仪表板中。 **Online Backup**页使执行常见的管理任务。 Online Backup 页面上的功能包括：  
  
-   下的子节页面中：  
  
    -   **Online Backup**注册的服务器联机备份后，此部分中显示当前备份状态、存储状态和帐户信息。  
  
    -   **受保护的文件夹**此部分中列出的所有共享的文件夹服务器，在所有文件夹中的文件历史记录和已经在 Azure 上备份的任何其他文件夹。 列表中包括文件夹名称、文件夹路径和共享的每个文件夹的状态。  
  
    -   **备份历史记录**此部分中显示的最近在线备份列表。 该列表将包括操作的时间和每个备份状态的类型。  
  
-   有关的所选备份作业或还原工作的其他信息的详细信息窗格。  
  
-   包含一组的管理任务，你可以执行的任务窗格。  
  
 作为最佳做法，你应备份最重要的企业和用户数据。 例如，你应备份 server 文件夹，其中包含重要数据文件。 你还应该备份包含的重要信息的网络计算机文件历史记录。  
  
 在决定要备份联机哪些数据时，请考虑备份你想要实现的频率和保留策略。 然后查看你 budget，并确定可以为你提供的存储空间量。 平衡成本和针对你的需求存储卷，然后配置在线备份，以尽可能多的重要数据尽可能保护。 了解定价信息，请参阅[定价详细信息，用于 Azure 备份](https://azure.microsoft.com/pricing/details/backup/)。  
  
 以下部分介绍了各种可能会出现在 Windows Server Essentials 仪表板中联机备份任务。  
  
## <a name="online-backup-tasks-in-the-dashboard"></a>仪表板中联机备份任务  
  
-   [上传到 Azure 备份圆顶证书](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [配置在线备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [开始联机的备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [从在线备份中还原的文件和文件夹](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [注册备份此服务器](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [注销 server](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
###  <a name="BKMK_1"></a>上传到 Azure 备份圆顶证书  
 对于 Windows Server Essentials 中的在线备份可以使用 Azure 备份之前，必须上载备份圆顶使用注册的公用证书。 证书用于验证 Azure 备份部署（代理）代表 Microsoft Online 服务订阅所有者若要管理与订阅相关的资源。  
  
> [!NOTE]
>  您上传证书之前，你必须完成中的步骤[注册 Azure 备份服务](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)。  
  
##### <a name="to-upload-a-certificate-to-use-with-the-azure-backup-service"></a>若要上传证书，才能使用 Azure 备份服务  
  
1.  登录到 Windows Server Essentials 使用管理员帐户仪表板上。  
  
2.  仪表板上**家庭**页上，单击**ONLINE BACKUP**。  
  
3.  在**ONLINE BACKUP**区域中，单击**到 Azure 备份圆顶上传证书**。  
  
     这会打开**恢复服务**Azure 管理门户中。 如果你并未 t 已登录到 Azure，你将需要使用你的 Microsoft 帐户登录。  
  
4.  单击你将使用的在线备份打开备份圆顶名称**快速启动**备份圆顶的页面。  
  
5.  单击**管理证书**。  
  
6.  在**管理证书**由 Windows Server Essentials 对话框中，粘贴生成的公用证书的路径。 若要获取公共证书的路径，请执行以下操作：  
  
    1.  在 Windows Server Essentials 仪表板中，单击**Online Backup**选项卡。  
  
    2.  在**Online Backup**页上，所生成的证书的路径复制。  
  
    3.  切换到 Azure 管理门户、，然后在**管理证书**对话框中，将其粘贴要上传生成的公用证书的路径。  
  
    > [!NOTE]
    >  你还可以使用你自己公共证书。 了解哪些证书，才能，在**快速启动**页上，单击**获取证书**链接。  
  
    > [!NOTE]
    >   Azure 带有公钥需要 x.509 v2 证书。 有关详细信息，请参阅[管理圆顶证书](https://msdn.microsoft.com/library/azure/dn169036.aspx)。  
  
7.  选择证书后，单击**确定**（复选标记）。  
  
8.  你将返回到**快速启动**页面。 单击**仪表板**，并验证的证书已成功上载。 证书上载成功后，该仪表板显示证书指纹和到期日期。  
  
 完成该过程后，请执行以下操作：  
  
1.  Azure 备份服务中注册的服务器。 有关详细信息，请参阅[注册备份此服务器](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)。  
  
2.  将配置服务器的在线备份。 有关详细信息，请参阅[配置联机备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
###  <a name="BKMK_2"></a>配置在线备份  
 使用 Azure 备份注册服务器后，你可以在 Windows Server Essentials 配置在线的备份设置。  
  
##### <a name="to-configure-online-backup"></a>若要配置联机备份  
  
1.  注册你的服务器向导中，或者从**在线备份**页面的 Windows Server Essentials 仪表板中，单击**配置联机备份**。 显示配置在线备份向导。  
  
2.  在**配置 Online Backup**向导页，选择你想要备份 Azure 备份到每个 server 文件夹的复选框。 清除你不希望在备份中包含的每个 server 文件夹的复选框。 若要添加未显示在列表中的文件夹，请单击**添加文件夹**。 完成选择后，单击**下一步**。  
  
    > [!NOTE]
    >  你无法选择乐在本地服务器上的文件夹或在驱动器上 ReFS 格式的文件夹。  
  
3.  在**添加的文件历史记录备份**页面向导中，您可以选择包含的网络支持的文件历史记录功能的计算机文件历史记录备份。 然后单击**下一步**以继续。  
  
4.  在**指定备份计划**页面向导中，将配置以下，然后单击**下一步**继续：  
  
    -   选择或接受当你想要运行的在线备份的一天的时间。 默认情况下是 10:00。 你也可以指定一次运行第二个备份。  
  
    -   选择或接受天的后，当你想要运行的在线备份。 默认情况下，联机备份运行周一到周五。  
  
5.  在**指定备份保留策略**页面向导中，选择你想要保留的联机备份，然后单击天数**下一步**。 默认值为 7 天。 你还可以选择保留在线备份 15 或 30 天。  
  
    > [!NOTE]
    >   Azure 备份始终保持最新备份。 如果备份目标不具有足够可用空间来存储的备份，将会失败备份的过程。 若要避免此情况，购买额外的存储空间或缩短数据保留时段。 了解定价信息，请参阅[定价的详细信息](https://azure.microsoft.com/pricing/details/backup/)Microsoft Azure 备份。  
  
6.  在**选择带宽使用量**向导中，如果你想要限制的分配给联机选择备份的 Internet 带宽量页面**启用 Internet 带宽使用量**。 页面上的选项可用于指定多少的 Internet 带宽，允许你使用工作时间内和非工作的时间内的在线备份。 然后，你的企业时间和个工作日内定义。  
  
    > [!NOTE]
    >   Azure 备份允许你自定义如何集成软件利用的网络带宽备份或还原信息时。 使用以下方法通常称为限制，你可以控制的适用于使用的备份中的网络带宽量，并在特定的日期和时间间隔恢复过程。 选择后**启用 Internet 带宽使用量**配置 Online Backup 向导中的复选框，你可以指定个工作日和工作期间其应用工作小时带宽限制的时间。 有时是根本使用非工作时间限制。 有效的带宽范围是从 256 Kbps 到无限制的这两个限制。  
  
7.  当在线备份配置完毕时，单击**关闭**。  
  
    > [!TIP]
    >  成功备份配置中之后,**在线备份**页面显示的最新的在线备份和备份圆顶使用存储空间量的状态。 若要查看之前的备份的状态，请单击**备份历史记录**。  
  
###  <a name="BKMK_3"></a>开始联机的备份  
  
> [!NOTE]
>  你可以开始联机备份之前，你必须先[注册备份此服务器](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)，，然后[配置联机备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
##### <a name="to-start-an-online-backup-immediately"></a>若要立即开始联机备份  
  
1.  以管理员身份登录到仪表板上。  
  
2.  在导航栏中，单击**ONLINE BACKUP**。  
  
3.  在**联机备份任务**窗格中，单击**从现在开始备份**。  
  
###  <a name="BKMK_4"></a>从在线备份中还原的文件和文件夹  
 还原文件和文件夹向导通过定位、选择，并还原的文件和文件夹在线备份的进程指导您。 前进向导时，你将执行以下任务：  
  
1.  **选择要还原的文件和文件夹从中联机备份**  
  
     还原文件和文件夹向导将运行时，你必须执行操作的第一个改进就是指定你希望从你当前登录到，服务器联机备份或不同服务器的备份还原文件。  
  
2.  **选择要还原的文件和文件夹从中服务器**  
  
     如果你选择还原的文件和文件夹，从另一台服务器，选择你想要还原从列表中可用的服务器，然后单击的服务器**下一步**。  
  
3.  **选择包含的文件和文件夹还原音量**  
  
     在线备份包含卷对应于卷备份源服务器上。 选择音量后，选择的日期和时间的备份进行还原，然后单击从中**下一步**。  
  
4.  **选择要还原文件和文件夹**  
  
     在**选择还原项目**页面向导中，你可以打开的各个文件夹位置，然后选择每个文件或你想要还原的文件夹的复选框。 当你完成后选择文件，请单击**下一步**。  
  
5.  **指定还原的文件和文件夹的目标位置**  
  
     **指定恢复选项**向导中的页面，可指定还原的文件和文件夹的位置。 有两个选项：  
  
    -   **将还原到原始位置**。 选择此选项可到精确的位置发起备份中还原文件和文件夹。  
  
    -   **还原到其他位置**。  选择此选项可指定来还原文件所在的服务器上的其他位置。  
  
         在默认安装中，如果还原文件同名文件已存在目标位置向导创建原始文件的副本。 此外，使用控制列表 (ACL) 会更新以反映当前文件权限。 你可以单击**高级**自定义默认设置。  
  
6.  **确认还原信息**  
  
     **确认还原信息**页面提供你所指定还原说明的摘要。 若要继续进行还原文件，必须在线备份帐户键入正确的密码。  
  
###  <a name="BKMK_5"></a>注册备份此服务器  
 若要备份或给 Azure 备份中还原文件、文件夹和 Windows Server Essentials 服务器上的文件历史记录，必须首先使用 Microsoft Azure 备份服务注册服务器。  
  
> [!NOTE]
>  注册服务器之前，你必须上载将存储在线备份备份圆顶上使用的证书。 有关详细信息，请参阅[上载到 Azure 备份圆顶证书](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)。  
  
##### <a name="to-register-your-server-with-azure-backup"></a>若要使用 Azure 备份注册服务器  
  
1.  登录到该服务器以 administrator 身份，并打开对仪表板。  
  
2.  在导航栏中，单击**ONLINE BACKUP**。  
  
3.  在**Online Backup**子节中，单击**注册**。  
  
4.  选择你想要使用的在线备份备份圆顶证书。 默认证书选择默认设置。如果你想要选择不同的证书，使用**浏览**。 然后单击**下一步**。  
  
5.  按照创建一个密码，然后完成注册向导中的说明进行操作。  
  
###  <a name="BKMK_6"></a>注销 server  
  
> [!CAUTION]
>  注销 Windows Server Essentials 服务器从 Microsoft Azure 备份服务时，如果 Azure 备份不会再可以备份服务器。 此外，之前已上传的服务器数据将被删除。 若要继续在线备份，必须注册再次服务器。  
  
##### <a name="to-unregister-your-server-with-azure-backup"></a>若要注销使用 Azure 备份服务器  
  
1.  登录到[Azure 管理门户](https://manage.windowsazure.com)。  
  
2.  单击**恢复服务**。  
  
3.  在**恢复服务**，单击备份圆顶的名称。  
  
4.  从**快速启动**页面的圆顶上，单击**服务器**。  
  
5.  选择的服务器上，单击**删除**，然后单击**是**在确认提示。  
  
## <a name="related-tasks"></a>相关的任务  
  
-   [更改备份联机策略](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [查看备份的在线存储使用情况](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [在联机的备份中包含一个新文件夹](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [删除或从在线备份策略排除文件历史记录备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [禁用或重新启用联机服务器备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [停止正在联机服务器备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [查看在线备份状态](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [查看和管理在线备份通知](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [重置在线备份到默认设置](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [注册 Azure 备份服务](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [与 Windows Server Essentials 集成 Azure 备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [保护联机备份 Windows Server Essentials 中的文件夹](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_18)  
  
-   [Windows Server Essentials 中的在线备份历史记录](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_19)  
  
###  <a name="BKMK_7"></a>更改备份联机策略  
 很容易地使用 Windows Server Essentials 仪表板更改在线备份的策略。  
  
##### <a name="to-change-the-online-backup-policy"></a>若要更改的备份的联机策略  
  
1.  以管理员身份登录到仪表板上。  
  
2.  在导航栏中，单击**Online Backup**。  
  
3.  在**联机备份任务**窗格中，单击**配置联机备份**。  
  
4.  按照该向导中的说明自定义联机备份的策略。  
  
 有关详细信息，你可以自定义设置，请参阅[配置联机备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
###  <a name="BKMK_8"></a>查看备份的在线存储使用情况  
  
##### <a name="to-view-the-amount-of-storage-space-that-online-backup-uses"></a>若要查看 Online Backup 使用的存储空间量  
  
1.  以管理员身份登录到仪表板上。  
  
2.  在导航栏中，单击**ONLINE BACKUP**。  
  
3.  在**Online Backup**选项卡上，**存储状态**信息窗格中会出现。  
  
###  <a name="BKMK_9"></a>在联机的备份中包含一个新文件夹  
  
##### <a name="to-include-a-new-folder-in-the-online-backup-policy"></a>联机策略备份中包含一个新文件夹  
  
1.  以管理员身份登录到仪表板上。  
  
2.  在导航栏中，单击**Online Backup**。  
  
3.  在**联机备份任务**窗格中，单击**配置联机备份**。  
  
4.  在**更改或删除备份策略**页上，单击**自定义联机备份策略**。  
  
5.  在**配置 Online Backup**页上，如果你想要包括文件夹没有出现在列表中，单击**添加文件夹**。  
  
6.  在**添加文件夹**窗口中，浏览到，然后选择你想要将包含在在线备份，然后单击的文件夹**确定**。  
  
7.  在**配置 Online Backup**页上，选择其他文件夹，以包含根据需要，然后单击**下一步**。  
  
8.  按照向导中的说明来完成自定义联机备份的策略。  
  
 有关详细信息，你可以自定义的其他设置，请参阅[配置联机备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
###  <a name="BKMK_10"></a>删除或从在线备份策略排除文件历史记录备份  
  
##### <a name="to-remove-or-exclude-a-folder-from-the-online-backup-policy"></a>若要删除或从在线备份策略排除文件夹  
  
1.  以管理员身份登录到仪表板上。  
  
2.  在导航栏中，单击**Online Backup**。  
  
3.  单击**受保护的文件夹**选项卡。详细信息窗格中，将显示与他们的在线备份状态文件夹的列表。  
  
4.  选择你想要排除联机备份策略，然后在任务窗格中，单击该文件夹**从备份中联机中删除该文件夹**。  
  
###  <a name="BKMK_11"></a>禁用或重新启用联机服务器备份  
 有关如何使用 Azure 备份来备份，或者还原 server 的数据的说明，请参阅[注册备份此服务器](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)。  
  
 有关如何停止使用 Azure 备份来备份，或者还原 server 的数据的说明，请参阅[注销服务器](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6)。  
  
###  <a name="BKMK_12"></a>停止正在联机服务器备份  
  
##### <a name="to-stop-an-online-server-backup-in-progress"></a>若要阻止正在联机服务器备份  
  
1.  以管理员身份登录到仪表板上。  
  
2.  在导航栏中，单击**ONLINE BACKUP**。  
  
3.  在**联机备份任务**窗格中，单击**停止备份**。  
  
     停止备份，状态为后**已取消**的备份中显示**备份历史记录**列表。  
  
###  <a name="BKMK_13"></a>查看在线备份状态  
  
##### <a name="to-view-the-backup-status"></a>若要查看备份状态  
  
1.  以管理员身份登录到仪表板上。  
  
2.  在导航栏中，单击**ONLINE BACKUP**。  
  
3.  单击**备份历史记录**选项卡。列表视图显示有关每个备份作业状态。 选择备份工作并查看该作业有关其他详细信息。  
  
###  <a name="BKMK_14"></a>查看和管理在线备份通知  
 其他许多通知，如备份 Azure 的通知警报查看器中显示。  
  
##### <a name="to-view-online-backup-alerts-in-the-alert-viewer"></a>若要查看在线备份警报通知查看器中  
  
1.  打开的面板。  
  
2.  请执行以下任一操作：  
  
      Windows Server 软件包：在导航窗格中，单击提醒图标 \（可能是关键、警告或 Informational\）。 这将打开警报查看器。  
  
      Windows Server 软件包：上**家庭**页上，单击**健康监视**选项卡。  
  
3.  查看列表中 Azure 备份到相关的问题的警报。  
  
 有关使用警报查看或健康监视选项卡，若要管理通知的详细信息，请参阅[管理系统运行状况](Manage-System-Health-in-Windows-Server-Essentials.md)。  
  
###  <a name="BKMK_15"></a>重置在线备份到默认设置  
 Windows Server 软件包提供，可帮助你为联机备份配置设置向导。 如果你想要还原默认设置，运行**配置联机备份**任务，并选择**删除在线备份策略**选项。 然后，运行**配置联机备份**再次任务。 你以前上载的数据将保持不变。  
  
###  <a name="BKMK_16"></a>注册 Azure 备份服务  
 准备与 Windows Server Essentials 集成 Microsoft Azure 备份，您将使用你的 Microsoft Online 服务的帐户，登录到 Azure 管理门户，，然后创建备份圆顶存储在 Azure 在线备份。 然后，将下载 Azure 备份集成的模块，，并使用下载的文件的备份 Azure Add-In 安装 Windows Server Essentials 服务器上。 如果你没有 Microsoft 帐户，您可以注册进行免费试用。  
  
 若要执行此设置，请完成以下任务：  
  
1.  注册的 Microsoft Online 服务帐户和备份预览。  
  
2.  创建备份的圆顶存储在线备份。  
  
3.  下载 Azure 备份的代理。  
  
4.  在服务器上安装的 Azure 备份 Add-In。  
  
####  <a name="BKMK_SignupforaMicrosoftOnlineServiceAccount"></a>注册 Microsoft Online 服务帐户和备份预览  
  
1.  登录到 Windows Server Essentials 仪表板。  
  
2.  仪表板上**家庭版**页上，单击**外接程序**类别中，单击**使用 Azure 备份集成**，然后单击**单击以注册 Azure 备份**。  
  
3.  在 Azure 上**恢复服务**页上，在**备份（预览）**部分中，检查的详细信息。  
  
4.  如果你没有 Azure 的订阅，请单击**免费试用版**，然后按照说明来获取 Azure 的订阅。  
  
     如果你已经拥有 Azure 的订阅，请单击**门户**右上角的网页以转到 Azure 管理门户。  
  
5.  在 Azure 管理门户页上，你将看到**恢复服务**左侧窗格中。 你将在其中管理备份 s 仓库 Windows Server Essentials 从该应用商店在线备份。  
  
####  <a name="BKMK_Createabackupvaulttostoreonlinebackups"></a>创建备份的圆顶存储在线备份  
  
1.  登录到[Azure 管理门户](https://manage.windowsazure.com)从你的 Windows Server Essentials 服务器上的 web 浏览器。  
  
2.  在 Azure 管理门户、单击**新建**，单击**数据服务**，单击**恢复服务**，单击**备份圆顶**，然后单击**快速创建**。  
  
3.  为你备份的圆顶输入一个名称，请选择的区域所需用来存储你的备份，然后单击**快速创建**。  
  
     **恢复服务**打开同时显示你新备份圆顶的区域。  
  
####  <a name="BKMK_DownloadtheWindowsAzureBackupAgent"></a>下载 Azure 备份代理  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  仪表板上**家庭**页上，单击**入门**选项卡上，单击**外接程序**类别中，单击**使用 Azure 备份集成**，，然后单击**单击以下载 Azure 备份集成模块**。  
  
####  <a name="BKMK_InstalltheWindowsAzureBackupAddIn"></a>在服务器上的 Azure 备份 Add-In 安装  
  
1.  登录到使用管理员帐户，你服务器，然后运行**OnlineBackupAddin.wssx**在上一步中下载的文件。  
  
2.  **软件许可条款**显示。 如果你同意的条款和条件，请单击**接受**继续安装。  
  
3.  在**外接程序安装**页上，单击**外接程序安装**。  
  
    > [!NOTE]
    >  你可能需要重新启动才能安装任何先决条件软件的服务器。  
  
     **安装**页面的显示方式。 进度指示器显示时安装开始，将显示安装的进度。 安装完成后，你将收到一条消息，Azure 备份 Add-in 已成功安装。  
  
4.  单击**完成**。  
  
5.  关闭并重新打开仪表板。  
  
     一个新选项卡，**Online Backup**，添加到仪表板。 此实验中，从注册服务器、配置备份设置，并打开**恢复服务**Azure 管理门户管理存储有关你的服务器备份库中。  
  
 完成这些步骤后，请执行以下操作：  
  
1.  在 Windows Server Essentials 上传的证书，以用于在线备份。 有关详细信息，请参阅[上载到 Azure 备份圆顶证书](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)。  
  
2.  Azure 备份圆顶注册服务器。 有关详细信息，请参阅[注册备份此服务器](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)。  
  
3.  将配置服务器的在线备份。 有关详细信息，请参阅[配置联机备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
###  <a name="BKMK_17"></a>与 Windows Server Essentials 集成 Azure 备份  
 Microsoft Azure 备份集成模块是一项新的 Windows Server Essentials 功能，使您能够加密并备份文件和文件夹从服务器与提供的 Microsoft Azure 托管存储系统。 通过使用 Azure 备份加密并备份在服务器上的数据，您可以帮助防止严重胡子由于灰烬、洪水、被盗或其他灾难关键业务数据。 当你使用 Azure 备份来备份 server 的数据时的信息将加密之前将其上载到安全数据中心 Internet 上使用你的密码。 若要访问数据在线备份，你必须具有进行身份验证证书的服务器，必须提供此密码。  
  
 集成，注册 Azure 备份服务器后，您可以配置在线备份设置，以执行计划的定期备份。 你还可以启动联机备份随时方法是依次单击**从现在开始备份**在线备份仪表板中的任务。  
  
 联机的备份设置涉及这些步骤：  
  
1.  [Azure 备份服务注册](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)-后您登录以备份服务中，您将创建备份的圆顶、下载和安装 Azure 备份集成模块。  
  
2.  [上传到 Azure 备份圆顶证书](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
3.  [注册备份此服务器](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
4.  [配置在线备份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
> [!NOTE]
>   Azure 备份使用密码进行加密文件和联机备份的文件夹。 更改加密密码将替换注册服务器时指定的密码。 密码接受仅 ASCII 编码字符。  
  
###  <a name="BKMK_18"></a>保护联机备份 Windows Server Essentials 中的文件夹  
 **受保护的文件夹**仪表板中的 Online Backup 部分中的子节显示服务器上的所有共享文件夹的列表。 下表介绍了在列表中包含的信息。  
  
|列|描述|  
|------------|-----------------|  
|**文件夹名称：**|联机的备份中包含的文件夹的名称。<br /><br /> 若要添加或排除文件夹，请运行**配置联机备份**任务。|  
|**文件夹路径：**|文件夹的位置。|  
|**状态：**|有三种类型的状态**保护**，**不受保护**，并**未知**。|  
  
###  <a name="BKMK_19"></a>Windows Server Essentials 中的在线备份历史记录  
 **备份历史记录**仪表板中的 Online Backup 部分中的子部分显示的最近在线备份的列表。 你可以使用成功备份还原文件和文件夹。 下表介绍了在列表中包含的信息。  
  
|列|描述|  
|------------|-----------------|  
|**操作：**|有两种类型的运营-**备份**和**还原**。|  
|**时间：**|这是记录的最新状态的时间。|  
|**状态：**|有五种类型的状态**成功**，**正在进行**，**已取消**，**警告**，并**成功**。|  
  
## <a name="see-also"></a>请参阅  
  
-   [管理备份和还原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [管理 Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
