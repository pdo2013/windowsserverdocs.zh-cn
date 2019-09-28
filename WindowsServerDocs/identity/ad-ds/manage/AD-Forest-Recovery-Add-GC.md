---
title: AD 林恢复-添加 GC
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: f82033dd042847c7c735423c25756b936b137230
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369338"
---
# <a name="ad-forest-recovery---adding-the-gc"></a>AD 林恢复-添加 GC

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

使用以下过程将全局编录添加到 DC。  
  
## <a name="to-add-the-global-catalog"></a>添加全局编录  
  
1. 单击 "**开始**"，指向 "**所有程序**"，指向 "**管理工具**"，然后单击 " **Active Directory 站点和服务**"。  
2. 在控制台树中，展开 "**站点**" 容器，然后选择包含目标服务器的相应站点。  
3. 展开 "**服务器**" 容器，然后展开要向其添加全局编录的 DC 的服务器对象。  
4. 右键单击 " **NTDS 设置**"，然后单击 "**属性**"。  
5. 选中 "**全局编录**" 复选框。  
![Add GC @ no__t-1

## <a name="to-add-the-global-catalog-using-repadmin"></a>使用 Repadmin 添加全局编录  

- 打开提升的命令提示符，键入以下命令，然后按 ENTER：  

   ```  
   repadmin.exe /options DC_NAME +IS_GC  
   ```  

以下是加快将全局编录添加到根域中 DC 的过程的方法：  

- 理想情况下，根域中的 DC 应为非根域中已还原 Dc 的复制伙伴。 如果是这样，请确认知识一致性检查器（KCC）已为根 dc 中的源 DC 和分区创建相应的**repsFrom**对象。 可以通过运行**repadmin/showreps/v**命令来确认这一点。 

- 如果未创建任何**repsFrom**对象，则为配置分区创建此对象。 这样一来，根域中的 DC 可以确定已删除非根域中的 dc。 可以通过以下命令执行此操作：  

   ```
   repadmin /add ConfigurationNamingContext DestinationDomainController SourceDomainControllerCNAME  
   ```

   ```
   repadmin /options DSA -Disable_NTDSCONN_XLATE  
   ```

   *SourceDomainControllerCNAME*的格式为：  

   ```
  
   sourceDCGuid._msdcs.root domain  
   ```

   例如，contoso.com 域的配置分区的 repadmin/add 命令可能是：  

   ```
   repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com  
   ```

- 如果**repsFrom**对象存在，请尝试将根域中的 dc 与非根域中的 dc 同步，如下所示：  

   ```
   Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID  
   ```

   其中， *DestinationDomainController*是根域中的 Dc， *SourceDomainController*是非根域中的还原 dc。 

- 根域 DNS 服务器应该具有源 DC 的别名（CNAME）资源记录。 确保父 DNS 区域包含子区域中正确 Dc （已从备份还原的 Dc）的委派资源记录（名称服务器（NS）和主机（A）资源记录）。 
- 请确保根域中的 DC 正在联系非根域中的正确密钥发行中心（KDC）。 若要对此进行测试，请在命令提示符下键入以下命令，然后按 ENTER：  

   ```
   nltest /dsgetdc:nonroot domain name /KDC /Force  
   ```

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)  
