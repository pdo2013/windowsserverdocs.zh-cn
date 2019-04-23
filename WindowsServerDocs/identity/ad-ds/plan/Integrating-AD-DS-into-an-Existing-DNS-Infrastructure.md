---
ms.assetid: 4981b32f-741e-4afc-8734-26a8533ac530
title: 将 AD DS 集成到现有的 DNS 基础结构
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 62405ea9ee38bb3fa457b7731e26fbffb2594797
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891038"
---
# <a name="integrating-ad-ds-into-an-existing-dns-infrastructure"></a>将 AD DS 集成到现有的 DNS 基础结构

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

如果你的组织已有现有的域名系统 (DNS) 服务器服务，DNS 的 Active Directory 域服务 (AD DS) 所有者必须使用你的组织的 DNS 所有者才能将 AD DS 集成到现有基础结构。 这涉及到创建的 DNS 服务器和 DNS 客户端配置。  
  
## <a name="creating-a-dns-server-configuration"></a>创建 DNS 服务器配置  
当将 AD DS 集成使用现有的 DNS 命名空间，我们建议您以下：  
  
-   在林中每个域控制器上安装 DNS 服务器服务。 如果其中一台 DNS 服务器不可用，这提供了容错能力。 这样一来，域控制器不必依赖于其他 DNS 服务器进行名称解析。 这还可简化管理环境，因为所有域控制器都具有统一的配置。  
  
-   配置 Active Directory 林根级域控制器承载 Active Directory 林的 DNS 区域。  
  
-   配置每个区域的域以承载对应于其 Active Directory 域的 DNS 区域的域控制器。  
  
-   配置包含 Active Directory 全林性定位器记录的区域 (即，_msdcs。*林*区域) 将通过使用全林性 DNS 应用程序目录分区复制到林中的每个 DNS 服务器。  
  
    > [!NOTE]  
    > 使用 Active Directory 域服务安装向导 （我们建议使用此选项） 安装 DNS 服务器服务时，会自动执行所有前面的任务。 有关详细信息，请参阅[部署 Windows Server 2008 目录林根域](https://technet.microsoft.com/library/cc731174.aspx)。  
  
    > [!NOTE]  
    > AD DS 使用全林性定位器记录来启用复制伙伴相互进行查找，并使客户端能够找到全局编录服务器。 AD DS 将全林性定位器记录存储在 _msdcs.&lt。*林*区域。 由于该区域中的信息必须广泛可用，此区域通过全林性 DNS 应用程序目录分区复制到林中的所有 DNS 服务器。  
  
现有的 DNS 结构保持不变。 不需要移动任何服务器或区域。 只需从现有的 DNS 层次结构到 Active Directory 集成的 DNS 区域创建委派。  
  
## <a name="creating-the-dns-client-configuration"></a>创建 DNS 客户端配置  
若要配置 DNS 客户端计算机上，AD DS 所有者的 DNS 必须指定命名方案和客户端将如何查找 DNS 服务器的计算机。 下表列出了这些设计元素我们建议的配置。  
  
|设计元素|配置|  
|------------------|-----------------|  
|计算机命名|使用默认命名方案。 当 Windows 2000，Windows XP、 Windows Server 2003 时，Windows Server 2008 中或基于 Windows Vista 的计算机加入域，计算机为自己分配完全限定的域名 (FQDN) 构成的计算机的主机名和活动的名称Directory 域。|  
|客户端解析程序配置|将客户端计算机指向网络上的任何 DNS 服务器配置。|  
  
> [!NOTE]  
> Active Directory 客户端和域控制器可以会动态即使它们不指向用作其名称具有权威的 DNS 服务器注册其 DNS 名称。  
  
如果组织以前，静态注册计算机在 DNS 或如果组织以前部署的集成的动态主机配置协议 (DHCP) 解决方案中，计算机可能具有不同的现有 DNS 名称。 如果客户端计算机已注册的 DNS 名称，它们已加入的域升级到 Windows Server 2008 AD DS 时，它们将具有两个不同的名称：  
  
-   现有的 DNS 名称  
  
-   新的完全限定的域名 (FQDN)  
  
仍可以通过其中任一名称位于客户端。 任何现有的 DNS、 DHCP 或 DNS/DHCP 集成的解决方案，将保持不变。 新的主名称自动创建和更新通过动态更新。 它们通过清理的时间进行自动清理。  
  
如果你想要充分利用 Kerberos 身份验证连接到运行 Windows 2000，Windows Server 2003 或 Windows Server 2008 的服务器时，必须确保客户端使用的主名称连接到服务器。  
  


