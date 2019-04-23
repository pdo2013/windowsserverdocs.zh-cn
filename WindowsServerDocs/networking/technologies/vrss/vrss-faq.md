---
title: vRSS Frequently Asked Questions
description: 在本主题中，您发现一些常见的问题和解答有关使用 vRSS。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 61ae242e-82a8-430d-b07d-52b86c01e686
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3fafe6c39285e65a9d39a76cc6b652dac5c3efbd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840238"
---
# <a name="vrss-frequently-asked-questions"></a>vRSS Frequently Asked Questions

在本主题中，您发现一些常见的问题和解答有关使用 vRSS。

## <a name="what-are-the-requirements-for-the-physical-network-adapters-that-i-use-with-vrss"></a>对于通过 vRSS 使用的物理网络适配器要求是什么？

网络适配器必须与虚拟机队列兼容\(VMQ\)并且必须具有链接速度为 10 Gbps 或更多。

有关详细信息，请参阅[规划 vRSS 使用](vrss-plan.md)。

## <a name="does-vrss-work-with-hyper-threaded-processor-cores"></a>VRSS 适用于超\-线程处理器内核？

否。 VRSS 和 VMQ 将忽略超\-线程处理器内核。

## <a name="does-vrss-work-for-host-virtual-nics-vnics"></a>VRSS 适用于主机虚拟 Nic \(Vnic\)？

是。 使用 **-ManagementOS**参数而不是虚拟机\(VM\)名称上**Set-vmnetworkadapter** Windows PowerShell 命令和**启用 NetAdapterRss**主机 vNIC 上。

有关详细信息，请参阅[Windows PowerShell 命令适用于 RSS 和 vRSS](vrss-wps.md)。

## <a name="how-many-logical-processors-does-a-vm-need-to-use-vrss"></a>VM 需要使用 vRSS 逻辑处理器数量？

Vm 需要两个或多个逻辑处理器\(LPs\)要能够使用 vRSS。

有关详细信息，请参阅[规划 vRSS 使用](vrss-plan.md)。

## <a name="is-vrss-compatible-with-nic-teaming"></a>是兼容的 NIC 组合 vRSS？

是。 如果使用 NIC 组合，务必正确配置 VMQ，从而与 NIC 组合设置一起使用。 有关 NIC 组合部署和管理的详细信息，请参阅[NIC 组合](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)。

## <a name="vrss-is-enabled-but-how-do-i-know-if-it-is-working"></a>已启用 vRSS，但如何知道它是否正常？ 

您可以告知 vRSS 工作在 VM 中打开任务管理器并查看虚拟处理器的利用率。 如果有多个连接建立到 VM，可以看到多个内核利用率超过 0%。

单个 TCP 会话不能为负载均衡分布在多个逻辑处理器核心，因为你的 VM 必须在接收多个 TCP 会话之前，您可以观察 vRSS 正常工作。

如果该 VM 正在接收多个 TCP 会话，但你看不到多个利用率超过 0%的 LP 内核，请确保你已完成所有主题中的准备步骤[规划 vRSS 使用](vrss-plan.md)。

## <a name="im-looking-at-the-host-and-not-all-of-the-processors-are-being-used-it-looks-like-every-other-one-is-being-skipped"></a>我正在查看主机，发现并非所有处理器都被使用。 看上去每隔一个处理器就会跳过一个处理器。
  
请检查是否启用了超线程。 VMQ 和 vRSS 设计用于跳过超\-线程核心。

## <a name="are-there-different-windows-powershell-commands-for-rss-and-vrss"></a>有不同的 Windows PowerShell 命令适用于 RSS 和 vRSS 吗？

是 / 否。 虽然适用于本机主机中的 RSS 和 RSS 在 Vm 中的使用的相同命令，vRSS 还要求启用和的 VM 和 vRSS 上的物理 NIC 的交换机端口上启用 VMQ。

有关详细信息，请参阅[Windows PowerShell 命令适用于 RSS 和 vRSS](vrss-wps.md)。

---
