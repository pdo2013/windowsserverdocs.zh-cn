---
title: 配置 Windows Server Update Services (WSUS) 内容服务器
description: 本主题是 BranchCache 部署指南为 Windows Server 2016 中，该示例演示了如何部署 BranchCache 在分布式和托管缓存模式下以优化分支机构中的 WAN 带宽使用情况的一部分。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e8576282be92f02daf716da82ea75eddc755ee5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873838"
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>配置 Windows Server Update Services (WSUS) 内容服务器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

安装 BranchCache 功能并启动 BranchCache 服务后, WSUS 服务器必须配置为存储在本地计算机上的更新文件。 

在配置 WSUS 服务器存储在本地计算机上的更新文件时，都通过下载并存储 WSUS 服务器时直接更新元数据和更新文件。 这可确保 BranchCache 客户端计算机接收 Microsoft 产品更新文件，从 WSUS 服务器而不是直接从 Microsoft 更新网站。  
  
有关 WSUS 同步的详细信息，请参阅[设置更新同步](https://technet.microsoft.com/library/mt612311.aspx)  