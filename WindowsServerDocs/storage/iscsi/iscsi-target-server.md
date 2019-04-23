---
title: iSCSI Target Server Overview
TOCTitle: iSCSI Target Server
ms.prod: windows-server-threshold
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 14dd373b71c1be2f1a4ff157b9c631530532fc80
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837938"
---
# <a name="iscsi-target-server-overview"></a>iSCSI 目标服务器概述

适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题提供概述的 iSCSI 目标服务器，使您能够使存储可通过 iSCSI 协议的 Windows Server 中的角色服务。 这是用于不能通过本机 Windows 文件共享协议，SMB 通信的客户端的 Windows 服务器上提供对存储的访问。

iSCSI 目标服务器适合于以下情况：

* **网络和无磁盘启动**   通过使用支持启动的网络适配器或软件加载程序，你可以部署上百个无盘服务器。 借助 iSCSI 目标服务器，部署可快速进行。 在 Microsoft 内部测试中，256 台计算机可在 34 分钟内进行部署。 通过使用差异虚拟硬盘，可以将用于操作系统映像的存储空间节省高达 90%。 对于大规模部署完全相同的操作系统映像，如在运行 Hyper-V 的虚拟机上或高性能计算 (HPC) 群集中，这是理想之选。

* **服务器应用程序存储**   某些应用程序需要块存储。 iSCSI 目标服务器可以为这些应用程序提供连续可用的块存储。 由于此存储可远程访问，它也可以为总部或分支机构办公室位置整合块存储。

* **异类存储**   iSCSI 目标服务器支持非 Microsoft iSCSI 发起程序，从而轻松地在混合的软件环境中的服务器上共享存储。

* **开发、 测试、 演示和实验室环境**   启用 iSCSI 目标服务器后，运行 Windows Server 操作系统的计算机将成为网络可访问的块存储设备。 这对于在存储区域网络 (SAN) 中进行部署之前测试应用程序非常有用。

## <a name="block-storage-requirements"></a>块存储要求

使 iSCSI 目标服务器能够提供块存储，从而利用现有以太网网络。 不需要额外的硬件。 如果高可用性是一项重要标准，则考虑设置高可用性集群。 需要将共享存储用于高可用性群集（光纤通道存储的硬件或串行连接 SCSI (SAS) 存储阵列）。

如果启用来宾群集，则需要提供块存储。 运行具有 iSCSI 目标服务器的 Windows Server 软件的任何服务器都可以提供块存储。

## <a name="see-also"></a>请参阅

[iSCSI 目标块存储方法](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh848268(v%3dws.11))  
[What's New Windows Server 中 iSCSI 目标服务器中](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn305893(v%3dws.11))

