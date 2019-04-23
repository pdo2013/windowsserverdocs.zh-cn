---
title: 配置基于证书的身份验证，但在副本服务器或故障转移群集节点上未安装指定的证书
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4cabbce3-9367-4ddc-a108-1e5e1ab2bcff
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: feef30d2f798057e4bd8e53ebab240af6b1d25b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832938"
---
# <a name="certificate-based-authentication-is-configured-but-the-specified-certificate-is-not-installed-on-the-replica-server-or-failover-cluster-nodes"></a>配置基于证书的身份验证，但在副本服务器或故障转移群集节点上未安装指定的证书

>适用于：Windows Server 2016


  
*有关最佳做法和扫描的详细信息，请参阅*[最佳做法分析器](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|错误|  
|**类别**|配置|  

在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。

## <a name="issue"></a>问题  
  
*已配置的 HYPER-V 副本使用来提供基于证书的复制副本服务器 （或任何故障转移群集节点） 上未安装安全证书。*  
  
## <a name="impact"></a>影响  
  
*在发生群集故障转移或迁移到另一个节点，如果新节点还没有安装相应的证书，将暂停的 HYPER-V 复制。这会影响以下节点：*  
  
\<节点的列表 >  
  
## <a name="resolution"></a>分辨率  
  
*在副本服务器 （和所有相关的节点在故障转移群集中，如果有） 上安装配置的证书。*  
  


