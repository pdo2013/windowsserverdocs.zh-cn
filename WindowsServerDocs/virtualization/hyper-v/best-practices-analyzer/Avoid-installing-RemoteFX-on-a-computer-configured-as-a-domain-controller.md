---
title: 避免在配置为 Active Directory 域控制器的计算机上安装 RemoteFX
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: da58694e-91f6-45d8-a599-18966db165f4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 67fd8e2568691b7e9be4b46e30b64bf44558d6d0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366469"
---
# <a name="avoid-installing-remotefx-on-a-computer-that-is-configured-as-an-active-directory-domain-controller"></a>避免在配置为 Active Directory 域控制器的计算机上安装 RemoteFX

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|Error|  
|**类别**|配置|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>**问题**  
*RemoteFX 安装在域控制器上。*  
  
## <a name="impact"></a>**对**  
*无法在这些计算机上使用为 RemoteFX 配置的虚拟机。*  
  
## <a name="resolution"></a>**解决方法**  
*决定是要将此服务器配置为使用 RemoteFX 作为 Hyper-v 还是 Active Directory 域控制器，然后根据需要重新配置服务器。*  
  


