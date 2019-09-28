---
title: Hyper-v 的第1代虚拟机安全设置
description: 介绍适用于第1代虚拟机的 Hyper-v 管理器中可用的安全设置
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f8f8c569-8b74-4c19-876e-1c7d00cce308
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: ceb3c2628546815f9b0af35946e173f4276130d2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392787"
---
# <a name="generation-1-virtual-machine-security-settings"></a>第1代虚拟机安全设置

>适用于：Windows Server 2016，Microsoft Hyper-V Server 2016，Windows Server 2019，Microsoft Hyper-V 服务器2019

使用 Hyper-v 管理器中的第1代虚拟机安全设置来帮助保护虚拟机的数据和状态。

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Hyper-v 管理器中的加密支持设置

可以通过选择以下加密支持选项来帮助保护虚拟机的数据和状态。

- **加密状态和虚拟机迁移流量**-将虚拟机写入磁盘和实时迁移流量时，对其进行加密。

若要启用此选项，必须为虚拟机添加密钥存储驱动器。

## <a name="key-storage-drive-in-hyper-v-manager"></a>Hyper-v 管理器中的密钥存储驱动器

密钥存储驱动器为虚拟机提供了一个用于存储 BitLocker 密钥的小型驱动器。 这允许虚拟机加密其操作系统磁盘，无需虚拟化受信任的平台模块（TPM）芯片。 密钥存储驱动器的内容使用密钥保护程序进行加密。 用于运行虚拟机的 Hyper-v 主机 authories 的密钥保护程序。 密钥存储驱动器和密钥保护程序的内容都作为虚拟机的运行时状态的一部分进行存储。

若要解密密钥存储驱动器的内容并启动虚拟机，Hyper-v 主机必须是以下项之一：

- 此虚拟机的授权受保护构造的一部分，或
- 有一个虚拟机的保护者的私钥。

若要了解有关受保护的构造的详细信息，请参阅[安全和保证](../../../security/Security-and-Assurance.md)中的 "防护的 vm 简介" 部分。

你可以将密钥存储驱动器添加到虚拟机的某个 IDE 控制器上的空插槽中。 为此，请单击 "**添加密钥存储驱动器**"，将密钥存储驱动器添加到此虚拟机的第一个可用 IDE 控制器插槽中。

## <a name="see-also"></a>请参阅

- [Hyper-v 管理器中的第2代虚拟机安全设置](Generation-2-virtual-machine-security-settings-for-hyper-v.md)
- [安全和保障](../../../security/Security-and-Assurance.md)