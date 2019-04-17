---
ms.assetid: 4981b32f-741e-4afc-8734-26a8533ac530
title: "集成到一个现有 DNS 基础结构的广告 DS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ab8d92a237a6d1fb623d9f4bb7dcc88561edf742
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="integrating-ad-ds-into-an-existing-dns-infrastructure"></a>集成到一个现有 DNS 基础结构的广告 DS

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

如果你的组织已经有现有域名系统 (DNS) 服务器服务，Active Directory 域服务 (广告 DS) 所有者 DNS 必须处理你的组织的 DNS 所有者以广告 DS 融入现有的基础结构。 这包括创建了 DNS 服务器和 DNS 客户端配置。  
  
## <a name="creating-a-dns-server-configuration"></a>创建了 DNS 服务器配置  
集成广告 DS 与现有 DNS 命名空间时，我们建议你以下操作：  
  
-   森林中的每个域控制器安装 DNS 服务器服务。 如果其中一个 DNS 服务器不可用，这提供故障能力。 这种方式，域控制器不必依赖于其他 DNS 服务器的名称分辨率。 这还简化了管理环境，由于所有域控制器都拥有一个统一的配置。  
  
-   配置 Active Directory 森林根域控制器主持 DNS Active Directory 林的区域。  
  
-   配置域控制器，以便每个区域地域宿主对应于它们的域的 Active Directory DNS 区域。  
  
-   配置包含 Active Directory 树林定位器记录区域 (即 _msdcs。*forestname*区域) 来通过使用树林 DNS 应用目录分区复制到每个森林中的 DNS 服务器。  
  
    > [!NOTE]  
    > 在 DNS 服务器服务已安装在使用 Active Directory 域服务安装向导 （我们建议使用此选项），自动执行所有以前的任务。 有关详细信息，请参阅[部署 Windows Server 2008 森林根域](https://technet.microsoft.com/library/cc731174.aspx)。  
  
    > [!NOTE]  
    > 广告 DS 使用树林定位器记录启用复制合作伙伴相互找到并使客户能够找到全球目录服务器。 广告 DS 存储在 _msdcs 树林定位器记录。*forestname*区域。 必须适用范围区域中的信息，因为该区域树林 DNS 应用目录分区通过复制到森林中的所有 DNS 服务器。  
  
现有的 DNS 结构保持不变。 你不需要移动任何服务器或区域。 只需从你的现有 DNS 层次到 Active Directory 集成 DNS 区域创建委派。  
  
## <a name="creating-the-dns-client-configuration"></a>创建了 DNS 客户端配置  
若要配置客户端计算机上 DNS，广告 DS 所有者 DNS 必须指定计算机命名方案，的客户端将如何查找 DNS 服务器。 下表列出了这些设计元素我们推荐的配置。  
  
|设计元素|配置|  
|------------------|-----------------|  
|计算机命名|使用默认命名。 当 Windows 2000，Windows XP 中，Windows Server 2003 时，Windows Server 2008 或 Windows Vista 基于计算机加入域，计算机分配本身完整的域名 (FQDN)，包括主计算机的名称和域的 Active Directory 的名称。|  
|客户端解析程序配置|将配置客户端计算机指向网络上的任何 DNS 服务器。|  
  
> [!NOTE]  
> Active Directory 客户端和域控制器可以会动态即使未指向 DNS 服务器其名称的授权注册其 DNS 名称。  
  
一台计算机可能会有不同的现有 DNS 名称如果组织之前，静态注册的计算机中 DNS 或组织是否之前部署集成的动态主机配置协议 (DHCP) 解决方案。 如果客户端计算机已注册的 DNS 名，在他们已加入域升级到 Windows Server 2008 广告 DS 时，它们将有两种不同的名称：  
  
-   现有 DNS 名称  
  
-   新完整的域名 (FQDN)  
  
仍可以通过任一名称位于客户端。 任何现有 DNS、 DHCP 或集成的 DNS 月 DHCP 解决方案将保持不变。 新的主名称自动创建并通过动态更新进行更新。 他们通过清理进行自动清理。  
  
如果你想要充分利用 Kerberos 身份验证，连接到运行 Windows 2000、 Windows Server 2003 或 Windows Server 2008 的服务器时，你必须确保连接到服务器的客户端使用主要的名称。  
  


