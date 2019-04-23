---
title: 对于 Https 通信配置 HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 83529a5bdb4547b9881bb307a8a4cd526552d02c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828998"
---
# <a name="configure-hgs-for-https-communications"></a>对于 HTTPS 通信配置 HGS

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

默认情况下，当初始化 HGS 服务器时它将 IIS 网站配置为仅限 HTTP 的通信。
HGS 来回传输的所有敏感材料都始终处于加密使用消息级加密，但如果需要更高级别的安全性还可以通过使用 SSL 证书配置 HGS 启用 HTTPS。

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)] 

