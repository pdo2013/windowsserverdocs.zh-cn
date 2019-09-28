---
title: 副本服务器必须配置为接受复制请求
description: 提供有关如何解决此最佳做法分析器规则报告的问题的说明。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 54868d4db2dccc893bd2897134d9125446873384
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366722"
---
# <a name="a-replica-server-must-be-configured-to-accept-replication-requests"></a>副本服务器必须配置为接受复制请求

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|Error|  
|**类别**|配置|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。
  
## <a name="issue"></a>问题  
*此计算机被指定为 Hyper-v 副本服务器，但未配置为接受来自主服务器的传入复制数据。*  
  
## <a name="impact"></a>影响  
*此服务器无法接受来自主服务器的复制流量。*  
  
## <a name="resolution"></a>分辨率  
*使用 Hyper-v 管理器指定此副本服务器应该接受复制数据的主服务器。*  
  
#### <a name="create-authorization-entries-using-hyper-v-manager"></a>使用 Hyper-v 管理器创建授权条目  
  
1.  打开 Hyper-V 管理器。 （从服务器管理器中，单击 "**工具**"  >  "**hyper-v 管理器**"。）  
  
2.  在主机列表中，右键单击所需的主机，然后单击 " **Hyper-v 设置**"。  
  
3.  在导航窗格中，单击 "**复制配置**"。  
  
4.  在 "**授权和存储**" 下，单击 **"允许从指定的服务器复制"** 。  
  
5.  在服务器列表下方，单击 "**添加**"。  
  
6.  在 "**添加授权条目**：  
  
    -   键入第一个服务器的完全限定名称。  
  
    -   指定一个专用位置以仅存储该服务器的文件。  
  
7.  单击 **“确定”** 。  
  
8.  为每个主服务器重复上述步骤。  
  
9. 再次单击 **"确定"** 以完成，然后关闭窗口。  
  
### <a name="create-authorization-entries-using-windows-powershell"></a>使用 Windows PowerShell 创建授权条目  
  
1.  打开 Windows PowerShell。 （在桌面上，单击 "开始"，然后开始键入**Windows PowerShell**。）  
  
2.  右键单击 " **Windows PowerShell** "，然后单击 "**以管理员身份运行**"。  
  
3.  运行与下面类似的命令，替换：  
  
    -   具有服务器的完全限定域名的 server01.domain01.contoso.com 的主服务器名称。  
  
    -   你所在位置的 D:\ReplicaVMStorage 的位置。  
  
    -   名为 DEFAULT 的信任组（如果已创建）。 否则，请使用默认值。  
  
```  
New-VMReplicationAuthorizationEntry server01.domain01.contoso.com D:\ReplicaVMStorage DEFAULT  
```  
  


