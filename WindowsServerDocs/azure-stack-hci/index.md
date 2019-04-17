---
title: Azure 堆栈 HCI 概述
description: Azure 堆栈 HCI 是超聚合使用的 Windows Server 2019 群集验证硬件运行虚拟化工作负载的本地 （可选） 连接到 Azure 服务的基于云的备份、 站点恢复和详细信息。 Azure 堆栈 HCI 解决方案使用 Microsoft 验证硬件，以确保获得最佳性能和可靠性，并包括对诸如 NVMe 驱动器，持久性内存和远程直接内存访问 (RDMA) 网络技术的支持。
ms.technology: storage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 03/26/2019
---

# Azure 堆栈 HCI 概述

>适用于： Windows Server 2019

Azure 堆栈 HCI 是超聚合使用的 Windows Server 2019 群集验证硬件运行虚拟化工作负载的本地 （可选） 连接到 Azure 服务的基于云的备份、 站点恢复和详细信息。 Azure 堆栈 HCI 解决方案使用 Microsoft 验证硬件，以确保获得最佳性能和可靠性，并包括对诸如 NVMe 驱动器，持久性内存和远程直接内存访问 (RDMA) 网络技术的支持。

## Azure Stack 系列

Azure 堆栈 HCI 是 Azure 的一部分，Azure Stack 系列，使用相同的软件定义计算、 存储和网络的软件，即 Azure Stack。 下面是不同的解决方案的核心摘要：

- [Azure](https://azure.microsoft.com) -使用公共云服务
- [Azure Stack](https://azure.microsoft.com/overview/azure-stack) -运行云服务的本地
- [Azure 堆栈 HCI](https://azure.microsoft.com/overview/azure-stack/hci) -运行虚拟化应用的本地，与可选连接到 Azure

![Azure 和 Azure Stack Azure 堆栈 HCI 运行时运行的云服务，虚拟化应用程序本地](media/azure-and-azure-stack-family.png)

若要了解详细信息：

- 在 2019 年 3 月 28 日上注册我们[混合云虚拟事件](https://info.microsoft.com/ww-landing-building-a-successful-hybrid-cloud-strategy.html)。
- 了解更多信息，请访问我们的[Azure 堆栈 HCI](https://azure.microsoft.com/overview/azure-stack/hci)解决方案网站。
- 观看 Microsoft 专家 Jeff Woolsey 和 Vijay Tewari[讨论新的 Azure 堆栈 HCI 解决方案](https://aka.ms/AzureStackOverviewVideo)。

## 硬件合作伙伴

你可以购买验证 15 合作伙伴从运行 Windows Server 2019 的 Azure 堆栈 HCI 解决方案。 首选的 Microsoft 合作伙伴获取你设置和运行不带冗长设计和生成时间，并提供单一实现和支持服务的联系人。

访问[Azure 堆栈 HCI 网站](https://azure.microsoft.com/overview/azure-stack/hci)以查看我们 70 + Azure 堆栈 HCI 的解决方案当前可从这些 Microsoft 合作伙伴： ASUS，Axellio，蓝筹股 DataON、 Dell EMC、 Fujitsu、 HPE、 Hitachi、 华、 Lenovo、 NEC、 primeLine 解决方案、 QCT、 SecureGUARD和 Supermicro。

## 常见问题解答

### Azure Stack 和 Azure 堆栈 HCI 解决方案有何共同点？ 
Azure 堆栈 HCI 解决方案具有相同的基于 HYPER-V 软件定义计算、 存储和网络技术功能作为 Azure Stack。 这两个产品/服务满足严格的测试和验证标准，以确保可靠性和兼容性的基础硬件平台。

### 如何在不同？
使用 Azure Stack 运行服务本地云。 你可以运行 Azure IaaS 和 PaaS 服务在本地完全一致地生成并运行云应用程序指向任何位置，通过 Azure 门户本地管理。

使用 Azure 堆栈 HCI，在运行虚拟化工作负载的本地管理 Windows Admin Center 和熟悉的 Windows Server 工具。 （可选） 可以为混合方案，例如基于云的站点恢复、 监视和其他用户连接到 Azure。

### Microsoft 为何推向向 Azure Stack 系列提供其 HCI？ 
Microsoft 的超聚合技术已经是 Azure 堆栈的基础。 

许多 Microsoft 客户具有复杂的 IT 环境，并且我们的目标是提供解决方案，以满足其所在的适当的业务的正确技术与需要。 Azure 堆栈 HCI 是之前可从硬件合作伙伴的基于 Windows Server 2016 的 windows server 软件定义 (WSSD) 解决方案。 我们引入其 Azure Stack 系列因为我们已开始提供新选项，可以无缝地使用 Azure 连接的基础架构管理服务。 

### 我是否能够从 Azure 堆栈 HCI 升级到 Azure 堆栈？ 
否，但客户可以其工作负载从迁移 Azure 堆栈 HCI 到 Azure 堆栈或 Azure。

### 可以连接到 Azure 堆栈 HCI 哪些 Azure 服务？

你可以连接到 Azure 堆栈 HCI 的 Azure 服务的更新列表，请参阅[连接到 Azure 混合服务的 Windows Server](../azure-hybrid-services/index.md)。

### 如何购买 Azure 堆栈 HCI 解决方案？
请按照下列步骤进行操作：

1. 从你的首选的硬件合作伙伴购买 Microsoft 验证硬件系统。
1. 安装 Windows Server 2019 数据中心版和 Windows Admin Center 进行管理和连接到 Azure 云服务的能力
1. （可选） 使用你的 Azure 帐户将基于云的管理和安全服务附加到你的工作负载。