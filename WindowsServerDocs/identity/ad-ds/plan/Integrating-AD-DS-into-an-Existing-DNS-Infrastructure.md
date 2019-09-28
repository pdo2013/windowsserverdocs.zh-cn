---
ms.assetid: 4981b32f-741e-4afc-8734-26a8533ac530
title: 将 AD DS 集成到现有的 DNS 基础结构
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f4bb480be4696f15f0a63c20ab47042264584d2c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402561"
---
# <a name="integrating-ad-ds-into-an-existing-dns-infrastructure"></a>将 AD DS 集成到现有的 DNS 基础结构

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果你的组织已经具有现有的域名系统（DNS）服务器服务，则 Active Directory 域服务（AD DS）所有者的 DNS 必须与你的组织的 DNS 所有者合作，以便将 AD DS 集成到现有基础结构中。 这涉及到创建 DNS 服务器和 DNS 客户端配置。  
  
## <a name="creating-a-dns-server-configuration"></a>创建 DNS 服务器配置  
将 AD DS 与现有 DNS 命名空间集成时，建议执行以下操作：  
  
-   在林中的每个域控制器上安装 DNS 服务器服务。 如果其中一个 DNS 服务器不可用，这将提供容错能力。 这样，域控制器便无需依赖其他 DNS 服务器进行名称解析。 这也简化了管理环境，因为所有域控制器都有统一的配置。  
  
-   将 Active Directory 目录林根级域控制器配置为承载 Active Directory 林的 DNS 区域。  
  
-   配置每个地区性域的域控制器，以便托管与其 Active Directory 域相对应的 DNS 区域。  
  
-   配置包含 Active Directory 林范围定位器记录（即 _msdcs）的区域。*林名称*zone），以使用林范围的 DNS 应用程序目录分区复制到林中的每个 dns 服务器。  
  
    > [!NOTE]  
    > 在 Active Directory 域服务安装向导安装 DNS 服务器服务时（建议选择此选项），将自动执行前面的所有任务。 有关详细信息，请参阅[部署 Windows Server 2008 林根级域](https://technet.microsoft.com/library/cc731174.aspx)。  
  
    > [!NOTE]  
    > AD DS 使用全林性定位器记录，使复制伙伴能够彼此查找，并使客户端能够查找全局编录服务器。 AD DS 在 _msdcs 中存储全林性定位器记录。*林名称*区域。 由于区域中的信息必须广泛使用，此区域将通过林范围的 DNS 应用程序目录分区复制到林中的所有 DNS 服务器。  
  
现有的 DNS 结构保持不变。 不需要移动任何服务器或区域。 只需从现有 DNS 层次结构创建到 Active Directory 集成的 DNS 区域的委托。  
  
## <a name="creating-the-dns-client-configuration"></a>创建 DNS 客户端配置  
若要在客户端计算机上配置 DNS，AD DS 所有者的 DNS 必须指定计算机命名方案以及客户端将如何查找 DNS 服务器。 下表列出了这些设计元素的建议配置。  
  
|Design 元素|配置|  
|------------------|-----------------|  
|计算机命名|使用默认命名。 当 Windows 2000、Windows XP、Windows Server 2003、Windows Server 2008 或基于 Windows Vista 的计算机加入域时，计算机会将一个完全限定的域名（FQDN）分配给一个完全限定的域名（FQDN），该域名包含计算机的主机名和活动的名称。Directory 域。|  
|客户端解析程序配置|将客户端计算机配置为指向网络上的任何 DNS 服务器。|  
  
> [!NOTE]  
> Active Directory 客户端和域控制器可以动态地注册其 DNS 名称，即使它们不是指向其名称的权威 DNS 服务器。  
  
如果组织以前在 DNS 中静态注册了计算机，或者组织以前部署了集成的动态主机配置协议（DHCP）解决方案，则计算机可能具有不同的现有 DNS 名称。 如果客户端计算机已具有已注册的 DNS 名称，则当它们加入的域升级到 Windows Server 2008 AD DS 时，它们将具有两个不同的名称：  
  
-   现有 DNS 名称  
  
-   新的完全限定的域名（FQDN）  
  
仍可通过任一名称找到客户端。 任何现有的 DNS、DHCP 或集成的 DNS/DHCP 解决方案都保持不变。 将自动创建新的主名称并通过动态更新方式更新。 它们通过清理自动清除。  
  
如果要在连接到运行 Windows 2000、Windows Server 2003 或 Windows Server 2008 的服务器时利用 Kerberos 身份验证，则必须确保客户端使用主名称连接到服务器。  
  


