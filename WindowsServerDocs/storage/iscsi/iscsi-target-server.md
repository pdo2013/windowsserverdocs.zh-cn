---
title: iSCSI Target Server Overview
TOCTitle: iSCSI Target Server
ms.prod: windows-server
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 638343abf782983020a3301898920470ffcd5952
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403054"
---
# <a name="iscsi-target-server-overview"></a>iSCSI 目标服务器概述

适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题简要概述了 iSCSI 目标服务器，这是 Windows Server 中的一项角色服务，可让你通过 iSCSI 协议提供存储。 这对于不能通过本机 Windows 文件共享协议 SMB 进行通信的客户端提供对 Windows server 上存储的访问很有用。

iSCSI 目标服务器适合于以下情况：

* **网络和无磁盘启动**@no__t 1By 使用支持启动的网络适配器或软件加载程序，你可以部署数百台无盘服务器。 借助 iSCSI 目标服务器，部署可快速进行。 在 Microsoft 内部测试中，256 台计算机可在 34 分钟内进行部署。 通过使用差异虚拟硬盘，可以将用于操作系统映像的存储空间节省高达 90%。 对于大规模部署完全相同的操作系统映像，如在运行 Hyper-V 的虚拟机上或高性能计算 (HPC) 群集中，这是理想之选。

* **服务器应用程序存储**   Some 应用程序需要块存储。 iSCSI 目标服务器可以为这些应用程序提供连续可用的块存储。 由于此存储可远程访问，它也可以为总部或分支机构办公室位置整合块存储。

* **异类存储**   ISCSI 目标服务器支持非 Microsoft iSCSI 发起程序，使你可以轻松地在混合软件环境中的服务器上共享存储。

* **开发、测试、演示和实验室环境**@no__t 启用了 ISCSI 目标服务器，运行 Windows Server 操作系统的计算机将成为可通过网络访问的块存储设备。 这对于在存储区域网络 (SAN) 中进行部署之前测试应用程序非常有用。

## <a name="block-storage-requirements"></a>块存储要求

使 iSCSI 目标服务器能够提供块存储，从而利用现有以太网网络。 不需要额外的硬件。 如果高可用性是一项重要标准，则考虑设置高可用性集群。 需要将共享存储用于高可用性群集（光纤通道存储的硬件或串行连接 SCSI (SAS) 存储阵列）。

如果启用来宾群集，则需要提供块存储。 运行具有 iSCSI 目标服务器的 Windows Server 软件的任何服务器都可以提供块存储。

## <a name="see-also"></a>请参阅

[iSCSI 目标块存储，如何](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh848268(v%3dws.11))  
[Windows Server 中 iSCSI 目标服务器的新增功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn305893(v%3dws.11))

