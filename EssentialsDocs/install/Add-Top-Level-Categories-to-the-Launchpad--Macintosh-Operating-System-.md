---
title: 向快速启动板中添加顶级类别（Macintosh 操作系统）
description: 介绍如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869958"
---
# <a name="add-top-level-categories-to-the-launchpad-macintosh-operating-system"></a>向快速启动板中添加顶级类别（Macintosh 操作系统）

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

你可以在运行 Macintosh 操作系统的计算机上向快速启动板中添加顶级类别。 若要创建添加顶级类别的快速启动板加载项，可以使用此页从和题为操作方法的主题的信息的组合：向快速启动板添加任务和类别？在中[Windows Server 解决方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)。  
  
 以下示例显示了如何将快速启动板条目指定为 .launchpad 文件中的顶级类别：  
  
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
  
 为使该条目成为顶级类别，Category 元素的 Id 属性必须为“Microsoft.Launchpad.HomeCategory”。  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [部署准备的映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)