---
title: DirectAccess 不受支持的配置
description: 本主题提供 Windows Server 2016 中不受支持的 DirectAccess 配置的列表。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 23d05e61-95c3-4e70-aa83-b9a8cae92304
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5e652083d4accf90b542a16d51e314299303954f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388839"
---
# <a name="directaccess-unsupported-configurations"></a>DirectAccess 不受支持的配置

>适用于：Windows Server（半年频道）、Windows Server 2016

在开始部署之前，请查看以下不受支持的 DirectAccess 配置列表，以避免重新启动部署。  

## <a name="bkmk_frs"></a>组策略对象的文件复制服务（FRS）分发（SYSVOL 复制）  
不要在域控制器运行文件复制服务（FRS）来分发组策略对象（SYSVOL 复制）的环境中部署 DirectAccess。 如果使用 FRS，则不支持部署 DirectAccess。  
  
如果有运行 Windows Server 2003 或 Windows Server 2003 R2 的域控制器，则使用 FRS。 此外，如果你以前使用过 Windows 2000 服务器或 Windows Server 2003 域控制器，并且从未从 FRS 迁移到分布式文件系统复制（DFS-R），则可能使用 FRS。  
  
如果通过 FRS SYSVOL 复制部署 DirectAccess，则会面临意外删除 DirectAccess 组策略包含 DirectAccess 服务器和客户端配置信息的 DirectAccess。 如果删除了这些对象，则 DirectAccess 部署将会中断，并且使用 DirectAccess 的客户端计算机将无法连接到你的网络。  
  
如果你计划部署 DirectAccess，则必须使用在 Windows Server 2003 R2 之前运行操作系统的域控制器，并且你必须使用 DFS-R。  
  
有关从 FRS 迁移到 DFS 的信息，请参阅 [SYSVOL 复制迁移指南：要 DFS 复制](https://technet.microsoft.com/library/dd640019(v=ws.10).aspx)的 FRS。  
  
## <a name="bkmk_nap"></a>DirectAccess 客户端的网络访问保护  
网络访问保护（NAP）用于确定远程客户端计算机在被授予对公司网络的访问权限之前是否符合该策略。 Windows Server 2012 R2 中不推荐使用 NAP，Windows Server 2016 中未包含此项。 出于此原因，不建议启动新的 DirectAccess 部署。 建议为 DirectAccess 客户端的安全提供不同的终结点控制方法。  
  
## <a name="bkmk_multi"></a>针对 Windows 7 客户端的多站点支持  
在多站点部署中配置 DirectAccess 时，Windows 10 @ no__t-0、Windows @ no__t-1 8.1 和 Windows @ no__t-2 8 客户端能够连接到最近的站点。  Windows 7 @ no__t-0 客户端计算机没有相同的功能。 Windows 7 客户端的站点选择在策略配置时设置为特定站点，这些客户端将始终连接到指定的站点，而不考虑其位置。  
  
## <a name="bkmk_user"></a>基于用户的访问控制  
DirectAccess 策略基于计算机，而不是基于用户。 不支持指定 DirectAccess 用户策略来控制对企业网络的访问。  
  
## <a name="bkmk_policy"></a>自定义 DirectAccess 策略  
可以使用 DirectAccess 安装向导、远程访问管理控制台或远程访问 Windows PowerShell cmdlet 来配置 DirectAccess。 不支持使用 DirectAccess 安装向导之外的任何方法来配置 DirectAccess，如直接修改 DirectAccess 组策略对象或手动修改服务器或客户端上的默认策略设置。 这些修改可能会导致配置无法使用。  
  
## <a name="bkmk_kerb"></a>KerbProxy authentication  
使用入门向导配置 DirectAccess 服务器时，DirectAccess 服务器将自动配置为使用 KerbProxy authentication 进行计算机和用户身份验证。 因此，只应为仅部署了 Windows 10 @ no__t、Windows 8.1 或 Windows 8 客户端的单站点部署使用入门向导。  
  
此外，不应将以下功能与 KerbProxy authentication 一起使用：  
  
-   使用外部负载均衡器或 Windows 负载平衡负载   
    平衡  
  
-   需要智能卡或一次性密码（OTP）的双因素身份验证  
  
如果启用 KerbProxy authentication，则不支持以下部署计划：  
  
-   多.  
  
-   Windows 7 客户端的 DirectAccess 支持。  
  
-   强制隧道。 若要确保在使用强制隧道时不启用 KerbProxy authentication，请在运行向导时配置以下各项：  
  
    -   启用强制隧道  
  
    -   为 Windows 7 客户端启用 DirectAccess  
  
> [!NOTE]  
> 对于之前的部署，你应该使用高级配置向导，该向导使用包含双隧道配置的计算机和用户身份验证。 有关详细信息，请参阅[使用高级设置部署单个 DirectAccess 服务器](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。  
  
## <a name="bkmk_isa"></a>使用 ISATAP  
ISATAP 是一种在仅支持 IPv4 的公司网络中提供 IPv6 连接的转换技术。 它仅限于使用单个 DirectAccess 服务器部署的中小型组织，并允许远程管理 DirectAccess 客户端。 如果为多站点、负载平衡或多域环境 deployedin ISATAP，则必须在配置 DirectAccess 之前将其删除或移动到本机 IPv6 部署。  
  
## <a name="bkmk_iphttps"></a>IPHTTPS 和一次性密码（OTP）终结点配置  
使用 IPHTTPS 时，IPHTTPS 连接必须在 DirectAccess 服务器上终止，而不能在任何其他设备（例如负载均衡器）上终止。 同样，在一次性密码（OTP）身份验证期间创建的带外安全套接字层（SSL）连接必须在 DirectAccess 服务器上终止。 这些连接的终结点之间的所有设备必须在直通模式下进行配置。  
  
## <a name="bkmk_ft"></a>利用 OTP 身份验证强制隧道  
不要使用 OTP 和强制隧道等双重身份验证部署 DirectAccess 服务器，否则 OTP 身份验证将失败。 DirectAccess 服务器和 DirectAccess 客户端之间需要带外安全套接字层（SSL）连接。 此连接需要豁免才能在 DirectAccess 隧道外发送流量。 在强制隧道配置中，所有流量必须流经 DirectAccess 隧道，并且在建立隧道后，不允许免除。 因此，不支持在强制隧道配置中使用 OTP 身份验证。  
  
## <a name="bkmk_rodc"></a>使用只读域控制器部署 DirectAccess  
DirectAccess 服务器必须具有对读写域控制器的访问权限，并且不能在只读域控制器（RODC）中正确运行。  
  
出于许多原因，需要读写域控制器，其中包括：  
  
-   在 DirectAccess 服务器上，需要读写域控制器才能打开远程访问 Microsoft 管理控制台（MMC）。  
  
-   DirectAccess 服务器必须在 DirectAccess 客户端和 DirectAccess 服务器组策略对象（Gpo）中进行读取和写入。  
  
-   DirectAccess 服务器将从主域控制器仿真器（PDCe）中读取和写入到客户端 GPO。  
  
由于这些要求，不要使用 RODC 部署 DirectAccess。  
  


