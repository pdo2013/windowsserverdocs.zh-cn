---
title: "从 Windows Server Essentials 转换为 Windows Server 2012 R2 标准"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-r2-standard"></a>从 Windows Server Essentials 转换为 Windows Server 2012 R2 标准

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

Windows Server 2016 是云准备好销售给操作系统引入新的技术，可使过渡到云计算轻松时支持当前的工作负载。 Windows Server 2016 内容帮助让你做好准备。

 Windows Server 软件包最 25 用户并 50 设备的支持。 当你的企业需要超过限制时，你可以执行对 Windows Server 2012 R2 Standard 保持许可证兼容 Windows Server Essentials 从就地许可证转换。  
  
 你对 Windows Server 2012 R2 Standard 过渡后，删除的用户帐户和设备限制，但特定于 Windows Server Essentials（如仪表板、远程网站访问和客户端计算机备份）的功能仍保持可用。 但是，这些功能的技术限制 100 的用户帐户和设备 200 最多支持。 客户端计算机备份功能将支持 75 最多的设备的备份。  
  
> [!IMPORTANT]
>   Windows Server 2012R2 Standard 每个用户或您的环境中的设备需要客户访问许可证 (CAL)。 这不会使用 CAL 型号和未配备任何 Cal，这是 Windows Server Essentials 不同。 在从 Windows Server Essentials 转换为 Windows Server 2012 R2 Standard 时，你将需要购买的相应的数目和您的环境（大部分客户购买用户 Cal）Cal 类型。  
  
## <a name="before-the-transition"></a>之前转换  
  
-   从 Windows Server Essentials 过渡到 Windows Server 2012 R2 Standard 之前, 你应完全备份服务器数据。  
  
    > [!IMPORTANT]
    >  没有完整服务器的备份，无法将服务器还原到之前过渡当时正在的状态。  
  
-   此外，请确保你读取并了解有关 Windows Server 2012 R2 Standard 的最终用户许可协议 (EULA)。 若要查看 EULA:  
  
    1.  以管理员身份打开命令窗口中。  
  
    2.  运行以下命令：  
  
         **dism /online /set-edition: ServerStandard /geteula:***eula 路径*(其中*eula 路径*表示你想要保存 EULA 文件; 的位置，例如：C:\ws8std_eula.rtf)。 请务必.rtf 用作文件扩展名。  
  
    3.  打开该文件，你保存的位置，然后双击该文件以打开它。  
  
## <a name="transition-to--windows-server-2012-r2-standard"></a>对 Windows Server 2012 R2 标准转换  
 之后你已经决定从过渡 Windows Server Essentials 对 Windows Server 2012 R2 Standard，完成这两个步骤：  
  
1.  Windows Server 2012 R2 Standard 和适当的用户和/或设备访问许可证您的环境的数量，购买许可证。  
  
     通过零售电源插座，分销商，或的帮助下，您可以为 Windows Server 2012 R2 Standard 购买许可证[Microsoft 合作伙伴](https://pinpoint.microsoft.com/SelectCulture.aspx)。  
  
    > [!NOTE]
    >  如果你购买了最初 Windows Server 2012 R2 Standard，并执行你降级权利作为 Windows Server Essentials 安装你的两个虚拟实例之一，你不需要购买任何其他内容。  
    >   
    >  如果你购买标准通过批量许可的频道的 R2 Windows Server 2012，你可以为 Windows Server 2012 R2 Standard 从批量许可服务中心 (VLSC) 下载 ISO 映像和产品密钥。  
    >   
    >  如果你购买标准从任何其他频道的 R2 Windows Server 2012，你可以下载 ISO 映像评估产品密钥适用于和从 Windows Server Essentials [TechNet 评估中心](https://technet.microsoft.com/evalcenter/jj659306.aspx)。 下一步中所述执行过渡时，会将该评估的产品转换为完全授权和支持的产品。  
  
2.  打开 Windows PowerShell 以管理员身份，并运行以下命令：  
  
     **dism /online /set-edition: ServerStandard /accepteula /productkey:***产品密钥*(其中*产品密钥*是对你的 Windows Server 2012 R2 Standard 副本的产品密钥)。  
  
     在服务器重启才能完成过渡过程。  
  
 切换之后, Windows Server Essentials 功能保留在服务器上，并且高达 100 用户和 200 设备的支持。  
  
## <a name="see-also"></a>请参阅  
  

-   [Windows Server Essentials 到迁移服务器数据](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Windows Server Essentials 到迁移服务器数据](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

