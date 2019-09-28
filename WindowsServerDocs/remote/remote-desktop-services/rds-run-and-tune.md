---
title: RDS - 运行和调整
description: 提供远程桌面服务的管理信息。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 02/08/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79909767-a4c3-4ecf-8d3f-77d37a663153
author: spatnaik
manager: scottman
ms.openlocfilehash: 5a2aa5c166b41ed8bd04ccfb92911368c07a5459
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387163"
---
# <a name="run-and-tune-your-remote-desktop-services-environment"></a>运行并调整远程桌面服务环境

调整部署不仅需要时间，还需要检测和监视。 可使用以下过程优化远程桌面部署、使其保持运行以及根据需要进行横向扩展（和缩小）。 

持续评估各项指标并平衡运行成本是一种很好的做法。

## <a name="management-and-monitoring"></a>管理和监视

有关如何管理对桌面和远程资源的访问的信息，请查看[管理 RDS 集合中的用户](rds-user-management.md)。

使用 **Microsoft Operations Management Suite (OMS)** 监视远程桌面部署是否存在潜在瓶颈，并使用以下方法之一管理它们： 

- **服务器管理器**：使用内置于 Windows Server 的 RD 管理工具来管理具有多达 500 个并发远程最终用户的部署。 
- **PowerShell**：使用同样内置于 Windows Server 的 RD PowerShell 模块来管理具有多达 5000 个并发远程最终用户的部署。

## <a name="scale-bigger-better-faster"></a>缩放：更大、更好、更快

借助对部署的可见性，可以更精确地控制缩放。 根据缩放需求轻松添加或删除远程桌面主机服务器。 

以 Azure 为基础的远程桌面部署可以使用 Azure 服务（如 Azure SQL）按需自动缩放。

## <a name="automation-script-for-success"></a>自动化：获得成功所需的脚本

维护一个持续运行且高度扩展的应用程序需要定期执行一些重复操作。 可使用远程桌面服务 PowerShell cmdlet 和 WMI 提供程序来开发可在需要时在多个部署上运行的脚本。 对部署中的远程桌面服务运行最佳做法分析器 (BPA) 规则以调整部署。

## <a name="load-testing-avoid-surprises"></a>负载测试：避免意外情况

使用压力测试和实际使用情况模拟对部署进行负载测试。 改变负载大小以避免意外情况！ 确保响应能力满足用户需求，并确保整个系统可复原。 使用 LoginVSI 等模拟工具创建负载测试，以检查部署能否满足用户需求。 