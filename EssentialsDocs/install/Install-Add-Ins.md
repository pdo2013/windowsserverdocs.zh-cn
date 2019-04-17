---
title: "安装 Add-Ins"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e62e4f07-c2ba-4c5e-b30c-bdc287cd654e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d00cb6886e812ee2b780ad79e1fba44442e279ad
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="install-add-ins"></a>安装 Add-Ins

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

你可以通过安装它们在创建映像之前包括所有服务器或的客户端计算机上的加载项。 随后将会加载项会自动包括复制使用该图像的所有计算机上。 你可以安装外接程序通过运行.wssx 文件，也可以通过遵循的指南中安装单独的加载项的文件[SDK 文档](https://go.microsoft.com/fwlink/?LinkID=248648)每种类型的加载项。 如果你安装通过.wssx 文件外, 接程序可以卸载通过 Add-In 管理器。 如果你安装的个别文件外, 接程序未管理从 Add-In 管理器。  
  
> [!NOTE]
>  安装或下载（包括仪表板标签页和健康通知）服务器上的任何软件应该会有一个本地化的界面语言的服务器安装的操作系统相匹配。  
  
#### <a name="to-install-an-add-in"></a>若要安装的加载项  
  
1.  （可选）如果你安装外接程序，方法是使用.wssx 文件，请完成以下步骤：  
  
    1.  保存 < AddinName\ >.wssx 文件参考计算机上。  
  
    2.  双击以打开 Add-in 安装向导.wssx 文件。  
  
    3.  按照该向导中的说明完成安装。  
  
2.  （可选）每种类型的加载项 SDK 中定义中适当的位置中安装个别外接程序文件。  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)