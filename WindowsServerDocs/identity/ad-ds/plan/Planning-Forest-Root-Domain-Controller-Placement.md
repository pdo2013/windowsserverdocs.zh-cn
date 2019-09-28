---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: 规划林根域控制器放置
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e619d5f5e9cc317f9ba1548d5ed3a32e7bd12c2c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402549"
---
# <a name="planning-forest-root-domain-controller-placement"></a>规划林根域控制器放置

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

需要目录林根级域控制器来为需要访问非其自己的域中的资源的客户端创建信任路径。 将目录林根级域控制器置于中心位置，并放置在托管数据中心的位置。 如果给定位置的用户需要访问同一位置的其他域中的资源，且数据中心和用户位置之间的网络可用性不可靠，则可以在该位置添加林根域控制器或创建这两个域之间的快捷方式信任。 在域之间创建快捷方式信任更为经济，除非有其他原因需要将目录林根级域控制器放置在该位置。  
  
快捷信任有助于优化来自任一域中的用户发出的身份验证请求。 有关域之间的快捷方式信任的详细信息，请参阅[了解何时创建快捷方式信任一](https://go.microsoft.com/fwlink/?LinkId=107061)文。  
  
要使工作表可以帮助你记录目录林根级域控制器的位置，请参阅[Windows Server 2003 部署工具包的作业帮助](https://go.microsoft.com/fwlink/?LinkID=102558)、下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services，并打开 "域控制器布局 "（DSSTOPO_4）。  
  
创建目录林根级域时，需要引用此信息。 有关部署目录林根级域的详细信息，请参阅[部署 Windows Server 2008 林根级域](https://technet.microsoft.com/library/cc731174.aspx)。  
