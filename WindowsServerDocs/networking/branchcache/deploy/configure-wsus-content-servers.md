---
title: 配置 Windows Server Update Services (WSUS) 的内容服务器
description: 本主题介绍分支缓存部署指南为 Windows Server 2016，其中演示如何在分布式托管的缓存模式优化分支机构中的 WAN 带宽使用量部署分支缓存的一部分。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8200a0905f7bc5c403288a22faece5f84eac8af9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>配置 Windows Server Update Services (WSUS) 的内容服务器

>适用于：Windows Server（半年通道），Windows Server 2016

安装分支缓存功能并后启动分支缓存服务，必须配置 WSUS 服务器存储在本地计算机上的更新文件。 

配置 WSUS 服务器存储在本地计算机上的更新文件时，更新元数据，这些更新文件下载通过和存储时 WSUS 服务器直接。 这可以确保分支缓存的客户端接收 Microsoft 产品更新文件，这是从 WSUS 服务器，而不是直接从 Microsoft 更新网站。  
  
有关 WSUS 同步的详细信息，请参阅[更新同步设置](https://technet.microsoft.com/en-us/library/mt612311.aspx)  