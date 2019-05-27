---
title: 从 Windows Server Essentials 转换到 Windows Server 2012 Standard
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51bcf124-c215-4e9d-9fa8-a90fa2c2fa22
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d2005b72adede72b718fa5b49b93435f5fbac1bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882498"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-standard"></a>从 Windows Server Essentials 转换到 Windows Server 2012 Standard

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

 Windows Server® 2012 Essentials 支持多达 25 名用户和 50 台设备。 当你的业务需求超过限制时，可以为 Windows Server 2012 Standard 保持许可证的合规性的就地许可证过渡执行从 Windows Server Essentials。  
  
## <a name="how-the-transition-affects-user-and-device-limits"></a>转换如何影响用户和设备限制  
 转换到 Windows Server 2012 Standard 后，用户帐户和设备限制都会消除，但仍是唯一的 Windows Server Essentials （如仪表板、 远程 Web 访问和客户端计算机备份），这些功能仍保持可用。 但是，由于这些功能在技术上的限制，最多只能支持 75 个用户帐户和 75 台设备。 如果有必要添加 75 个用户帐户或设备时，应关闭 Windows Server Essentials 功能并使用 Windows Server 2012 Standard 本机工具来管理用户帐户和设备。  
  
> [!IMPORTANT]
>   Windows Server 2012 Standard 需要为每个用户或您的环境中的设备客户端访问许可证 (CAL)。 这是不同于 Windows Server Essentials，不使用 CAL 模式，也不包含任何 Cal。  在从 Windows Server Essentials 转换到 Windows Server 2012 Standard 时，将需要购买相应数量和类型的 Cal （大多数客户购买的用户 Cal） 环境。  
  
## <a name="before-the-transition"></a>转换前  
  
-   从 Windows Server Essentials 过渡到 Windows Server 2012 Standard 之前，应当完整备份服务器数据。  
  
    > [!IMPORTANT]
    >  如果不完整备份服务器数据，则无法将服务器还原到转换前的状态。  
  
-   此外，请确保您阅读并了解最终用户许可协议 (EULA) 的 Windows Server 2012 Standard。 查看 EULA 的步骤：  
  
    1.  以管理员身份打开命令窗口。  
  
    2.  运行下面的命令：  
  
         **dism /online /set-edition:ServerStandard /geteula: eula path**  
  
         其中 **eula path** 表示希望用来保存 EULA 文件的位置。 例如，C:\ws8std_eula.rtf。  请务必使用 .rtf 作为文件后缀名。  
  
    3.  打开保存该文件的位置，然后双击打开该文件。  
  
## <a name="transition-to--windows-server-2012-standard"></a>过渡到 Windows Server 2012 Standard  
 之后您已决定转换从 Windows Server Essentials 到 Windows Server 2012 Standard，完成这两个步骤：  
  
1.  Windows Server 2012 Standard 和适当数量的用户和/或设备客户端访问许可证为您的环境购买许可证。  
  
     从零售商店、 经销商，或使用的帮助，可以为 Windows Server 2012 Standard 购买许可证[Microsoft 合作伙伴](https://pinpoint.microsoft.com/SelectCulture.aspx)。  
  
    > [!NOTE]
    >  如果最初购买 Windows Server 2012 Standard 和执行降级权限安装两个虚拟实例之一为 Windows Server Essentials，你不需要购买任何许可证。  
    >   
    >  如果通过批量许可渠道购买了 Windows Server 2012 Standard，您可以为 Windows Server 2012 Standard 从批量许可服务中心 (VLSC) 下载的 ISO 映像和产品密钥。  
    >   
    >  如果你通过其他渠道购买 Windows Server 2012 Standard 可以下载的 ISO 映像和评估版产品密钥适用于从 Windows Server Essentials [TechNet 评估中心](https://technet.microsoft.com/evalcenter/jj659306.aspx)。 下一步中介绍的转换操作会将评估产品转换为完全授权和受支持的产品。  
  
2.  以管理员身份打开 Windows PowerShell，然后运行如下命令。  
  
     **dism /online /set-edition:ServerStandard /accepteula /productkey:** *产品密钥*  
  
     其中*产品密钥*是您的 Windows Server 2012 Standard 副本的产品密钥。  
  
     服务器将重新启动，完成转换过程。  
  
 过渡后，Windows Server Essentials 功能保留在服务器上，并支持最多 75 位用户和 75 台设备。 如果你超出这些限制的任何一个，应使用 Windows Server 2012 Standard 本机工具来管理用户帐户和设备。  
  
 此外，你过渡到 Windows Server 2012 Standard 后，Windows Server Essentials 的媒体功能将不再可用。 这包括远程 Web 访问的媒体功能和仪表板上的媒体设置。  
  
## <a name="turn-off--windows-server-essentials-features"></a>关闭 Windows Server Essentials 的功能  
 如果您不再需要 Windows Server Essentials 仪表板或其他增值功能来管理服务器，可以关闭这些功能，并从服务器中删除。  
  
 **关闭 Windows Server Essentials 功能向导**可帮助你卸载这些功能。 它还会清除文件创建的 Windows Server Essentials 服务器软件的服务器。  某些清除操作会立即执行，而某些操作则需要等到服务器重新启动之后才会启动。  
  
 **关闭 Windows Server Essentials 功能向导**需要在完成向导前手动卸载所有加载项。 若要查看已安装加载项的列表，请在仪表板中打开“应用程序”页。 此向导会通知你是否检测到已安装的加载项，如果有则提醒你卸载这些加载项。  
  
 **关闭 Windows Server Essentials 功能向导**，可选择是否要关闭 Windows Server Essentials 功能后保留客户端的备份文件的计算机。  
  
 有两种方法来运行**关闭 Windows Server Essentials 功能向导**从仪表板：  
  
#### <a name="from-the-alert"></a>通过警报运行  
  
1.  在仪表板中打开“警报查看器”。  
  
2.  在组织列表中，选择报告有关在转换之后关闭 Windows Server Essentials 功能的信息的警报。  
  
3.  在警报中，单击**关闭 Windows Server Essentials 功能**。  
  
#### <a name="from-the-get-help-and-support-pane"></a>通过“获取帮助和支持”窗格运行  
  
1.  在主页上单击“获取帮助和支持”。  
  
2.  单击**关闭 Windows Server Essentials 功能向导**。  
  
 通过执行某些任务可能**关闭 Windows Server Essentials 功能向导**将无法成功完成。 某些情况下，这可能会阻止仪表板的运行。 如果发生此情况，你可以运行以下文件手动启动该向导：  
  
 **%systemdrive%\Program Files\Windows Server\Bin\TurnOffFeaturesWizard.exe**  
  
## <a name="see-also"></a>请参阅  
  

-   [转换到 Windows Server 2012 R2 Standard](Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [将服务器数据迁移到 Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [转换到 Windows Server 2012 R2 Standard](../migrate/Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [将服务器数据迁移到 Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

