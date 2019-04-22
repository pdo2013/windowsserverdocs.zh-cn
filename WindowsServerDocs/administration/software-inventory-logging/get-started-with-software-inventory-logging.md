---
title: 开始使用软件清单日志记录
description: 介绍如何安装和开始使用软件清单日志记录
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed51c13c-7cbf-4144-a675-011fd29379d4
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a3e51effa6321c3575e385f1c56bba57318eca5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822828"
---
# <a name="get-started-with-software-inventory-logging"></a>开始使用软件清单日志记录

>适用于：Windows Server （半年频道）、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

 软件清单日志记录收集每台服务器上的 Microsoft 软件清单数据。 在之前使用软件清单日志记录和 Windows Server 2012 R2，请确保该 Windows Update [KB 3000850](https://support.microsoft.com/kb/3000850)并[KB 3060681](https://support.microsoft.com/kb/3060681)安装在要列入清单的每个系统上。 不不需要 Windows Server 2016 的任何 Windows 更新。 此外，如果你想要使用 SIL 的功能将数据转发到聚合服务器，请确保为您的网络具有有效的 SSL 证书。

## <a name="BKMK_OVER"></a>功能说明
Windows Server 中软件清单日志记录功能包含一组简单的 PowerShell cmdlet，可帮助服务器管理员检索其服务器上安装的 Microsoft 软件的列表。 它还提供了使用 HTTPS 协议通过网络定期收集数据并将此数据转发到目标 Web 服务器进行聚合的功能。 对该功能（主要是按小时进行收集和转发）的管理也可以通过 PowerShell 命令完成。

> [!NOTE]
> 可以单独配置一台运行 Web 服务的聚合服务器。 了解有关 [软件清单日志记录聚合器](software-inventory-logging-aggregator.md)的详细信息。

> [!IMPORTANT]
> 软件清单日志记录功能在执行的过程中，不会将它收集到的任何数据发送到 Microsoft。

## <a name="BKMK_APP"></a>实际应用程序
软件清单日志记录旨在降低检索有关服务器上本地部署 Microsoft 软件的准确信息时存在的操作成本，尤其适用于这些软件分布在 IT 环境中的多台服务器上的情形（假设已在 IT 环境中部署并启用该功能）。 通过将这些数据转发到聚合服务器（如果 IT 管理员已单独设置此功能），你可以使用统一的自动化过程在一个位置收集数据。 尽管用户可以通过直接查询接口来实现此目的，但软件清单日志记录仍然通过采用一种可在每台服务器上启动的转发（通过网络）体系结构，来克服许多软件清单与资产管理方案经常存在的计算机发现难题。 将会使用 SSL 来确保通过 HTTPS 转发到管理员的聚合服务器的数据的安全。 将数据集中到一个位置（一台服务器上）可以更方便地分析、处理数据及生成相关报告。 必须指出的是，该功能在执行过程中不会将其中的任何数据发送到 Microsoft。 软件清单日志记录数据和功能仅供服务器软件的已授权所有者和管理员使用。

软件清单日志记录可以帮助服务器管理员执行以下任务：

-   从 Windows Server 远程和按需检索软件与服务器清单信息。

-   通过启用每台服务器的软件清单日志记录功能，并选择 Web 服务器目标 URI 和用于身份验证的证书指纹，聚合各种软件资产管理方案的软件与服务器清单信息。

## <a name="see-also"></a>请参阅
[软件清单日志记录聚合器](https://technet.microsoft.com/library/mt572043.aspx)<br>
[管理软件清单日志记录](manage-software-inventory-logging.md)<br>
[Windows PowerShell 中的软件清单日志记录 Cmdlet](https://technet.microsoft.com/library/dn283390.aspx)<br>
[Microsoft Assessment and Planning Toolkit](https://www.microsoft.com/download/en/details.aspx?id=7826)
[批量激活管理工具](http://blogs.technet.com/b/volume-licensing/)

