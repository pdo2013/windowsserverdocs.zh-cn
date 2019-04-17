---
title: "从 Windows Server Essentials 转换为 Windows Server 2012 Standard"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-standard"></a>从 Windows Server Essentials 转换为 Windows Server 2012 Standard

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

 Windows Server® 软件包 2012 年最 25 用户并 50 设备的支持。 当你的企业需要超过限制时，你可以执行对 Windows Server 2012 Standard 保持许可证兼容 Windows Server Essentials 从就地许可证转换。  
  
## <a name="how-the-transition-affects-user-and-device-limits"></a>如何切换影响用户和设备限制  
 你对 Windows Server 2012 Standard 过渡后，删除的用户帐户和设备限制，但特定于 Windows Server Essentials（如仪表板、远程网站访问和客户端计算机备份），功能仍保持可用。 但是，这些功能的技术限制 75 的用户帐户和设备 75 最多支持。 如果需要添加多个 75 的用户帐户或设备，你应关闭 Windows Server Essentials 功能，并使用原始的 Windows Server 2012 Standard 工具管理用户帐户和设备。  
  
> [!IMPORTANT]
>   Windows Server 2012 Standard 每个用户或您的环境中的设备需要客户端访问许可证 (CAL)。 这不会使用 CAL 型号和未配备任何 Cal，这是 Windows Server Essentials 不同。  在从 Windows Server Essentials 转换为 Windows Server 2012 Standard 时，你将需要购买的相应的数目和您的环境（大部分客户购买用户 Cal）Cal 类型。  
  
## <a name="before-the-transition"></a>之前转换  
  
-   从 Windows Server Essentials 过渡到 Windows Server 2012 Standard 之前, 你应完全备份服务器数据。  
  
    > [!IMPORTANT]
    >  没有完整服务器的备份，无法将服务器还原到之前过渡当时正在的状态。  
  
-   此外，请确保你读取并了解有关 Windows Server 2012 Standard 的最终用户许可协议 (EULA)。 若要查看 EULA:  
  
    1.  以管理员身份打开命令窗口中。  
  
    2.  运行以下命令：  
  
         **Dism /online /set-edition: ServerStandard /geteula: eula 路径**  
  
         其中**eula 路径**表示你想要保存 EULA 文件的位置。 例如;C:\ws8std_eula.rtf。  请务必.rtf 用作文件扩展名。  
  
    3.  打开该文件，你保存的位置，然后双击该文件以打开它。  
  
## <a name="transition-to--windows-server-2012-standard"></a>对 Windows Server 2012 Standard 转换  
 之后你已经决定从过渡 Windows Server Essentials 对 Windows Server 2012 Standard，完成这两个步骤：  
  
1.  Windows Server 2012 Standard 和适当的用户和/或设备访问许可证针对您的环境数，购买许可证。  
  
     通过零售电源插座，分销商，或的帮助下，您可以为 Windows Server 2012 Standard 购买许可证[Microsoft 合作伙伴](https://pinpoint.microsoft.com/SelectCulture.aspx)。  
  
    > [!NOTE]
    >  如果你购买 Windows Server 2012 Standard 最初并执行你降级权利作为 Windows Server Essentials 安装你的两个虚拟实例之一，你不需要购买任何其他内容。  
    >   
    >  如果你购买了 Windows Server 2012 Standard 通过批量许可的频道，你可以为 Windows Server 2012 Standard 从批量许可服务中心 (VLSC) 下载 ISO 映像和产品密钥。  
    >   
    >  如果你购买了 Windows Server 2012 Standard 从所有其他频道可以下载 ISO 映像评估产品密钥适用于和 Windows Server Essentials 从[TechNet 评估中心](https://technet.microsoft.com/evalcenter/jj659306.aspx)。 下一步中所述执行过渡时，会将该评估的产品转换为完全授权和支持的产品。  
  
2.  打开 Windows PowerShell 以管理员身份，并运行以下命令。  
  
     **dism /online /set-edition: ServerStandard /accepteula /productkey:***产品密钥*  
  
     其中*产品密钥*是对你的 Windows Server 2012 Standard 副本的产品密钥。  
  
     在服务器重启才能完成过渡过程。  
  
 切换之后, Windows Server Essentials 功能保留在服务器上支持和最多 75 用户和 75 设备。 如果你超过任一这些限制，你应该使用 Windows Server 2012 Standard 本机工具管理用户帐户和设备。  
  
 此外，您将转换为 Windows Server 2012 Standard 后，将不再提供 Windows Server Essentials 的媒体功能。 这包括远程网站访问和媒体设置仪表板上的媒体功能。  
  
## <a name="turn-off--windows-server-essentials-features"></a>关闭 Windows Server Essentials 功能  
 如果你不再需要在 Windows Server Essentials 仪表板或其他增值功能来管理服务器，你可以关闭这些功能，并从服务器中删除它们。  
  
 **关闭 Windows Server Essentials 功能向导**可帮助你卸载功能。 它还会清除已由 Windows Server Essentials 服务器软件的文件服务器。  其他人都启动后重新启动该服务器时，将立即，执行一些清洁操作。  
  
 **关闭 Windows Server Essentials 功能向导**需要才能完成向导中手动卸载所有加载项。 若要查看已安装的加载项的列表，请仪表板中打开应用程序页。 该向导将提醒你，如果它检测安装加载项并且将提示您将其卸载。  
  
 **关闭 Windows Server Essentials 功能向导**允许你选择是否保留关闭 Windows Server Essentials 功能后的客户端计算机备份文件。  
  
 有两种方法来运行**关闭 Windows Server Essentials 功能向导**仪表板中：  
  
#### <a name="from-the-alert"></a>从通知  
  
1.  从仪表板上，打开警报查看器。  
  
2.  组织列表中选择报告切换后关闭 Windows Server Essentials 功能的信息的警报。  
  
3.  在通知，单击**关闭 Windows Server Essentials 功能**。  
  
#### <a name="from-the-get-help-and-support-pane"></a>从获取帮助和支持窗格  
  
1.  在主页上，单击获取帮助和支持。  
  
2.  单击**关闭 Windows Server Essentials 功能向导**。  
  
 有可能某些任务由**关闭 Windows Server Essentials 功能向导**未成功完成。 在某些情况下，这可以阻止运行仪表板。 如果发生这种情况，你可以手动启动该向导通过运行该文件：  
  
 **%systemdrive%\Program Files\Windows Server\Bin\TurnOffFeaturesWizard.exe**  
  
## <a name="see-also"></a>请参阅  
  

-   [对 Windows Server 2012 R2 标准转换](Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Windows Server Essentials 到迁移服务器数据](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [对 Windows Server 2012 R2 标准转换](../migrate/Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Windows Server Essentials 到迁移服务器数据](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

