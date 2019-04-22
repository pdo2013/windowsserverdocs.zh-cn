---
title: AD 林恢复-添加 GC
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 156a4a64d9c3bb8261bd603b72ae11b81ff1d152
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825628"
---
# <a name="ad-forest-recovery---adding-the-gc"></a>AD 林恢复-添加 GC

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

使用以下过程添加全局编录的 dc。  
  
## <a name="to-add-the-global-catalog"></a>若要添加全局编录  
  
1. 单击**启动**，依次指向**所有程序**，指向**管理工具**，然后单击**Active Directory 站点和服务**。  
2. 在控制台树中，展开**站点**容器，并选择包含目标服务器的相应站点。  
3. 展开**服务器**容器，然后展开你想要添加全局编录的 DC 的服务器对象。  
4. 右键单击**NTDS 设置**，然后单击**属性**。  
5. 选择**全局编录**复选框。  
![添加 GC](media/AD-Forest-Recovery-Add-GC/addgc1.png)

## <a name="to-add-the-global-catalog-using-repadmin"></a>若要添加使用 Repadmin 全局目录  

- 打开提升的命令提示符，键入以下命令，并按 ENTER:  

   ```  
   repadmin.exe /options DC_NAME +IS_GC  
   ```  

方法可以加快将全局编录添加到根域中的 DC 的过程如下：  

- 理想情况下，根级域中的 DC 应为非根域中还原的 Dc 的复制伙伴。 如果是这样，确认知识一致性检查器 (KCC) 已创建的相应**repsFrom**源 DC 和 DC 的根目录中的分区对象。 你可以通过运行确认这一点**repadmin /showreps /v**命令。 

- 如果没有任何**repsFrom**对象创建，请创建此对象的配置分区。 这样一来，根级域中的 DC 可以确定哪些非根级域中的域控制器已被删除。 可以使用以下命令来执行此操作：  

   ```
   repadmin /add ConfigurationNamingContext DestinationDomainController SourceDomainControllerCNAME  
   ```

   ```
   repadmin /options DSA -Disable_NTDSCONN_XLATE  
   ```

   格式*SourceDomainControllerCNAME*是：  

   ```
  
   sourceDCGuid._msdcs.root domain  
   ```

   例如，repadmin / 添加命令，为 contoso.com 域的配置分区可能是：  

   ```
   repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com  
   ```

- 如果**repsFrom**对象存在，尝试同步根级域中使用的非根域中的 DC 的 DC，如下所示：  

   ```
   Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID  
   ```

   其中*DestinationDomainController*中的根域的 dc 并*SourceDomainController*非根级域中的已还原 dc。 

- 根域的 DNS 服务器应具有源 DC 的别名 (CNAME) 资源记录。 确保父 DNS 区域包含委派资源记录 （名称服务器 (NS) 和主机 (A) 资源记录） 正确 Dc (已从备份中还原 Dc) 的子区域中。 
- 请确保根级域中的 DC 联系正确密钥分发中心 (KDC) 非根级域中。 若要测试此操作，请在命令提示符下，键入以下命令，然后按 ENTER:  

   ```
   nltest /dsgetdc:nonroot domain name /KDC /Force  
   ```

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)  
