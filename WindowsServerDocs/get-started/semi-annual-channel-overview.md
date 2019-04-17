---
title: Windows Server 半年频道概述
description: Microsoft 即将简化对 Windows Server 的维护，使操作系统的更新更易于测试、管理和部署。
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: high
ms.date: 05/07/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9cf87597-b15d-4f43-8aa1-91e60367f011
ms.openlocfilehash: 2995fca3085d6611ecce083685dca0e587913f17
ms.sourcegitcommit: fcc26ec5a2cc73b59c5752377b39c070d288655e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/19/2018
ms.locfileid: "8976723"
---
# Windows Server 半年频道概述

>适用于：Windows Server（半年频道）

Windows Server 版本模型将提供新的选项，从而与类似版本保持一致，并且将为用于 [Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview) 和 [Office 365 专业增强版](https://support.office.com/article/Overview-of-the-upcoming-changes-to-Office-365-ProPlus-update-management-78b33779-9356-4cdf-9d2c-08350ef05cca?ui=en-US&rs=en-US&ad=US)的模型提供维护。 已使用过 Windows 10 或 Office 365 专业增强版的用户可能已经熟悉此类改进之处。

**目前为 Windows Server 客户提供的两种主要版本频道为 Long-Term Servicing Channel 和半年频道。** 你可以将服务器置于 Long-Term Servicing Channel (LTSC)、移到新半年频道或将部分服务器置于任一频道，具体取决于你的需求。


## Long-Term Servicing Channel (LTSC)
用户已熟悉了解该版本模型（以前称为“Long-Term Servicing *Branch*”)，其中 Windows Server 的全新主要版本每 2-3 年发布一次。 用户有权享受 5 年的主流支持和 5 年的延长支持。 该渠道适用于需要更长时间服务选项和功能稳定性的系统。 新的半年频道版本不会影响 Windows Server 2016 和 Windows Server 早期版本的部署工作。 Long-Term Servicing Channel 将持续接受安全和非安全更新，但不会接受新特性和新功能。

> [!Note]  
> **当前的 LTSC 产品是 Windows Server 2016**。 如果你想坚持使用此频道，则应该安装（或继续使用）Windows Server 2016，你可以通过服务器核心安装选项或带桌面体验的服务器安装选项进行安装。 有关详细信息，请参阅 [Windows Server 2016 入门](server-basics.md)。 



## 半年频道 
半年频道是快速创新客户的最佳选- 这类用户可以在应用程序（特别是基于容器和微服务的应用程序）以及软件定义混合数据中心中更快地利用新操作系统功能。 以半年频道发布的 Windows Server 产品每年发布两次新版本，分别在春季和秋季。 每个以该频道发布的版本自初始版本开始享受 18 个月的支持服务。

半年频道中引入的大部分功能将汇总至 Windows Server 的 Long-Term Servicing Channel 版本。 版本、功能和支持内容可能因版本而异，具体取决于客户反馈。

可使用[软件保障](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx)，也可以通过 Azure Marketplace 或其他云/托管服务提供商及 Visual Studio 订阅等会员计划为批量授权的客户提供半年频道。

> [!Note]  
> **当前的半年频道版本是 Windows Server 版本 1803**。 如果你想将服务器置于此频道，则应该安装 Windows Server 版本 1803，该版本可以在服务器核心模式下安装或作为容器中运行的 Nano Server 来安装。 请参阅 [Windows Server 版本 1803 简介](get-started-with-1803.md)，以了解如何获取和激活 Windows Server 版本 1803。 不支持从 Windows Server 2016 就地升级为 Windows Server 版本 1803，因为两者位于**不同的发布频道**。 Windows Server 版本 1803 不是 Windows Server 2016 的更新版，而是此半年频道上的下一个 Windows Server 版本。



在该模型中，Windows Server 版本通过发布的年份和月份进行标识：例如，2017 年 9 月（九月）发布的版本标识为**版本 1709**。 以半年频道发布的 Windows Server 每年发布两次新版本。 每个版本的支持周期为 18 个月。

## 在 LTSC 上保留服务器还是将它们转移至半年频道？
以下是需考虑的主要区别：

