---
title: Microsoft Server Performance Advisor
description: Microsoft Server Performance Advisor
ms.assetid: 468ebcb3-9eaf-477c-ab10-e3f1b3ce63dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: manage
ms.openlocfilehash: ab124f3efabf2ac3ae8904157a81587c0c21ebb5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856328"
---
# <a name="microsoft-server-performance-advisor"></a>Microsoft Server Performance Advisor

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

下载 Microsoft Server Performance Advisor (SPA) 可帮助您诊断 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 部署中的性能问题。 SPA 生成全面的诊断报告和图表，并提供建议来帮助你快速分析问题和制订更正措施。

-   [服务器性能顾问概述](#bkmk-aboutspa)

-   [下载服务器性能顾问](#bkmk-downloadspa)

-   [Server Performance Advisor 用户指南](server-performance-advisor-users-guide.md)

-   [服务器性能顾问包开发指南](server-performance-advisor-pack-development-guide.md)

## <a href="" id="bkmk-aboutspa"></a>服务器性能顾问概述

服务器性能顾问组成两个部分，SPA 框架和 SPA Advisor 包。

### <a name="the-server-performance-advisor-framework"></a>服务器性能顾问 framework

负责收集 advisor 包指定的数据、 收集的数据写入到 Microsoft SQL Server 数据库、 创建 IT 友好型环境对于 SPA 顾问包，运行脚本并显示最终报表引擎。 只需在 SPA 控制台上安装 SPA 框架。 也可以远程访问待测试的服务器或待测试的服务器上安装在独立计算机上安装了 SPA 控制台。

### <a name="server-performance-advisor-packs"></a>Server Performance Advisor 包

SPA 顾问包是所有优化规则，其中包含一系列的元数据和 SQL 脚本文件的中心。 SPA 附带以下顾问包：

-   核心操作系统顾问包分析性能的常规操作系统功能，独立于专门的服务器角色。

-   Internet 信息服务器 (IIS) advisor 包跟踪 IIS 的性能。

-   HYPER-V Advisor 包分析的 HYPER-V 服务器角色的常规性能。

    **请注意**HYPER-V Advisor 包不会分析来宾操作系统。

     

-   Active directory 顾问包分析 active directory 角色的常规性能。

SPA 还提供了非 Microsoft 开发人员能够编写 advisor 包来满足自己需求的可扩展模型。

**请注意**SPA 无法理解所有硬件和用户方案上下文。 应使用提供的工具以帮助你做出决策，并了解对服务器所做的任何潜在更改的后果的建议。

 

## <a href="" id="bkmk-downloadspa"></a>下载服务器性能顾问


使用以下链接下载服务器性能顾问 for Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008:

-   [Microsoft Server Performance Advisor 3.1 （32 位）](https://go.microsoft.com/fwlink/p/?linkid=327751)

-   [Microsoft Server Performance Advisor 3.1 （64 位）](https://go.microsoft.com/fwlink/p/?linkid=327752)

可以通过使用以下命令提取 CAB 文件中的文件：

-   适用于 x86 版本： `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_x86.cab`

-   针对 x64 版本： `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_amd64.cab`

**警告：** SPA 时提取.cab 文件，必须保留层次结构的目录结构才能正常工作。 具体取决于在服务器安装的 CAB 工具，提取可能会导致不可操作状态的目录结构。 若要保留的层次结构的目录结构，可以使用文件目录结构中提取一个 CAB 提取实用程序工具。

如果正确 CAB 提取工具提取的文件，将自动提取目标文件夹中显示子文件夹。

### <a name="spa-prerequisites"></a>SPA 先决条件

SPA 控制台需要安装以下软件：

-   Microsoft .NET Framework 4

-   SQL Server 2008 R2 Express SP1 or Microsoft SQL Server 2008 R2 SP1

可能会较新版本兼容。 将记录与 SPA 控制台的任何已知的产品不兼容性。
