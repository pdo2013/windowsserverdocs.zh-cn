---
title: 适用于 Windows Server 的 Windows Defender 概述
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-defender
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 751efb33-a08e-4e90-9208-6f2bc319e029
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 29506acf9ee7c52e100eb278c53205d03ef472d6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855618"
---
# <a name="windows-defender-antivirus-for-windows-server"></a>适用于 Windows Server 的 Windows Defender 防病毒

>适用于：Windows Server 2016

Windows Server 2016 现在包括 Windows Defender 防病毒软件。 Windows Defender AV 是立即并积极地可以抵御已知的恶意软件的 Windows Server 2016，并定期更新通过 Windows 更新的反恶意软件定义的恶意软件防护。

请参阅[在 Windows 10 的 Windows Defender 防病毒](https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10)文档库的详细信息。


尽管 Windows 10 和 Windows Server 2016 上的 Windows Defender AV 在功能、配置和管理方面大体相同，但仍有一些关键区别：

- 在 Windows Server 2016 中，会基于定义的服务器角色应用[自动排除项](https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus)。
- 在 Windows Server 2016 中，如果运行其他防病毒产品，Windows Defender AV 将无法禁用自己。

[Windows Server 2016 上的 Windows Defender 防病毒](https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016)主题包含一组设置和配置信息特定于 Windows Server 2016 中，其中包括如何：

-   [启用的界面](https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016#BKMK_UsingDef)

-   [验证 Windows Defender AV 正在运行]( https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016#BKMK_DefRun)

-   [更新反恶意软件定义]( https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016#BKMK_UpdateDef)

-   [提交示例]( https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016#BKMK_DefSamples)

-   [配置自动排除项]( https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016#BKMK_DefExclusions)
