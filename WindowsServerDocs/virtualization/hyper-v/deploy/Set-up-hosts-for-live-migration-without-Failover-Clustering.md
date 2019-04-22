---
title: 设置为没有故障转移群集的实时迁移的主机
description: 提供有关如何设置实时迁移非群集环境中的说明
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5e3c405-cb76-4ff2-8042-c2284448c435
author: KBDAzure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: 49e36d7fae5eec07772fd6f82c5a5d69838351d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821988"
---
# <a name="set-up-hosts-for-live-migration-without-failover-clustering"></a>设置为没有故障转移群集的实时迁移的主机

>适用于：Windows Server 2016 中，Microsoft 的 HYPER-V Server 2016 中，Windows Server 2019，Microsoft HYPER-V Server 2019

本文介绍如何设置使你可以执行实时迁移它们之间不群集的主机。 如果你没有设置实时迁移，当安装了 HYPER-V，或如果你想要更改的设置，请使用这些说明。 若要设置群集的主机，使用故障转移群集工具。  
  
## <a name="requirements-for-setting-up-live-migration"></a>设置实时迁移的要求  
  
若要设置实时迁移非群集主机，您将需要：   
  
-  具有执行各个步骤的权限的用户帐户。 中的本地 HYPER-V 管理员组或源和目标计算机上的 Administrators 组的成员身份符合此要求，除非你配置约束的委派。 域管理员组的成员身份，才能配置约束的委派。  
  
