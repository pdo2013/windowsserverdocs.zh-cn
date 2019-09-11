---
title: vRSS 常见问题
description: 在本主题中，你将了解有关使用 vRSS 的一些常见问题和解答。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 61ae242e-82a8-430d-b07d-52b86c01e686
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f0f3cb2bebf5aed31be0dda0c8e33a78aae94367
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871818"
---
# <a name="vrss-frequently-asked-questions"></a>vRSS 常见问题

在本主题中，你将了解有关使用 vRSS 的一些常见问题和解答。

## <a name="what-are-the-requirements-for-the-physical-network-adapters-that-i-use-with-vrss"></a>与 vRSS 一起使用的物理网络适配器的要求是什么？

网络适配器必须与虚拟机队列\(VMQ\)兼容，且链接速度必须为 10 Gbps 或更高。

有关详细信息，请参阅[计划使用 vRSS](vrss-plan.md)。

## <a name="does-vrss-work-with-hyper-threaded-processor-cores"></a>VRSS 是否适用于超\-线程处理器内核？

否。 VRSS 和 VMQ 都忽略超\-线程处理器核心。

## <a name="does-vrss-work-for-host-virtual-nics-vnics"></a>VRSS 是否适用于主机虚拟 nic \(vnic\)？

是。 使用 **-ManagementOS**参数而不是**VMNetworkAdapter** Windows PowerShell \(命令\)上的虚拟机 VM 名称，并在主机 vNIC 上**启用-get-netadapterrss** 。

有关详细信息，请参阅[RSS 和 vRSS 的 Windows PowerShell 命令](vrss-wps.md)。

## <a name="how-many-logical-processors-does-a-vm-need-to-use-vrss"></a>VM 需要多少个逻辑处理器才能使用 vRSS？

Vm 需要两个或更多\(逻辑\)处理器 LPs 才能使用 vRSS。

有关详细信息，请参阅[计划使用 vRSS](vrss-plan.md)。

## <a name="is-vrss-compatible-with-nic-teaming"></a>VRSS 是否与 NIC 组合兼容？

是。 如果使用的是 NIC 组合，则必须正确配置 VMQ 才能使用 NIC 组合设置。 有关 NIC 组合部署和管理的详细信息，请参阅[Nic 组合](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)。

## <a name="vrss-is-enabled-but-how-do-i-know-if-it-is-working"></a>已启用 vRSS，但我如何知道它是否正常工作？ 

通过在 VM 中打开任务管理器并查看虚拟处理器使用率，你将能够告诉 vRSS 正在运行。 如果虚拟机建立了多个连接，则可以看到多个核心的利用率超过 0%。

由于单个 TCP 会话不能跨多个逻辑处理器核心进行负载均衡，因此，你的 VM 必须接收多个 TCP 会话，然后才能观察 vRSS 是否正常工作。

如果 VM 正在接收多个 TCP 会话，但你看不到超过 0% 使用率的多个 LP 内核，请确保已完成[规划 VRSS 使用](vrss-plan.md)主题中的所有准备步骤。

## <a name="im-looking-at-the-host-and-not-all-of-the-processors-are-being-used-it-looks-like-every-other-one-is-being-skipped"></a>我正在查看主机，而不是所有处理器都被使用。 看上去每隔一个处理器就会跳过一个处理器。
  
请检查是否启用了超线程。 VMQ 和 vRSS 都设计为跳过超\-线程核心。

## <a name="are-there-different-windows-powershell-commands-for-rss-and-vrss"></a>RSS 和 vRSS 是否有不同的 Windows PowerShell 命令？

是和否。 虽然在本机主机和 Vm 中的 RSS 中同时使用相同的命令，但 vRSS 还需要在物理 NIC 上启用 VMQ，并在交换机端口上启用 VM 和 vRSS。

有关详细信息，请参阅[RSS 和 vRSS 的 Windows PowerShell 命令](vrss-wps.md)。

---
