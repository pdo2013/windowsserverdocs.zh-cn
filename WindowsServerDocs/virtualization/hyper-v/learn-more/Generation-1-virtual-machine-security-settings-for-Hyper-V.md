---
title: 第 1 代虚拟机的 HYPER-V 安全设置
description: 介绍了适用于第 1 代虚拟机中的 HYPER-V 管理器的安全设置
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f8f8c569-8b74-4c19-876e-1c7d00cce308
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 923216142a45071bc3623e3f37b37cc6a2361f26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812138"
---
# <a name="generation-1-virtual-machine-security-settings"></a>第 1 代虚拟机安全设置

>适用于：Windows Server 2016 中，Microsoft 的 HYPER-V Server 2016 中，Windows Server 2019，Microsoft HYPER-V Server 2019

使用在 Hyper-v 管理器中的第 1 代虚拟机安全设置来帮助保护数据和虚拟机的状态。

## <a name="encryption-support-settings-in-hyper-v-manager"></a>在 Hyper-v 管理器中的加密支持设置

您可以帮助保护数据和虚拟机的状态通过选择以下加密的支持选项。

- **加密状态和虚拟机迁移流量**-时写入到磁盘和实时迁移通信加密的虚拟机已保存状态。

若要启用此选项，必须添加虚拟机的密钥存储驱动器。

## <a name="key-storage-drive-in-hyper-v-manager"></a>在 Hyper-v 管理器中的密钥存储驱动器

密钥存储驱动器提供到 BitLocker 密钥来存储虚拟机的小型驱动器。 这样，虚拟机加密其操作系统磁盘，而不需要虚拟化的受信任的平台模块 (TPM) 芯片。 通过使用密钥保护程序进行加密密钥存储驱动器的内容。 要运行该虚拟机的密钥保护程序 authories 的 HYPER-V 主机。 这两个密钥存储驱动器和密钥保护程序的内容作为虚拟机的运行时状态的一部分存储。

若要解密的密钥存储驱动器的内容和启动虚拟机，需要为 HYPER-V 主机：

- 此虚拟机，授权受保护的构造的一部分或
- 从一个虚拟机的 guardians 具有私钥。

若要了解有关受保护的构造的详细信息，请参阅中的引入了受防护的 Vm 部分[安全和保障](../../../security/Security-and-Assurance.md)。

可以将密钥存储驱动器添加到其中一个虚拟机的 IDE 控制器上的空槽。 若要执行此操作，请单击**将密钥存储驱动器添加**的密钥存储驱动器添加到此虚拟机的第一个可用 IDE 控制器槽。

##<a name="see-also"></a>请参阅

- [第 2 代虚拟机的 HYPER-V 管理器中的安全设置](Generation-2-virtual-machine-security-settings-for-hyper-v.md)
- [安全和保障](../../../security/Security-and-Assurance.md)