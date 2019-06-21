---
title: 步骤 4 验证群集
description: 本主题是指南的在 Windows Server 2016 中的群集中部署远程访问的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f22dcf10-b453-4664-a9ef-e40e95c72f63
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 424c4f881c168ea691dd51cd2d86a4a234c41075
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282982"
---
# <a name="step-4-verify-the-cluster"></a>步骤 4 验证群集

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍如何验证正确配置了 DirectAccess 群集部署。  
  
### <a name="to-verify-access-to-internal-resources-through-the-cluster"></a>若要验证到内部资源通过群集的访问权限  
  
1.  将 DirectAccess 客户端计算机连接到公司网络并获取组策略。  
  
2.  将客户端计算机连接到外部网络并尝试访问内部资源。  
  
    你应能够访问所有公司资源。  
  
3.  通过关闭或断开与外部网络，除一台群集服务器的所有在群集中测试通过的每个服务器的连接。 在客户端计算机上，尝试访问公司资源。 重复该测试不同的群集服务器上。  
  
    您应能够通过每台群集服务器中访问所有公司资源。  
  


