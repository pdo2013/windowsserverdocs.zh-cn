---
title: 应用程序注意事项
description: MultiPoint 服务上的应用程序兼容性信息
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 445e6184-4e1e-4f10-ad3c-042f2a6c2f5f
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 400f87c09f1b2e897d67f94e9b7ac12ae0a1e799
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839828"
---
# <a name="application-considerations"></a>应用程序注意事项
  
## <a name="application-compatibility"></a>应用程序兼容性

你想要在 MultiPoint 服务系统上运行任何应用程序必须满足以下要求：
  
- 它应安装和 Windows Server 2016 上运行 
- 它必须是会话感知，以便每个用户可以在 MultiPoint 系统中运行的应用程序实例。
  
如果应用程序未指定此要求我们建议来尝试安装应用程序并在远程桌面会话中使用它。 

## <a name="addressing-application-compatibility-problems"></a>解决应用程序兼容性问题  
MultiPoint 服务提供将工作站与同一台主机计算机上以虚拟方式运行的 Windows 10 企业版版本的完整实例相关联的选项。 对于关键的应用程序将不会运行多个实例的多个用户，或将不安装在 64 位操作系统上，这可能是一种解决方案。 部署桌面这种方式需要使用 MultiPoint 管理器中的虚拟桌面选项卡：  
  
-   启用虚拟桌面  
-   创建一个桌面模板  
-   自定义与问题应用程序模板  
-   将工作站与自定义模板相关联  

每个工作站启动从同一模板中，因此任何更改都会被清除，每次计算机启动的时。  
  
>[!NOTE] 
>请务必确认你想要在多点上运行的应用程序的授权要求。 尽管您安装一个复制应用程序可能需要的每用户许可。  
  