- 在 Windows Server 2016 或 Windows Server 2012 R2 的源和目标服务器上安装 HYPER-V 角色。 您可以执行如果虚拟机是在至少运行 Windows Server 2016 和 Windows Server 2012 R2 的主机之间的实时迁移版本 5。 <br>版本升级的说明，请参阅[在 Windows 10 或 Windows Server 2016 上的 HYPER-V 中的升级虚拟机版本](Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。 有关安装说明，请参阅[Windows Server 上安装 HYPER-V 角色](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md)。  
  
- 属于同一 Active Directory 域，或者属于互相信任的域的源和目标计算机。    
- 除非源或目标服务器上安装这些工具安装在运行 Windows Server 2016 或 Windows 10 的计算机上的 HYPER-V 管理工具将从服务器中运行这些工具。  
  
## <a name="consider-options-for-authentication-and-networking"></a>请考虑进行身份验证和网络选项  
  
请考虑你想要安装以下软件：  
  
-  **身份验证**:将使用哪些协议进行身份验证源和目标服务器之间的实时迁移流量？ 选择将确定是否需要在启动实时迁移之前登录到源服务器：   
   - Kerberos 可让您无需登录到服务器，但需要约束的委派进行设置。 有关说明，请参阅下文。  
   - CredSSP 允许你避免配置约束的委派，但需要登录到源服务器。 可以执行此操作通过本地控制台会话，远程桌面会话或远程 Windows PowerShell 会话。  
  
      CredSPP 要求可能不太明显的情况下登录。 例如，如果在你登录 TestServer01，以将虚拟机移动到 TestServer02，然后想要将虚拟机移回 TestServer01，将需要登录到 TestServer02，然后再尝试将虚拟机移回 TestServer01。 如果不这样做，身份验证尝试失败，出现错误，并显示以下消息：  
    
      "在迁移源虚拟机迁移操作失败。  
      未能与主机建立连接*计算机名*:无凭据中提供了安全包 0x8009030E。"
  
-   **性能**:没有，最好配置性能选项？ 这些选项可以减少网络和 CPU 使用率，以及使实时迁移速度更快。 考虑您的要求和基础结构，并测试不同的配置，以帮助您决定。 在第 2 步末尾描述了这些选项。  
  
-  **网络首选项**:你是允许实时迁移流量通过任何可用网络，还是将其隔离到特定网络？ 我们建议将迁移流量隔离到受信任的专用网络上，这是最佳安全做法，因为实时迁移流在网络上发送时未进行加密设置。 可通过物理上隔离的网络或通过另一个受信任的网络技术（如 VLAN）来实现网络隔离。  
  
## <a name="BKMK_Step1"></a>步骤 1:配置约束的委派 （可选）  
如果您已决定使用 Kerberos 进行身份验证实时迁移流量，配置约束的委派使用该帐户是域管理员组的成员。  
  
### <a name="use-the-users-and-computers-snap-in-to-configure-constrained-delegation"></a>使用用户和计算机管理单元来配置约束的委派  
  
1.  打开“Active Directory 用户和计算机”管理单元。 (如果未选择，从服务器管理器中，选择服务器，单击**工具** >> **Active Directory 用户和计算机**)。  
  
2.  从导航窗格中**Active Directory 用户和计算机**，选择域，然后双击**计算机**文件夹。  
  
3.  从**计算机**文件夹中，右键单击源服务器的计算机帐户，然后单击**属性**。  
  
4.  从**属性**，单击**委派**选项卡。  
  
5.  在委派选项卡上选择**信任此计算机来委派到特定服务仅**，然后选择**使用任何身份验证协议**。  
  
6.  单击**添加**。  
  
7.  从**将服务添加**，单击**用户或计算机**。  
  
8.  从**选择用户或计算机**，键入目标服务器的名称。 单击**检查名称**以验证它，然后单击**确定**。  
  
9. 从**将服务添加**，在可用服务列表中，执行以下操作，然后单击**确定**:  
  
    -   要移动虚拟机存储器，请选择 **cifs**。 如果你想要迁移存储器与虚拟机，也因为如果你想要移动仅虚拟机的存储，这是必需的。 如果将该服务器配置为使用 Hyper-V 的 SMB 存储器，则应首先选中该选项。  
  
    -   要迁移虚拟机，选择 **“Microsoft 虚拟系统迁移服务”**。  
  
10. 在“属性”对话框的 **“委派”** 选项卡上，确定上一步选定的服务列在目标计算机可以为其提供委派证书的服务中。 单击 **“确定”**。  
  
11. 从 **“Computers”** 文件夹中选择目标服务器的计算机帐户，然后重复执行该程序。 在 **“选择用户或计算机”** 对话框，确保指定源服务器的名称。  
  
发生了以下两项后，配置更改生效：  
  
  -  所做的更改被复制到运行 HYPER-V 的服务器登录到域控制器。  
  -  域控制器颁发新的 Kerberos 票证。  
  
## <a name="BKMK_Step2"></a>步骤 2:设置用于实时迁移的源和目标计算机  
此步骤包括选择用于身份验证和网络的选项。 作为安全性最佳实践，我们建议你选择特定的网络用于实时迁移流量，如上文所述。 此步骤还演示了如何选择性能选项。   
  
### <a name="use-hyper-v-manager-to-set-up-the-source-and-destination-computers-for-live-migration"></a>使用 Hyper-v 管理器来设置实时迁移的源和目标计算机  
  
1.  打开 Hyper-V 管理器。 (从服务器管理器中，单击**工具** >>**Hyper-v 管理器**。)  
  
2.  在导航窗格中，选择一台服务器。 (如果未列出，请右键单击**Hyper-v 管理器**，单击**连接到服务器**，键入服务器名称，然后单击**确定**。 重复此步骤添加更多的服务器。）  
  
3.  在中**操作**窗格中，单击**HYPER-V 设置** >>**实时迁移**。  
  
4.  在 **“实时迁移”** 面板中，勾选 **“启用内向和外向实时迁移”**。  
  
5.  下**同时实时迁移**，指定不同的数字，如果不想要使用默认值为 2。  
  
6.  在 **“内向实时迁移”** 下面，如果打算使用特定网络连接来运行实时迁移流量，则单击 **“添加”** ，键入 IP 地址信息。 另外，单击 **“使用任何可用网络进行实时迁移”**。 单击 **“确定”**。  
  
7.  若要选择 Kerberos 和性能选项，展开**实时迁移**，然后选择**高级功能**。  
  
    - 如果已在配置约束的委派**身份验证协议**，选择**Kerberos**。  
    - 下**性能选项**，查看详细信息，然后选择一个不同的选项是否适合你的环境。   
  
8. 单击 **“确定”**。  
  
9. 在 Hyper-v 管理器中选择另一台服务器，重复步骤。  
  
### <a name="use-windows-powershell-to-set-up-the-source-and-destination-computers-for-live-migration"></a>使用 Windows PowerShell 来设置实时迁移的源和目标计算机  
  
非群集主机上配置实时迁移提供了三个 cmdlet:[Enable-vmmigration](https://technet.microsoft.com/library/hh848544.aspx)， [Set-vmmigrationnetwork](https://technet.microsoft.com/library/hh848467.aspx)，和[Set-vmhost](https://technet.microsoft.com/library/hh848524.aspx)。 此示例使用所有这三个，并执行以下任务：   
  - 本地主机上配置实时迁移  
  - 仅在特定网络上允许内向迁移流量  
  - 选择 Kerberos 作为身份验证协议   
  
每一行代表一个单独的命令。  
  
```  
PS C:\> Enable-VMMigration  
  
PS C:\> Set-VMMigrationNetwork 192.168.10.1  
  
PS C:\> Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos  
  
```  
Set-vmhost 还允许您选择的性能选项 （和许多其他主机设置）。 例如，若要选择 SMB 但保持设置为默认值为 CredSSP 身份验证协议，请键入：  
  
```  
PS C:\> Set-VMHost -VirtualMachineMigrationPerformanceOption SMBTransport  
  
```  
  
下表介绍的性能选项的工作原理。  
  
|Option|描述|  
|----------|---------------|  
    |TCP/IP|将虚拟机的内存复制到目标服务器中，通过 TCP/IP 连接。|  
    |压缩|通过 TCP/IP 连接将其复制到目标服务器之前压缩的虚拟机的内存内容。 **注意：** 这是**默认**设置。|  
    |SMB|通过 SMB 3.0 连接复制到目标服务器的虚拟机的内存。<br /><br />的当源和目标服务器上的网络适配器具有远程直接内存访问 (RDMA) 功能启用时，将使用 SMB 直通。<br />-SMB 多通道会自动检测并标识正确的 SMB 多通道配置时使用多个连接。<br /><br />有关详细信息，请参阅[通过 SMB 直通优化文件服务器的性能](https://technet.microsoft.com/library/jj134210(WS.11).aspx)。|  
      
 ## <a name="next-steps"></a>后续步骤
 
 设置主机后，你准备好执行实时迁移。 有关说明，请参阅[使用没有故障转移群集的实时迁移移动虚拟机](../manage/Use-live-migration-without-Failover-Clustering-to-move-a-virtual-machine.md)。 
    


