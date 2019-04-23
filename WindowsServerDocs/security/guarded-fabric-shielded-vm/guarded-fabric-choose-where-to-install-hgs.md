---
title: 选择是否在其自己的新林或现有堡垒林中安装 HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 4e02cd37391e629c9b947095fe32626bd15726ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827498"
---
# <a name="choose-whether-to-install-hgs-in-its-own-dedicated-forest-or-in-an-existing-bastion-forest"></a>选择是否在其自己的专用林或现有堡垒林中安装 HGS

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016


HGS 的 Active Directory 林是敏感的因为其管理员有权控制受防护的 Vm 的密钥。 默认安装将为安装新林专用 HGS，并配置其他依赖项。 此选项是建议，因为环境是自包含并在创建时已知安全。 

在现有林中安装 HGS 的仅技术要求是它被添加到根域中;不支持非根域。 但还有操作要求和有关使用现有林的安全性最佳做法。 特意构建合适的林提供一个敏感函数，例如由林[适用于 AD DS 的 Privileged Access Management](https://docs.microsoft.com/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services)或[增强的安全管理环境 (ESAE) 林](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access-reference-material#ESAE_BM). 此类林通常具有以下特性：

- 它们具有几个管理员 （独立于构造管理员的访问）
- 它们具有较少的登录
- 它们不是在本质上常规用途 

常规用途等生产林的林不适合由 HGS 使用。 Fabric 林也是不合适，因为 HGS 需要从构造管理员隔离开来。

## <a name="next-step"></a>下一步

选择最适合您的环境的安装选项：

- [在其自己专用的林中安装 HGS](guarded-fabric-install-hgs-default.md)
- [在现有的堡垒林中安装 HGS](guarded-fabric-install-hgs-in-a-bastion-forest.md)


