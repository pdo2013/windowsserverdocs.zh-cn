---
title: RDS-运行和优化
description: 提供远程桌面服务的管理的信息。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 40f8dbd560da359e8764ed715e7776cc2d230a7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862178"
---
# <a name="run-and-tune-your-remote-desktop-services-environment"></a>运行和调整你的远程桌面服务环境

优化您的部署时间和需要进行检测和监视。 使用下面的进程来改善远程桌面部署，使其保持运行并实现根据需要进行扩展扩展 （和缩减）。 

它是一个好办法不断地进行评估的指标和针对正在运行的成本的平衡。

## <a name="management-and-monitoring"></a>管理和监视

请查看[管理 RDS 集合中的用户](rds-user-management.md)有关如何管理对你的桌面和远程资源访问权限信息。

使用**Microsoft Operations Management Suite (OMS)** 来监视潜在的瓶颈的远程桌面部署和管理使用下列方法之一： 

- **服务器管理器**:使用内置于 Windows Server 以使用最多 500 个并发远程最终用户管理部署 RD 管理工具。 
- **PowerShell**:使用 RD PowerShell 模块，也内置于 Windows Server 中，若要使用最多 5000 个并发远程最终用户管理部署。

## <a name="scale-bigger-better-faster"></a>缩放：更大、 更好的更快

使用到部署中的可见性，可以控制缩放性更高的精度。 轻松地添加或删除远程桌面主机服务器根据规模需求。 

请在 Azure 构建的远程桌面部署可以使用的 Azure 服务，如 Azure SQL、 按需自动缩放。

## <a name="automation-script-for-success"></a>自动化：成功的脚本

维护一个正在运行的高度可扩展应用程序涉及到重复定期进行的操作。 使用远程桌面服务 PowerShell cmdlet 和 WMI 提供程序来开发可以在需要时的多个部署运行的脚本。 在你的部署来优化你的部署上运行远程桌面服务的最佳做法分析器 (BPA) 规则。

## <a name="load-testing-avoid-surprises"></a>负载测试：避免意外情况

负载测试的压力测试和模拟的实际使用情况的部署。 改变负载大小，以避免意外情况 ！ 请确保响应能力满足用户需求，并灵活处理整个系统。 使用模拟工具，如 LoginVSI，检查你的部署能力满足用户的需求，创建负载测试。 