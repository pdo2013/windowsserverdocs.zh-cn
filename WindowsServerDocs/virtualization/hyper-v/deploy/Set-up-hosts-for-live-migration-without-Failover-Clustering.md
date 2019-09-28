---
title: 为无故障转移群集的实时迁移设置主机
description: 提供有关在非群集环境中设置实时迁移的说明
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5e3c405-cb76-4ff2-8042-c2284448c435
author: KBDAzure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: ad5c3da632df3afc4c7b22c4e1c79b3b92ea7508
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364289"
---
# <a name="set-up-hosts-for-live-migration-without-failover-clustering"></a>为无故障转移群集的实时迁移设置主机

>适用于：Windows Server 2016，Microsoft Hyper-V Server 2016，Windows Server 2019，Microsoft Hyper-V 服务器2019

本文介绍如何设置未群集的主机，以便可以在它们之间进行实时迁移。 如果你在安装 Hyper-v 时未设置实时迁移，或者如果你想要更改设置，请使用这些说明。 若要设置群集主机，请使用故障转移群集工具。  
  
## <a name="requirements-for-setting-up-live-migration"></a>设置实时迁移的要求  
  
若要为实时迁移设置非群集主机，需要：   
  
-  有权执行各个步骤的用户帐户。 在源计算机和目标计算机上，本地 Hyper-v Administrators 组中的成员身份或 Administrators 组的成员资格满足此要求，除非你要配置约束委派。 若要配置约束委派，需要域管理员组中的成员身份。  
  
