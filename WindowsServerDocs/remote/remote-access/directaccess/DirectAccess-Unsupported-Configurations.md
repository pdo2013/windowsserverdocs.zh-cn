---
title: DirectAccess 不受支持的配置
description: 本主题提供了一系列不支持 Windows Server 2016 中的 DirectAccess 配置。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-da
ms.topic: article
ms.assetid: 23d05e61-95c3-4e70-aa83-b9a8cae92304
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 49fa86883e6a590ac8f7dcf0c724f87af419f88e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828848"
---
# <a name="directaccess-unsupported-configurations"></a>DirectAccess 不受支持的配置

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在开始部署，以避免不得不开始重新部署之前，请查看以下列表的 DirectAccess 配置不受支持。  

## <a name="bkmk_frs"></a>文件复制服务 (FRS) 分发的组策略对象 （SYSVOL 复制）  
不要在域控制器正在分发组策略对象 （SYSVOL 复制） 的文件复制服务 (FRS) 的环境中部署 DirectAccess。 使用 FRS.时不支持 DirectAccess 的部署  
  
如果您具有运行 Windows Server 2003 或 Windows Server 2003 R2 的域控制器，使用 FRS。 此外，你可能会使用 FRS，如果你以前使用 Windows 2000 Server 或 Windows Server 2003 域控制器，并且您永远不会迁移 SYSVOL 复制从 FRS 到分布式文件系统复制 (DFS-R)。  
  
如果使用 FRS SYSVOL 复制部署 DirectAccess，风险意外删除 DirectAccess 组策略对象包含的 DirectAccess 服务器和客户端配置信息。 如果这些对象将被删除，你在 DirectAccess 部署将发生服务中断，且使用 DirectAccess 客户端计算机将无法再连接到网络。  
  
如果你打算部署 DirectAccess，必须使用 Windows Server 2003 R2，比更高版本运行操作系统的域控制器，必须使用 DFS-R。  
  
从 FRS 迁移到 DFS-R 的信息，请参阅[SYSVOL 复制迁移指南：FRS 到 DFS 复制](https://technet.microsoft.com/library/dd640019(v=ws.10).aspx)。  
  
## <a name="bkmk_nap"></a>DirectAccess 客户端的的网络访问保护  
网络访问保护 (NAP) 用于确定之前向他们授予对公司网络的访问，远程客户端计算机是否符合 IT 策略。 NAP 在 Windows Server 2012 R2 中已弃用，并且不包括 Windows Server 2016 中。 出于此原因，不建议从 NAP 的 DirectAccess 的新部署。 建议为 DirectAccess 客户端的安全终结点控件的不同方法。  
  
## <a name="bkmk_multi"></a>Windows 7 客户端的的多站点支持  
当在多站点部署中，Windows 10 配置 DirectAccess&reg;，Windows&reg; 8.1 和 Windows&reg; 8 客户端具有连接到最近的站点的功能。  Windows 7&reg;客户端计算机不具有相同的功能。 Windows 7 客户端的站点选择在策略配置的时间设置为特定站点，这些客户端将始终连接到指定站点，而不考虑其位置。  
  
## <a name="bkmk_user"></a>基于用户的访问控制  
DirectAccess 策略是基于，不基于用户的计算机。 不支持指定 DirectAccess 用户策略来控制对公司网络访问。  
  
## <a name="bkmk_policy"></a>自定义 DirectAccess 策略  
可以使用 DirectAccess 安装向导、 远程访问管理控制台中或远程访问 Windows PowerShell cmdlet 配置 DirectAccess。 不支持使用 DirectAccess 安装向导以外的任何方式来配置 DirectAccess，例如直接修改 DirectAccess 组策略对象或手动修改服务器或客户端上的默认策略设置。 这些修改可能会导致不可用的配置。  
  
## <a name="bkmk_kerb"></a>KerbProxy 身份验证  
当使用开始向导配置 DirectAccess 服务器时，DirectAccess 服务器自动配置为使用 KerbProxy 身份验证的计算机和用户身份验证。 因此，您应仅使用开始向导对于单站点部署在仅 Windows 10&reg;，部署了 Windows 8.1 或 Windows 8 客户端。  
  
此外，不应与 KerbProxy 身份验证使用以下功能：  
  
-   负载均衡通过使用外部负载均衡器或 Windows 加载   
    均衡器  
  
-   智能卡或一次性密码 (OTP) 所需的双因素身份验证  
  
如果启用 KerbProxy 身份验证，则不支持以下部署计划：  
  
-   多站点。  
  
-   DirectAccess 支持 Windows 7 客户端。  
  
-   强制隧道。 若要确保使用强制隧道时未启用 KerbProxy 身份验证，请在运行向导时配置以下各项：  
  
    -   启用强制隧道  
  
    -   使 Windows 7 的 DirectAccess 客户端  
  
> [!NOTE]  
> 对于以前的部署，应使用高级配置向导，它使用两个隧道配置基于证书的计算机和用户身份验证。 有关详细信息，请参阅[部署单个 DirectAccess 服务器使用高级设置](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。  
  
## <a name="bkmk_isa"></a>使用 ISATAP  
ISATAP 是一种转换技术，它提供在仅使用 IPv4 的公司网络的 IPv6 连接。 它被限制为小型和中等规模组织的单个 DirectAccess 服务器部署，并允许远程管理 DirectAccess 客户端。 如果 ISATAP deployedin 多站点、 负载平衡或多域环境，必须将其删除，或将其移到本机 IPv6 部署在配置 DirectAccess 之前。  
  
## <a name="bkmk_iphttps"></a>IPHTTPS 和一次性密码 (OTP) 终结点配置  
当使用 IPHTTPS 时，IPHTTPS 连接必须在 DirectAccess 服务器上，不在任何其他设备，例如负载均衡器上终止。 同样，必须在 DirectAccess 服务器上终止一次性密码 (OTP) 身份验证期间创建的带外安全套接字层 (SSL) 连接。 必须在直通模式下配置的这些连接终结点之间的所有设备。  
  
## <a name="bkmk_ft"></a>带有 OTP 身份验证的强制隧道  
不要部署具有使用 OTP 和强制隧道，双因素身份验证的 DirectAccess 服务器或 OTP 身份验证将失败。 需要 DirectAccess 服务器和 DirectAccess 客户端之间的带安全套接字层 (SSL) 连接。 此连接需要发送之外的 DirectAccess 隧道流量的免除条目。 在强制隧道配置中，所有流量必须都流经 DirectAccess 隧道，并建立隧道后，允许无例外。 因此，它不是支持强制隧道配置中具有 OTP 身份验证。  
  
## <a name="bkmk_rodc"></a>部署 DirectAccess 与只读域控制器  
DirectAccess 服务器必须有权访问读写域控制器，和不正常使用只读域控制器 (RODC)。  
  
读写域控制器是必需的很多原因，包括以下：  
  
-   DirectAccess 服务器上的读写域控制器需要打开远程访问 Microsoft 管理控制台 (MMC)。  
  
-   DirectAccess 服务器必须读取和写入的 DirectAccess 客户端和 DirectAccess 服务器组策略对象 (Gpo)。  
  
-   DirectAccess 服务器中读取和写入到客户端 GPO 专门主域控制器模拟器 (PDCe)。  
  
由于这些要求，请不要部署与 RODC 的 DirectAccess。  
  


