---
title: 正在运行或配置的虚拟机的数量必须在支持的限制范围内
description: 提供有关如何解决此最佳做法分析器规则报告的问题的说明。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9d3c4aa3-8416-46ec-a253-26dc98088d7b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 56d7fd528d7fda20dbdbb16a6262bb072f053ef0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364626"
---
# <a name="the-number-of-running-or-configured-virtual-machines-must-be-within-supported-limits"></a>正在运行或配置的虚拟机的数量必须在支持的限制范围内

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|Error  
|**类别**|配置|  
  
在以下部分中，斜体表示在此问题的最佳做法分析器工具中出现的文本。  
  
## <a name="issue"></a>问题  
*多个虚拟机正在运行或配置，而不受支持。*  
  
## <a name="impact"></a>影响  
*Microsoft 不支持在此服务器上运行或配置的虚拟机的当前数目。*  
  
## <a name="resolution"></a>分辨率  
*将一个或多个虚拟机移动到另一个服务器。*  
  
有关 Hyper-v 支持的最大配置（例如正在运行的虚拟机的数目）的详细信息，请参阅[规划 Windows Server 2016 中的 hyper-v 可伸缩性](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)。  
  
若要将虚拟机移动到另一台服务器，可以执行以下操作：  
  
- 从当前服务器导出虚拟机，然后将其导入到新服务器中，如下所述。   
- 执行实时迁移：   
    - 如果此服务器属于故障转移群集，请使用故障转移群集功能中提供的工具。 有关说明，请参阅[从节点到节点的实时迁移、快速迁移或移动虚拟机](https://go.microsoft.com/fwlink/?LinkID=181519)。  
    - 如果这是独立服务器，请参阅[配置实时迁移和迁移虚拟机（无故障转移群集](https://technet.microsoft.com//library/jj134199(v=ws.11).aspx)）中的说明  
  
### <a name="to-export-a-virtual-machine"></a>导出虚拟机  
  
   > [!IMPORTANT]  
   > 如果导出的 Hyper-v 主机属于某个域，并且你想要将导出的文件存储在远程位置，则必须为 Hyper-v 主机配置约束委派。 远程位置可以是共享的网络文件夹，也可以是要导入到的主机上的文件夹。 约束委派使 Hyper-v 主机的计算机帐户可以向远程计算机提供通用 Internet 文件系统（CIFS）服务的委派凭据。 有关配置约束委派的说明，请参阅下面的导出和导入说明部分。  
  
1.  打开 Hyper-V 管理器。 单击 **“开始”** ，指向 **“管理工具”** ，然后单击 **“Hyper-V 管理器”** 。  
  
2.  在结果窗格中的 "**虚拟机**" 下，右键单击虚拟机，然后单击 "**导出**"。  
  
3.  在 "**导出虚拟机**" 对话框中，键入或浏览到具有足够可用空间来存储所有虚拟机资源的位置。 导出虚拟机时，所有虚拟硬盘（.vhd 文件或 .vhdx 文件）、检查点（.avhd 文件）和与虚拟机关联的已保存状态文件都将复制到指定的文件夹。  
  
4.  单击“导出”。  
  
导出虚拟机后，将虚拟机导入到另一个服务器。  
  
### <a name="to-import-a-virtual-machine-to-another-server"></a>将虚拟机导入到另一个服务器  
  
1.  连接到运行 Hyper-v 的服务器，并打开 Hyper-v 管理器。  
  
2.  在 "**操作**" 窗格中，单击 "**导入虚拟机**"。  
  
3.  在 "**导入虚拟机**" 对话框中，指定要将虚拟机导出到的位置。 除非你要重新导入此虚拟机，否则请保留导入设置。  
  
4.  单击“导入”。  
  
### <a name="to-configure-constrained-delegation"></a>配置约束委派的方法  
  
需要**域管理员**组中的成员身份才能完成此过程。  
  
1.  在安装了 Active Directory 域服务工具功能的计算机上，在 "**管理工具**" 中**Active Directory 打开 "用户和计算机**"，然后导航到运行 hyper-v 的计算机的计算机帐户。  
  
    > [!NOTE]  
    > 如果 **“Active Directory 用户和计算机”** 没有列出，则请安装 Active Directory 域服务工具功能。 有关说明，请参阅[安装 AD DS 的远程服务器管理工具](https://go.microsoft.com/fwlink/?LinkId=140463)（ https://go.microsoft.com/fwlink/?LinkId=140463) 。  
  
2.  右键单击运行 Hyper-v 的计算机的计算机帐户，然后单击 "**属性**"。  
  
3.  在“委派”选项卡上，单击“仅信任此计算机来委派指定的服务”，然后单击“使用任何身份验证协议”。  
  
4.  若要允许 Hyper-v 计算机帐户提供到远程计算机的委派凭据，请执行以下操作：  
  
    1.  单击**添加**。  
  
    2.  在 "**添加服务**" 对话框中，单击 "**用户或计算机**"，选择远程计算机，然后单击 **"确定"** 。  
  
    3.  在 "**可用服务**" 列表中，选择 " **cifs** " 协议（也称为服务器消息块（SMB）协议），然后单击 "**添加**"。  
  
  
  