- 在源服务器和目标服务器上安装的 Windows Server 2016 或 Windows Server 2012 R2 中的 Hyper-v 角色。 如果虚拟机至少为版本5，则可以在运行 Windows Server 2016 和 Windows Server 2012 R2 的主机之间执行实时迁移。 <br>有关版本升级说明，请参阅在[windows 10 或 Windows Server 2016 上的 hyper-v 中升级虚拟机版本](Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。 有关安装说明，请参阅[在 Windows Server 上安装 hyper-v 角色](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md)。  
  
- 属于同一 Active Directory 域或属于彼此信任的域的源计算机和目标计算机。    
- Hyper-v 管理工具安装在运行 Windows Server 2016 或 Windows 10 的计算机上，除非在源服务器或目标服务器上安装了这些工具，并且你将从服务器运行这些工具。  
  
## <a name="consider-options-for-authentication-and-networking"></a>考虑用于身份验证和网络的选项  
  
考虑如何设置以下各项：  
  
-  **身份验证**：将使用哪种协议对源服务器和目标服务器之间的实时迁移流量进行身份验证？ 选择确定在开始实时迁移之前是否需要登录到源服务器：   
   - Kerberos 使你可以避免登录到服务器，但需要设置约束委派。 有关说明，请参阅下文。  
   - CredSSP 使你可以避免配置约束委派，但需要登录到源服务器。 可以通过本地控制台会话、远程桌面会话或远程 Windows PowerShell 会话执行此操作。  
  
      CredSPP 要求在可能不明显的情况下登录。 例如，如果你登录到 TestServer01 将虚拟机移动到 TestServer02，然后想要将虚拟机移回 TestServer01，则在尝试将虚拟机移回 TestServer01 之前，你需要登录到 TestServer02。 如果不这样做，身份验证尝试将失败，出现错误，并显示以下消息：  
    
      "迁移源的虚拟机迁移操作失败。  
      未能建立与主机*计算机名称*的连接：Security package 0x8009030E 中没有可用的凭据。
  
-   **性能**：配置性能选项是否有意义？ 这些选项可以减少网络和 CPU 使用率，并使实时迁移速度更快。 考虑你的要求和基础结构，并测试不同的配置，以帮助你做出决定。 步骤2的末尾介绍了这些选项。  
  
-  **网络首选项**：你是允许实时迁移流量通过任何可用网络，还是将其隔离到特定网络？ 我们建议将迁移流量隔离到受信任的专用网络上，这是最佳安全做法，因为实时迁移流在网络上发送时未进行加密设置。 可通过物理上隔离的网络或通过另一个受信任的网络技术（如 VLAN）来实现网络隔离。  
  
## <a name="BKMK_Step1"></a>步骤1：配置约束委派（可选）  
如果已决定使用 Kerberos 对实时迁移流量进行身份验证，请使用域管理员组成员的帐户配置约束委派。  
  
### <a name="use-the-users-and-computers-snap-in-to-configure-constrained-delegation"></a>使用 "用户和计算机" 管理单元配置约束委派  
  
1.  打开“Active Directory 用户和计算机”管理单元。 （在服务器管理器中，如果未选择服务器，请选择该服务器，单击 "**工具**"  >>  "**Active Directory 用户和计算机**）"。  
  
2.  从**Active Directory 用户和计算机**"中的导航窗格中，选择域，然后双击"**计算机**"文件夹。  
  
3.  在 "**计算机**" 文件夹中，右键单击源服务器的计算机帐户，然后单击 "**属性**"。  
  
4.  在 "**属性**" 中，单击 "**委派**" 选项卡。  
  
5.  在 "委派" 选项卡上，选择 "**仅信任此计算机来委派指定的服务"** ，然后选择 "**使用任何身份验证协议**"。  
  
6.  单击**添加**。  
  
7.  从 "**添加服务**" 中，单击 "**用户或计算机**"。  
  
8.  从 "**选择用户或计算机**" 中，键入目标服务器的名称。 单击 "**检查名称**" 以验证该名称，然后单击 **"确定"** 。  
  
9. 从 "**添加服务**" 的 "可用服务" 列表中，执行以下操作，然后单击 **"确定"** ：  
  
    -   要移动虚拟机存储器，请选择 **cifs**。 如果要将存储与虚拟机一起移动，以及仅移动虚拟机的存储，则需要执行此过程。 如果将该服务器配置为使用 Hyper-V 的 SMB 存储器，则应首先选中该选项。  
  
    -   要迁移虚拟机，选择 **“Microsoft 虚拟系统迁移服务”** 。  
  
10. 在“属性”对话框的 **“委派”** 选项卡上，确定上一步选定的服务列在目标计算机可以为其提供委派证书的服务中。 单击 **“确定”** 。  
  
11. 从 **“Computers”** 文件夹中选择目标服务器的计算机帐户，然后重复执行该程序。 在 **“选择用户或计算机”** 对话框，确保指定源服务器的名称。  
  
配置更改将在以下两种情况下生效：  
  
  -  这些更改将复制到运行 Hyper-v 的服务器所登录的域控制器。  
  -  域控制器发出新的 Kerberos 票证。  
  
## <a name="BKMK_Step2"></a>步骤2：设置用于实时迁移的源计算机和目标计算机  
此步骤包括选择用于身份验证和网络的选项。 作为安全方面的最佳做法，我们建议你选择用于实时迁移流量的特定网络，如上文所述。 此步骤还说明了如何选择 "性能" 选项。   
  
### <a name="use-hyper-v-manager-to-set-up-the-source-and-destination-computers-for-live-migration"></a>使用 Hyper-v 管理器设置用于实时迁移的源计算机和目标计算机  
  
1.  打开 Hyper-V 管理器。 （从服务器管理器中，单击 "**工具**"  >> "**hyper-v 管理器**"。）  
  
2.  在导航窗格中，选择其中一个服务器。 （如果未列出此项，请右键单击 " **Hyper-v 管理器**"，单击 "**连接到服务器**"，键入服务器名称，然后单击 **"确定"** 。 重复此步骤以添加更多服务器。）  
  
3.  在 "**操作**" 窗格中，单击 " **hyper-v 设置**" @no__t "**实时迁移**"。  
  
4.  在 **“实时迁移”** 面板中，勾选 **“启用内向和外向实时迁移”** 。  
  
5.  如果不想使用默认值2，请在 "**同时实时迁移**" 下指定其他数字。  
  
6.  在 **“内向实时迁移”** 下面，如果打算使用特定网络连接来运行实时迁移流量，则单击 **“添加”** ，键入 IP 地址信息。 另外，单击 **“使用任何可用网络进行实时迁移”** 。 单击 **“确定”** 。  
  
7.  若要选择 Kerberos 和性能选项，请展开 "**实时迁移**"，然后选择 "**高级功能**"。  
  
    - 如果已配置约束委派，请在 "**身份验证协议**" 下选择 " **Kerberos**"。  
    - 在 "**性能选项**" 下，查看详细信息并选择其他选项（如果适用于你的环境）。   
  
8. 单击 **“确定”** 。  
  
9. 选择 "Hyper-v 管理器" 中的其他服务器，然后重复上述步骤。  
  
### <a name="use-windows-powershell-to-set-up-the-source-and-destination-computers-for-live-migration"></a>使用 Windows PowerShell 设置用于实时迁移的源计算机和目标计算机  
  
有三个 cmdlet 可用于配置非群集主机上的实时迁移：[Enable-enable-vmmigration](https://technet.microsoft.com/library/hh848544.aspx)、 [set-vmmigrationnetwork](https://technet.microsoft.com/library/hh848467.aspx)和[VMHost](https://technet.microsoft.com/library/hh848524.aspx)。 此示例使用全部三个，并执行以下操作：   
  - 在本地主机上配置实时迁移  
  - 仅允许在特定网络上传入迁移流量  
  - 选择 Kerberos 作为身份验证协议   
  
每一行代表一个单独的命令。  
  
```  
PS C:\> Enable-VMMigration  
  
PS C:\> Set-VMMigrationNetwork 192.168.10.1  
  
PS C:\> Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos  
  
```  
VMHost 还允许您选择性能选项（以及许多其他主机设置）。 例如，若要选择 SMB 但将身份验证协议设置为默认值 CredSSP，请键入：  
  
```  
PS C:\> Set-VMHost -VirtualMachineMigrationPerformanceOption SMBTransport  
  
```  
  
下表介绍了性能选项的工作方式。  
  
|选项|描述|  
|----------|---------------|  
    |TCP/IP|通过 TCP/IP 连接将虚拟机的内存复制到目标服务器。|  
    |压缩|在通过 TCP/IP 连接将虚拟机复制到目标服务器之前压缩该虚拟机的内存内容。 **注意：** 这是**默认**设置。|  
    |SMB|通过 SMB 3.0 连接将虚拟机的内存复制到目标服务器。<br /><br />-当源服务器和目标服务器上的网络适配器启用了远程直接内存访问（RDMA）功能时，将使用 SMB 直通。<br />-在识别到正确的 SMB 多通道配置后，SMB 多通道将自动检测并使用多个连接。<br /><br />有关详细信息，请参阅[通过 SMB 直通优化文件服务器的性能](https://technet.microsoft.com/library/jj134210(WS.11).aspx)。|  
      
 ## <a name="next-steps"></a>后续步骤
 
 设置主机后，便可以执行实时迁移。 有关说明，请参阅[使用没有故障转移群集的实时迁移来移动虚拟机](../manage/Use-live-migration-without-Failover-Clustering-to-move-a-virtual-machine.md)。 
    


