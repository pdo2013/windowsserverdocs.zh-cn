---
title: 网络策略服务器用户数据收集
description: 哪些信息用于通过 Windows Server 2016 中的网络策略服务器帮助用户进行身份验证。
author: MicrosoftGuyJFlo
manager: mtillman
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.date: 05/01/2018
ms.openlocfilehash: 5bddd22c9c2f954435cc6ce37347d18c76ee7de3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888738"
---
# <a name="network-policy-server-user-data-collection"></a>网络策略服务器用户数据收集

本文档介绍如何查找用户信息收集由网络策略服务器 (NPS)，以防你想要将其删除。

>[!Note]
>如果有兴趣查看或删除个人数据，请查看在 Microsoft 的指导[Windows 数据主体请求 gdpr](https://docs.microsoft.com/microsoft-365/compliance/gdpr-dsr-windows)站点。 如果您正在寻找有关 GDPR 的常规信息，请参阅[GDPR 服务信任门户部分](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)。

## <a name="information-collected-by-nps"></a>Nps 中收集的信息

- 时间戳
- 事件时间戳
- Username
- 完全限定的用户名
- 客户端 IP 地址
- 客户端供应商
- 客户端友好名称
- 身份验证类型
- 有关 RADIUS 协议的许多其他字段

## <a name="gather-data-from-nps"></a>从 NPS 中收集数据

如果启用并配置记帐数据，可以从 SQL Server 或根据配置的日志文件获取的用户的 NPS 身份验证尝试的记录。 

如果 SQL server 配置记帐数据，所有查询都记录其中 User_Name = `'<username>'`。

如果日志文件配置为记帐数据，然后搜索的日志文件的`<username>`若要查找的所有日志条目。

网络策略和访问服务事件日志条目被视为重复的记帐数据，无需进行收集。

如果未启用记帐数据，则可以通过搜索从网络策略和访问服务事件日志中获取的用户的 NPS 身份验证尝试的记录`<username>`。