- 是否需要快速创新？ 是否需要提前访问最新的 Windows Server 功能？ 是否需要支持快速节奏混合应用程序、dev-ops 和 Hyper-V 结构？ 如果需要，你应该考虑安装 [Windows Server 版本 1803](get-started-with-1803.md)，**加入半年频道**。 如本主题中所述，每年你将收到两个新版本，每个版本的主流生产支持期为 18 个月。 可通过批量许可、Azure 或 Visual Studio 订阅服务获取新版本。 目前，你需要批量许可和软件保障才能在生产中运行以半年频道发布的新版本。
- 是否需要稳定性和可预测性？ 是否需要在物理服务器上运行虚拟机和传统工作负载？ 如果需要，你应该考虑**将服务器置于 Long-Term Servicing Channel**。 当前的 LTSC 版本为 [Windows Server 2016](server-basics.md)。 如本主题中所述，你有权每 2 至 3 年接收新版本，每个版本享受 5 年的主流支持和 5 年的延长支持。 LTSC 版本在所有发布机制中均有提供。 无论使用何种许可模型，所有人均可使用 LTSC 版本。 

下表汇总了从 Windows Server 版本 1803 起两个频道之间的主要区别：

|  | Long-Term Servicing Channel (Windows Server 2016) |半年频道 (Windows Server) |
| ------------------- | ------------------------------------ | ------------------------------------------------- |
|建议应用场景 | 通用文件服务器、Microsoft 和非 Microsoft 工作负载、传统应用、基础架构角色、软件定义数据中心和超级集成基础架构 | 容器化应用程序、容器主机和受益于更快创新的应用程序方案 |
| 最新发布 | 每 2 - 3 年 |每 6 个月 |
| 支持 |5 年的主流支持和 5 年的延长支持 | 18 个月 |
| 版本 | 所有可用的 Windows Server 版本 | 标准版和数据中心版 |
| 谁可以使用 | 所有频道的所有客户 | 仅软件保障客户和云客户 |
| 安装选项 | Server Core 和带桌面体验的 Server | 容器主机服务器核心、映像和 Nano Server 容器映像 |                |


## 设备兼容性
除非另行沟通，否则运行半年频道版本的最低硬件要求将与运行最新 Windows Server Long-Term Servicing Channel 版本的要求相同。 例如，**当前 Long-Term Servicing Channel 版本为 Windows Server 2016**。 大多数硬件驱动器仍可在这些版本中正常工作。

## 维护
Long-Term Servicing Channel 和半年频道两种版本都将由安全更新和非安全更新进行支持。 不同之处在于版本受支持的时间长度，如前文所述。

### 维护工具
IT 专业人员可以使用多种工具维护 Windows Server。 每个选项均有其优点和缺点，包括功能和控制、简洁性和低管理要求等。 以下是可用于管理维护更新的维护工具示例：

- **Windows 更新（独立）**：此选项仅适用于已连接到 Internet 并已启用 Windows 更新的服务器。
- **Windows Server Update Services (WSUS)** 可在大范围内控制 Windows 10 和 Windows Server 更新，并且内置在 Windows Server 操作系统中。 除了能够延迟更新，组织还可以添加用于更新的批准层，并在准备就绪后将它们部署到特定计算机或计算机组。
- **System Center Configuration Manager** 可最大程度地控制维护。 IT 专业人员可以延迟更新、批准更新，并且可以使用多种选项指向部署以及管理带宽使用情况和部署次数。

你可能已基于你的资源、员工和专业知识选择使用此类选项中的至少一项。 你可以继续将相同的流程用于半年频道版本：例如，如果你已使用 System Center Configuration Manager 管理更新，则可以继续使用。 同样，如果你正在使用 WSUS，也可以继续使用。

## 通过 Windows 预览体验计划获取预览版本
测试较早版本的 Windows Server 对 Microsoft 及其客户都有帮助，因为他们有机会在发布之前发现可能出现的问题。 此外，这还为客户提供了一个直接影响产品功能的独特机会。 

Microsoft 依赖于在开发过程中接收反馈，从而可以尽快做出调整。 提前测试和反馈对于快速发布模型至关重要。

有关如何参与 Windows 预览体验计划的详细信息，请参阅 [Windows 预览体验计划服务器版文档](https://docs.microsoft.com/windows-insider/at-work/)。
# 相关主题
[Windows Server 2019 服务频道：LTSC 和 SAC](https://docs.microsoft.com/windows-server/get-started-19/servicing-channels-19)

[Windows Server 半年频道中对 Nano Server 所做的更改](nano-in-semi-annual-channel.md)

[Windows Server 支持周期](https://support.microsoft.com/en-us/lifecycle)

[Windows Server 2016 系统要求](https://docs.microsoft.com/windows-server/get-started/system-requirements) 




