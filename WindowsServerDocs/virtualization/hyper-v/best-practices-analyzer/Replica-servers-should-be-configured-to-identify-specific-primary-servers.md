---
title: 应将副本服务器配置为标识有权发送复制流量的特定主服务器
description: 提供说明来解决此最佳实践分析程序规则所报告的问题。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 0aeb1f4b-2e75-430b-9557-fe64738c4992
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 47b215d4c84e68d93ae1189ddd370358e2781eff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822238"
---
# <a name="replica-servers-should-be-configured-to-identify-specific-primary-servers-authorized-to-send-replication-traffic"></a>应将副本服务器配置为标识有权发送复制流量的特定主服务器

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
*经过配置之后，此副本服务器接受从所有主服务器的复制流量，并将其存储在一个位置。*  
  
### <a name="impact"></a>影响  
*从所有主服务器的所有复制都存储在一个位置，可能会引入的隐私或安全问题。*  
  
## <a name="resolution"></a>分辨率  
*使用 Hyper-v 管理器来创建新的授权条目的特定主服务器，并指定为每个单独的存储位置。可以使用通配符来分组到的每个授权项集的主服务器。*  
  
#### <a name="create-authorization-entries-using-hyper-v-manager"></a>创建使用 Hyper-v 管理器的授权条目  
  
1.  打开 Hyper-V 管理器。 (从服务器管理器中，单击**工具** > **Hyper-v 管理器**。)  
  
2.  从主机列表中，右键单击的一个，然后单击**HYPER-V 设置**。  
  
3.  在导航窗格中，单击**复制配置**。  
  
4.  下**授权和存储**，单击**允许从指定的服务器进行复制**。  
  
5.  下面的服务器列表中，单击**添加**。  
  
6.  下**添加的授权条目**:  
  
    -   键入第一台服务器的完全限定的名称。  
  
    -   指定用于存储只有该服务器的文件的专用的位置。  
  
7.  单击 **“确定”**。  
  
8.  重复的每个主服务器。  
  
9. 单击**确定**再次以完成并关闭窗口。  
  
### <a name="create-authorization-entries-using-windows-powershell"></a>创建使用 Windows PowerShell 的授权条目  
  
1.  打开 Windows PowerShell。 (在桌面上，单击开始，然后开始键入**Windows PowerShell**。)  
  
2.  右键单击**Windows PowerShell**然后单击**以管理员身份运行**。  
  
3.  运行类似以下的命令，替换为：  
  
    -   主服务器的 server01.domain01.contoso.com 名称与你的服务器的完全限定的域名。  
  
    -   使用你的位置的 D:\ReplicaVMStorage 位置。  
  
    -   如果你已创建了一个信任组名与你的组的名称为 DEFAULT。 如果没有，则使用默认值。  
  
```  
New-VMReplicationAuthorizationEntry server01.domain01.contoso.com D:\ReplicaVMStorage DEFAULT  
```  
  
## <a name="see-also"></a>请参阅  
[New-VMReplicationAuthorizationEntry](https://technet.microsoft.com/library/hh848606.aspx)  
  


