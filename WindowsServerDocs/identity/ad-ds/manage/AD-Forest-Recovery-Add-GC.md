---
title: "广告森林恢复-添加 GC"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 3749fd99737f61c55871f613b9feaa21090d3bae
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---adding-the-gc"></a>广告森林恢复-添加 GC 

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 使用以下步骤添加到 DC 全球目录。  
  
## <a name="to-add-the-global-catalog"></a>若要添加全球目录  
  
1.  单击**开始**，指向**所有程序**，指向**管理工具**，然后单击**Active Directory 的站点和服务**。  
2.  控制台树中，展开**站点**容器，然后依次选择包含目标服务器的相应站点。  
3.  展开**服务器**容器，然后为你想要添加全球目录 DC 展开服务器对象。  
4.  右键单击**各自的**，然后单击**属性**。  
5.  选择**全球目录**复选框。  
![添加 GC](media/AD-Forest-Recovery-Add-GC/addgc1.png)
  
## <a name="to-add-the-global-catalog-using-repadmin"></a>若要添加使用 Repadmin 全球目录  
  
1.  打开提升了权限的命令提示符下，键入以下命令，并按 ENTER:  
  
    ```  
    repadmin.exe /options DC_NAME +IS_GC  
    ```  
  
 以下是加快添加到 DC 根域中的全局目录的过程的方法：  
  
-   理想的情况下，根域中的直流应该非根域中的恢复 Dc 复制合作伙伴。 如果是这样，确认知识一致性检查 (KCC) 已创建相应**repsFrom**源 DC 和分区中根直流对象。 你可以通过运行确认**repadmin /showreps /v**命令。  
  
-   如果没有任何**repsFrom**对象创建、创建配置分区此对象。 这样一来，根域中的直流可以确定哪些 Dc 非根域中的已被删除。 你可以使用以下命令：  
  
    ```  
    repadmin /add ConfigurationNamingContext DestinationDomainController SourceDomainControllerCNAME  
    ```  
  
    ```  
    repadmin /options DSA -Disable_NTDSCONN_XLATE  
    ```  
  
     对于格式*SourceDomainControllerCNAME*是：  
  
    ```  
  
    sourceDCGuid._msdcs.root domain  
    ```  
  
     例如，可能是配置分区的 contoso.com 域 repadmin /add 命令：  
  
    ```  
    repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com  
    ```  
  
-   如果**repsFrom**物体，尝试将与非根域中 DC 根域中的 DC 同步，如下所示：  
  
    ```  
    Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID  
    ```  
  
     其中*DestinationDomainController*是根域中的 DC 和*SourceDomainController*是非根域中的恢复的 DC。  
  
-   根域 DNS 服务器应有源直流 (CNAME) 别名资源记录。 确保家长 DNS 区域包含委派资源记录（名称服务器 (NS) 和主机 (A) 资源记录）的正确域控制器 (Dc 已从备份中还原) 中的孩子区域。  
  
-   请确保根域中的直流联系正确密钥 Distribution 中心 (KDC) 中的非根域。 对此进行测试，在命令提示符下，键入以下命令，，然后按 ENTER:  
  
    ```  
    nltest /dsgetdc:nonroot domain name /KDC /Force  
    ```  
## <a name="next-steps"></a>后续步骤

- [广告林恢复指南](AD-Forest-Recovery-Guide.md)
- [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)  
