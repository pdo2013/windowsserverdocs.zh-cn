---
title: 步骤 2 配置 EDGE1
description: 本主题是一部分的测试实验室指南-使用 Windows Server 2016 的 Windows NLB 的群集中演示 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84457351-1ca7-4e7c-8e2c-53d55b1fcdc0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 03d7d85f730cf792238aef372337030861cd6a9d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281677"
---
# <a name="step-2-configure-edge1"></a>步骤 2 配置 EDGE1

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在 DirectAccess 服务器上执行下列过程：

## <a name="to-configure-directaccess-on-edge1"></a>若要 EDGE1 上配置 DirectAccess
  
1.  上**启动**屏幕上，键入**RAMgmtUI.exe**，然后按 ENTER。 如果出现了“用户帐户控制”  对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”  。  
  
2.  在远程访问管理控制台中，在左窗格中，单击**配置**。  
  
3.  在控制台中，在中间窗格中**步骤 2 远程访问服务器**区域中，单击**编辑**。  
  
4.  在中**远程访问服务器设置**向导中，单击**前缀配置**。 上**前缀配置**页上，在**分配给 DirectAccess 客户端计算机的 IPv6 前缀**，输入**2001:db8:1:1000:: / 59**，然后单击**下一步**.  
  
5.  单击 **“完成”** 。  
  
6.  在控制台的中间窗格中，单击**完成**。  
  
7.  上**远程访问评审**对话框中，查看配置设置，然后单击**应用**。 在 **“应用远程访问设置向导设置”** 对话框中，单击 **“关闭”** 。
