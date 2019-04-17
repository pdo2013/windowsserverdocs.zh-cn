---
title: "顶级类别添加到启动栏 （Macintosh 操作系统）"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ee2173c3-e464-4001-9f43-6d926a575092
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ae4eb5943d37b4a9d3b554af28cb425420782cf8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="add-top-level-categories-to-the-launchpad-macintosh-operating-system"></a>顶级类别添加到启动栏 （Macintosh 操作系统）

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

你可以添加到运行 Macintosh 操作系统的计算机上启动栏的顶级类别。 若要创建顶级类别中添加的启动栏加载项，可以使用此页面和标题操作方法信息组成： 添加任务和启动栏的类别？在[Windows Server 解决方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)。  
  
 下面的示例显示了如何指定你启动栏项都必须是.launchpad 文件中的顶级类别：  
  
```  
  
<?xml version="1.0" encoding="utf-8" ?>  
<LaunchPad resFile="Localizable">  
   <Category id="Microsoft.Launchpad.HomeCategory" name="SampleAddin"  image="SampleImage01.png">  
      <Task id="Microsoft.Launchpad.SampleAddin.Pictures"   
          name="Pictures"       
           src="smb://%ServerAddress%/Pictures"   
         image="SampleImage02.png"/>  
   </Category>  
</LaunchPad>  
```  
  
 若要将顶级类别的条目，类别元素的 Id 特性必须"Microsoft.Launchpad.HomeCategory"。  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)