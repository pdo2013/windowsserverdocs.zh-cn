---
title: Microsoft Server Performance Advisor
description: Microsoft Server Performance Advisor
ms.assetid: 468ebcb3-9eaf-477c-ab10-e3f1b3ce63dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.topic: article
ms.prod: windows-server
ms.technology: manage
ms.openlocfilehash: 49f6132cfe99d9d4b719aeeecf149ecb1d7b76f2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382994"
---
# <a name="microsoft-server-performance-advisor"></a>Microsoft Server Performance Advisor

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

下载 Microsoft Server Performance Advisor （SPA），帮助诊断 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 部署中的性能问题。 SPA 生成全面的诊断报告和图表，并提供建议来帮助你快速分析问题和制定纠正措施。

-   [服务器性能顾问概述](#bkmk-aboutspa)

-   [下载服务器性能顾问](#bkmk-downloadspa)

-   [erver Performance Advisor 用户指南](server-performance-advisor-users-guide.md)

-   [Server Performance Advisor 包开发指南](server-performance-advisor-pack-development-guide.md)

## <a href="" id="bkmk-aboutspa"></a>服务器性能顾问概述

服务器性能顾问由两部分组成： SPA 框架和 SPA 顾问包。

### <a name="the-server-performance-advisor-framework"></a>服务器性能顾问框架

负责收集由 advisor 包指定的数据、将收集的数据写入 Microsoft SQL Server 数据库、创建适合于 SPA 的环境以运行 SPA Advisor 包的脚本以及显示最终报表的引擎。 只需在 SPA 控制台上安装 SPA 框架即可。 SPA 控制台可以安装在独立的计算机上，以远程访问受测服务器或要安装在受测服务器上的服务器。

### <a name="server-performance-advisor-packs"></a>Server Performance Advisor 包

SPA 顾问包是所有优化规则的中心，其中包含一系列的元数据和 SQL 脚本文件。 SPA 附带以下 Advisor 包：

-   核心操作系统顾问包分析一般操作系统函数的性能，而不考虑专用服务器角色。

-   Internet Information Server （IIS） advisor 包跟踪 IIS 的性能。

-   Hyper-v Advisor 包分析 Hyper-v 服务器角色的常规性能。

    **注意**Hyper-v Advisor 包不分析来宾操作系统。

     

-   Active directory advisor 包分析 active directory 角色的常规性能。

SPA 还为非 Microsoft 开发人员提供了一个可扩展模型，以满足其需求。

**注意**SPA 无法识别所有硬件和用户方案上下文。 你应使用该工具提供的建议来帮助你做出决策，并了解对服务器所做的任何可能更改的后果。

 

## <a href="" id="bkmk-downloadspa"></a>下载服务器性能顾问


使用以下链接下载适用于 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 的服务器性能顾问：

-   [Microsoft Server Performance Advisor 3.1 （32）](https://go.microsoft.com/fwlink/p/?linkid=327751)

-   [Microsoft Server Performance Advisor 3.1 （64）](https://go.microsoft.com/fwlink/p/?linkid=327752)

可以使用以下命令提取 CAB 文件中的文件：

-   对于 x86 版本： `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_x86.cab`

-   对于 x64 版本： `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_amd64.cab`

**警告**提取 .cab 文件时，SPA 必须保留分层目录结构才能正常工作。 根据您的服务器上安装的 CAB 工具，提取可能会导致无法操作的目录结构。 若要保留分层目录结构，可以使用解压缩文件目录结构的 CAB 提取实用工具工具。

如果 CAB 提取工具正确提取了文件，子文件夹将自动显示在提取目标文件夹中。

### <a name="spa-prerequisites"></a>SPA 必备组件

SPA 控制台要求安装以下软件：

-   Microsoft .NET Framework 4

-   SQL Server 2008 R2 Express SP1 或 Microsoft SQL Server 2008 R2 SP1

较新版本可能兼容。 将记录任何与 SPA 控制台有关的已知产品不兼容性。
