---
title: vRSS 常见问题
description: 在本主题中，你会发现一些常见的问题和回答有关使用 vRSS。
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
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133813"
---
# vRSS 常见问题

在本主题中，你会发现一些常见的问题和回答有关使用 vRSS。

## 使用了 vRSS 物理网络适配器的要求有哪些？

网络适配器必须与虚拟机队列 \(VMQ\) 兼容，并且必须具有 10 Gbps 的链接速度或的详细信息。

有关详细信息，请参阅[计划 vRSS 的使用](vrss-plan.md)。

## 不会显示线程处理器核心 vRSS 用于？

不需要。 VRSS 和 VMQ 忽略会显示线程处理器核心。

## VRSS 是否适合主机虚拟 Nic \(vNICs\)？

是。 使用 **-ManagementOS**参数而不是**设置 VMNetworkAdapter** Windows PowerShell 命令上的虚拟机 \(VM\) 名称和**启用 NetAdapterRss**主机 vNIC 上。

有关详细信息，请参阅[RSS 和 vRSS 的 Windows PowerShell 命令](vrss-wps.md)。

## VM 需要使用 vRSS 多少逻辑处理器？

虚拟机需要两个或多个逻辑处理器 \(LPs\) 能够使用 vRSS。

有关详细信息，请参阅[计划 vRSS 的使用](vrss-plan.md)。

## 是 vRSS 与 NIC 组合兼容？

是。 如果你使用 NIC 组合，务必正确配置 VMQ 要使用的 NIC 组合的设置。 有关 NIC 组合部署和管理的详细信息，请参阅[NIC 组合](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)。

## vRSS 处于启用状态，但如何知道是否它工作？ 

你将能够告知 vRSS 正在通过在你的虚拟机中打开任务管理器并查看虚拟处理器使用率。 如果有多个连接建立到 VM，你可以看到多个核心利用率为 0%以上。

在单个 TCP 会话不能为负载平衡跨多个逻辑处理器内核，因为你的虚拟机必须会收到多个 TCP 会话之前可以观察 vRSS 正常工作。

如果 VM 收到多个 TCP 会话，但你不会看到多个利用率为 0%以上的 LP 核心，请确保已完成所有[计划的 vRSS 使用](vrss-plan.md)主题中的准备步骤。

## 我查看主机并不是所有的处理器正在使用。 它看起来每个另一个将被跳过。
  
检查以查看是否已启用超线程。 VMQ 和 vRSS 旨在跳过会显示线程的核心。

## 是否有不同的 Windows PowerShell 命令 RSS 和 vRSS？

是和否。 虽然在本机主机中的 RSS 和 RSS 在 Vm 中的使用相同的命令，vRSS 还需要 VMQ 启用物理 NIC-上并针对虚拟机和 vRSS 交换机端口上启用。

有关详细信息，请参阅[RSS 和 vRSS 的 Windows PowerShell 命令](vrss-wps.md)。

---
