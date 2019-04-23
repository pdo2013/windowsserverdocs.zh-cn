---
title: 配置对无线网络的支持
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d7020d4-fd46-4858-a406-de5c0f21ea06
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c5c98727b81bf37fdb3f90c612270462a51908c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833108"
---
# <a name="configure-support-for-a-wireless-network"></a>配置对无线网络的支持

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

你可以将操作系统配置为支持无线网络。 必须满足以下要求，才能在服务器上启用无线支持：  
  
-   服务器中必须安装有线网络适配器。  
  
-   如果操作系统不支持网络适配器，则必须预安装无线网络适配器的正确驱动程序。  
  
-   必须提供用于启用和禁用无线网络适配器的功能。 用于完成该操作的方法可能包括服务器上的物理按钮或仪表板中的自定义用户界面。 产品手册应提供用于启用和禁用无线网络适配器的步骤。  
  
-   必须提供用于选择并连接到无线网络的功能。 应通过在仪表板中添加自定义用户界面来完成该操作。 产品手册应提供用于选择并连接到无线网络的步骤。  
  
-   如果需要支持临时无线网络，则必须在仪表板中提供扩展的用户界面。 在 Windows Server 2008 R2 控制面板中，该用户界面可以是用于启动“设置临时无线网络向导”的一个按钮或链接。  
  
## <a name="additional-considerations"></a>其他注意事项  
 在配置无线网络支持时，还应考虑下面的信息：  
  
-   服务器必须以有线方式连接到网络，以运行设置程序。  
  
-   在其上执行裸机还原的网络计算机必须以有线方式连接到网络。  
  
-   服务器必须以有线方式连接到网络，才能执行服务器裸机还原。  
  
-   如果在服务器上创建了临时网络，无线网络适配器将专用于该临时网络，所以用户必须始终将网络电缆插入服务器以获取 Internet 连接。  
  
> [!NOTE]
>  有关配置网络连接的详细信息，请参阅[预配置路由器](Preconfiguring-a-Router.md)。  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [部署准备的映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)