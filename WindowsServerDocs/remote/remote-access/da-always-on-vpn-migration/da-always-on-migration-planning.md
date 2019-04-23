---
title: 远程访问 Always On VPN 迁移规划
description: 将 DirectAccess 迁移到 Always On VPN 需要适当的计划以确定你的迁移阶段，它可帮助确定任何问题影响到整个组织之前。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/29/2018
ms.openlocfilehash: 494dc7916b505991c22b07bec738c2300d660ec1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835668"
---
# <a name="plan-the-directaccess-to-always-on-vpn-migration"></a>规划 DirectAccess 到始终启用 VPN 迁移

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows 10

&#171;[**前面：** Always On VPN 迁移到 DirectAccess 的概述](da-always-on-migration-overview.md)<br>
&#187;[**下一步：** 将迁移到 Always On VPN 和取消配置 DirectAccess](da-always-on-migration-deploy.md)


将 DirectAccess 迁移到 Always On VPN 需要适当的计划以确定你的迁移阶段，它可帮助确定任何问题影响到整个组织之前。 迁移的主要目标是保持远程连接到在整个过程中的 office 用户。 如果执行不按顺序的任务，可能发生的争用条件，因此，没有访问公司资源的方法，远程用户。 因此，Microsoft 建议使用 Always On VPN 从 DirectAccess 执行计划内的、 由并行迁移。 有关详细信息，请参阅[Always On VPN 迁移部署](da-always-on-migration-deploy.md)部分。

一节介绍分离用户迁移、 标准配置注意事项和 Always On VPN 的增强功能的好处。 迁移规划阶段包括：

1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

## <a name="build-migration-rings"></a>生成迁移环
迁移通道用于将 Always On VPN 客户端迁移工作划分为多个阶段。 获取到最后一个阶段时，你的过程应该也经过测试且一致。

本部分提供了一种方法将用户划分到迁移阶段，然后管理将这些阶段。 无论您选择的用户阶段分离方法，在迁移完成后维护一个 VPN 用户组中的，更轻松地管理。

>[!NOTE] 
>单词_阶段_不应以指示这是一个很长的过程。 是否在几天或几个月中每个阶段中移动时，Microsoft 建议您充分利用通过并行迁移，并使用分阶段的方法。

### <a name="benefits-of-dividing-the-migration-effort-into-multiple-phases"></a>将迁移工作划分为多个阶段的优点

-   **大容量的服务中断保护。** 通过将划分成阶段的迁移，迁移生成问题可能会影响的人数是要小得多。

-   **改进了的进程或从反馈的通信。** 理想情况下，用户未完全注意不到发生迁移。 但是，如果他们的体验是较不理想，这类应用的反馈会允许您有机会提高您的规划并避免将来出现问题。

### <a name="tips-for-building-your-migration-ring"></a>生成迁移环的相关提示

-   **标识远程用户。** 首先，将用户分成两个存储桶： 那些经常进入办公室和不这样做的任何人。 迁移过程是相同的两个组，但很可能需要更长时间，以接收更新的用户更频繁地连接比远程客户端。 理想情况下，每个迁移阶段，应包括每个存储桶中的成员。

-  **确定用户的优先级。** 领导力和其他影响较大的用户通常在最后一个用户间迁移。 时确定用户的优先级，但是，如果其客户端计算机的迁移失败考虑其业务生产效率影响。 例如，如果您有其分级为 1 到 3，使用 1 表示，员工将不能与工作和 3，这意味着迫切的工作不会中断，业务分析员远程使用仅内部业务线 (LOB) 应用会为 1，而销售人员使用云 应用程序就会 3。

-   **在多个阶段中迁移每个部门或业务单位。** Microsoft 强烈建议不要在同一时间迁移针对整个部门。 如果应会出现问题，您又不希望会影响整个部门远程工作。 相反，迁移在至少两个阶段中的每个部门或业务单位。

-   **逐渐增加用户计数。** 最常见迁移方案开始 IT 组织中的成员，然后将其移动到跟领导力和其他影响较大的用户的业务用户。 每个迁移阶段通常涉及以渐进方式更多的人。 例如，第一阶段可能包括有十个用户，以及最后一组可包含 5,000 个用户。 为了简化部署，创建一个单独的 VPN 用户安全组，并将用户添加到它，因为其阶段到达。 在这种方式，您最终会得到单一 VPN 用户向其中您可以添加的组成员在将来。

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../includes/always-on-vpn-standard-config-considerations-include.md)]


## <a name="next-step"></a>下一步

|如果想要...  |然后请参阅...  |
|---------|---------|
|开始将迁移到 Always On VPN     |[将迁移到 Always On VPN 和取消配置 DirectAccess](da-always-on-migration-deploy.md)。 将 DirectAccess 迁移到 Always On VPN 要求特定的进程以迁移客户端，这有助于最大程度减少执行迁移步骤顺序从出现的争用条件。         |
|了解有关 Always On VPN 和 DirectAccess 的功能    |[功能比较的 Always On VPN 和 DirectAccess](../vpn/vpn-map-da.md)。 在以前版本的 Windows VPN 体系结构，平台限制进行难提供替换 （例如在用户登录之前启动的自动连接） 的 DirectAccess 所需的关键功能。 Always On VPN，但是，缓解大部分这些限制或扩展之外的 DirectAccess 功能的 VPN 功能。         |



---