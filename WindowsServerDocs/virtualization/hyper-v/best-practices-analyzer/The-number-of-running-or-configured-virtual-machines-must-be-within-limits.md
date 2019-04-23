---
title: 运行数或配置的虚拟机必须在支持的限制
description: 提供说明来解决此最佳实践分析程序规则所报告的问题。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9d3c4aa3-8416-46ec-a253-26dc98088d7b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8a971a48b2d8199a6c279f1bd3f1715039fa6e0d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855348"
---
# <a name="the-number-of-running-or-configured-virtual-machines-must-be-within-supported-limits"></a>运行数或配置的虚拟机必须在支持的限制

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|错误  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的文本。  
  
## <a name="issue"></a>问题  
*更多虚拟机正在运行或不是支持的配置。*  
  
## <a name="impact"></a>影响  
*Microsoft 不支持当前的运行或在此服务器上配置的虚拟机数。*  
  
## <a name="resolution"></a>分辨率  
*将一个或多个虚拟机移动到另一台服务器。*  
  
有关支持的最大配置 HYPER-V，如正在运行的虚拟机的数量的详细信息请参阅[规划 Windows Server 2016 中的 HYPER-V 可伸缩性](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)。  
  
若要将虚拟机移动到另一台服务器，您可以：  
  
- 从当前服务器中导出虚拟机，然后其导入到新的服务器如下所述。   
- 执行实时迁移操作：   
    - 如果此服务器属于故障转移群集，使用提供的故障转移群集功能的工具。 有关说明，请参阅[实时迁移、 快速迁移或移动虚拟机从节点到节点](https://go.microsoft.com/fwlink/?LinkID=181519)。  
    - 如果这是一个独立服务器，请参阅中的说明[配置实时迁移和没有故障转移群集迁移虚拟机](https://technet.microsoft.com//library/jj134199(v=ws.11).aspx)  
  
### <a name="to-export-a-virtual-machine"></a>若要导出虚拟机  
  
   > [!IMPORTANT]  
   > 如果要从导出的 HYPER-V 主机属于某个域，并且你想要存储在远程位置上的已导出的文件，必须进行约束委派配置的 HYPER-V 主机。 远程位置可能是共享的网络文件夹或要导入到主机上的文件夹。 约束的委派可让远程计算机的通用 Internet 文件系统 (CIFS) 服务提供委派的凭据的 HYPER-V 主机的计算机帐户。 有关配置约束的委派的说明，请参阅部分在导出和导入下面的说明。  
  
1.  打开 Hyper-V 管理器。 单击 **“开始”**，指向 **“管理工具”**，然后单击 **“Hyper-V 管理器”**。  
  
2.  在结果窗格中下,**虚拟机**，右键单击虚拟机，然后单击**导出**。  
  
3.  在中**导出虚拟机**对话框中，键入或浏览至具有足够的可用空间来存储所有虚拟机资源的位置。 当您导出虚拟机时，所有虚拟硬盘 （.vhd 文件或.vhdx 文件）、 检查点 （.avhd 文件） 和与虚拟机关联的已保存的状态文件复制到指定的文件夹。  
  
4.  单击**导出**。  
  
导出虚拟机后, 到其他服务器导入虚拟机。  
  
### <a name="to-import-a-virtual-machine-to-another-server"></a>若要将虚拟机导入到另一台服务器  
  
1.  连接到运行 HYPER-V 的服务器并打开 Hyper-v 管理器。  
  
2.  在中**操作**窗格中，单击**导入虚拟机**。  
  
3.  在中**导入虚拟机**对话框框中，指定导出虚拟机的位置。 除非你想要重新导入此虚拟机，则将导入设置保留原样。  
  
4.  单击**导入**。  
  
### <a name="to-configure-constrained-delegation"></a>配置约束委派的方法  
  
中的成员身份**域管理员**完成此过程所需的组。  
  
1.  已安装，在 Active Directory 域服务工具功能的计算机上**管理工具**，打开**Active Directory 用户和计算机**，然后导航到的计算机帐户运行 HYPER-V 的计算机。  
  
    > [!NOTE]  
    > 如果 **“Active Directory 用户和计算机”** 没有列出，则请安装 Active Directory 域服务工具功能。 有关说明，请参阅[安装 AD DS 的远程服务器管理工具](https://go.microsoft.com/fwlink/?LinkId=140463)(https://go.microsoft.com/fwlink/?LinkId=140463)。  
  
2.  右键单击运行 HYPER-V 的计算机的计算机帐户，然后单击**属性**。  
  
3.  在“委派”选项卡上，单击“仅信任此计算机来委派指定的服务”，然后单击“使用任何身份验证协议”。  
  
4.  若要允许 HYPER-V 计算机帐户提供委派的凭据到远程计算机：  
  
    1.  单击**添加**。  
  
    2.  在中**将服务添加**对话框中，单击**用户或计算机**，选择远程计算机，然后单击**确定**。  
  
    3.  在中**可用的服务**列表中，选择**cifs**协议 （也称为服务器消息块 (SMB) 协议），，然后单击**添加**。  
  
  
  


