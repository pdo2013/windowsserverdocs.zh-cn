---
title: Windows 管理中心 SDK 案例研究-QCT
description: Windows 管理中心 SDK 案例研究-QCT
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/14/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 2922bcdd08fac7bf2179a0ebbad37c7151d660b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357246"
---
# <a name="qct-management-suite-extension"></a>QCT 管理套件扩展

## <a name="a-simple-path-to-server-infrastructure-management"></a>服务器基础结构管理的简单路径

适用于 Windows 管理中心的 QCT 管理套件扩展提供了一个窗格，用于监视系统配置和管理[QCT AZURE STACK HCI 认证系统](https://go.qct.io/solutions/enterprise-private-cloud/qxstack-windows-server-cloud-ready-appliances/windows-server-software-defined-solution-wssd/)的服务器运行状况：[QUANTAGRID D52BQ-2u](https://www.qct.io/product/index/Server/rackmount-server/2U-Rackmount-Server/QuantaGrid-D52BQ-2U)， [QUANTAGRID D52T-1ULH](https://www.qct.io/product/index/Storage/Storage-Server/1U-Storage-Server/QuantaGrid-D52T-1ULH) And [QuantaPlex T21P-4u](https://www.qct.io/product/index/Storage/Storage-Server/4U-Storage-Server/QuantaPlex-T21P-4U)。

由于现有监视和管理方面的客户难题，QCT 提供了独占的互补特征和功能，其中包括系统事件日志、监视驱动程序和硬件组件运行状况的概述，以提高总体管理体验。

![QCT 扩展](../../media/extend-case-study-qct/D52T_DarkMode_Disk-Detail-General.PNG)

QCT 管理套件扩展了 Windows 管理中心的功能，其中包含以下主要功能：
- **一键式专用硬件管理**-直观的用户界面，显示硬件信息，包括型号名称、处理器、内存和 BIOS。 IT 管理员可以使用简单的一键式 UI 重启 BMC。

![QCT 扩展](../../media/extend-case-study-qct/D52T_Overview.PNG)

- **用于高效服务支持的磁盘映射和 LED 标识**-QCT 管理套件向导 UI 设计显示每个选定磁盘的槽，并概述所选磁盘的磁盘配置文件和 LED 光源控制以实现有效替换。

![QCT 扩展](../../media/extend-case-study-qct/T21P_disk_mapping.png)

- 用于硬件事件日志和运行状态的易于使用的监视工具。

![QCT 扩展](../../media/extend-case-study-qct/D52T_event_log.PNG)

- **预测磁盘管理**-使用 S. T 信息和不正常的通知评估系统条件，这会使组织能够在发生总体故障之前采取措施。

![QCT 扩展](../../media/extend-case-study-qct/T21P_SMART.PNG)

详细了解适用于 Windows 管理中心的 QCT 管理套件：
- [QCT 管理套件网页](https://go.qct.io/solutions/enterprise-private-cloud/qxstack-windows-server-cloud-ready-appliances/)
- [QCT 管理套件数据表](https://go.qct.io/wp-content/uploads/2019/04/WAC-data-sheet_v04222019.pdf)