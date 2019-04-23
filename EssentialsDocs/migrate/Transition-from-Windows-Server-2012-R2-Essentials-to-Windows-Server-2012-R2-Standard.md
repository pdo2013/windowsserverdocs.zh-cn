---
title: 从 Windows Server Essentials 转换到 Windows Server 2012 R2 Standard
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a14689e3-2310-4229-bd3e-dafc0e739e02
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d371e24b17310c0687666185f56fe07a135ff91f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840078"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-r2-standard"></a>从 Windows Server Essentials 转换到 Windows Server 2012 R2 Standard

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

Windows Server 2016 是云就绪操作系统同时引入了新的技术，可以轻松快速地转换到云计算支持当前的工作负荷。 Windows Server 2016 内容可帮助做好准备。

 Windows Server Essentials 支持多达 25 名用户和 50 台设备。 当你的业务需求超过限制时，可以为 Windows Server 2012 R2 Standard 保持许可证的合规性的就地许可证过渡执行从 Windows Server Essentials。  
  
 转换到 Windows Server 2012 R2 Standard 后，用户帐户和设备限制都会消除，但所独有 （如仪表板、 远程 Web 访问和客户端计算机备份） 的 Windows Server Essentials 的功能仍然可用。 但是，由于这些功能在技术上的限制，最多只能支持 100 个用户帐户和 200 台设备。 客户端计算机备份功能将支持最多 75 台设备的备份。  
  
> [!IMPORTANT]
>   Windows Server 2012 R2 Standard 需要为每个用户或您的环境中的设备客户端访问许可证 (CAL)。 这是不同于 Windows Server Essentials，不使用 CAL 模式，也不包含任何 Cal。 在从 Windows Server Essentials 转换到 Windows Server 2012 R2 Standard 时，将需要购买相应数量和类型的 Cal （大多数客户购买的用户 Cal） 环境。  
  
## <a name="before-the-transition"></a>转换前  
  
-   从 Windows Server Essentials 过渡到 Windows Server 2012 R2 Standard 之前，应当完整备份服务器数据。  
  
    > [!IMPORTANT]
    >  如果不完整备份服务器数据，则无法将服务器还原到转换前的状态。  
  
-   此外，请确保您阅读并了解最终用户许可协议 (EULA) 的 Windows Server 2012 R2 Standard。 查看 EULA 的步骤：  
  
    1.  以管理员身份打开命令窗口。  
  
    2.  运行下面的命令：  
  
         **dism /online-/set-edition: ServerStandard /geteula:** *eula 路径*(其中*eula 路径*表示的位置你想要保存 EULA 文件; 例如：C:\ws8std_eula.rtf）。 请务必使用 .rtf 作为文件后缀名。  
  
    3.  打开保存该文件的位置，然后双击打开该文件。  
  
## <a name="transition-to--windows-server-2012-r2-standard"></a>转换到 Windows Server 2012 R2 Standard  
 之后您已决定转换，以便从 Windows Server Essentials 为 Windows Server 2012 R2 Standard，完成这两个步骤：  
  
1.  Windows Server 2012 R2 Standard 和适当数量的用户和/或你的环境的设备客户端访问许可证购买许可证。  
  
     从零售商店、 经销商，或使用的帮助，可以为 Windows Server 2012 R2 Standard 购买许可证[Microsoft 合作伙伴](https://pinpoint.microsoft.com/SelectCulture.aspx)。  
  
    > [!NOTE]
    >  如果您最初购买 Windows Server 2012 R2 Standard，并执行降级权限安装两个虚拟实例之一为 Windows Server Essentials，你不需要购买任何许可证。  
    >   
    >  如果通过批量许可渠道购买了 Windows Server 2012 R2 Standard，您可以对 Windows Server 2012 R2 标准版从批量许可服务中心 (VLSC) 下载的 ISO 映像和产品密钥。  
    >   
    >  如果通过其他渠道购买了 Windows Server 2012 R2 Standard，则可以从 Windows Server essentials 下载的 ISO 映像和评估版产品密钥[TechNet 评估中心](https://technet.microsoft.com/evalcenter/jj659306.aspx)。 下一步中介绍的转换操作会将评估产品转换为完全授权和受支持的产品。  
  
2.  以管理员身份打开 Windows PowerShell，然后运行以下命令：  
  
     **dism /online /set-edition:ServerStandard /accepteula /productkey:***产品密钥*(其中*产品密钥*是您的 Windows Server 2012 R2 Standard 副本的产品密钥)。  
  
     服务器将重新启动，完成转换过程。  
  
 过渡后，Windows Server Essentials 功能保留在服务器上，并支持最多 100 个用户和 200 台设备。  
  
## <a name="see-also"></a>请参阅  
  

-   [将服务器数据迁移到 Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [将服务器数据迁移到 Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

