---
title: 为 Https 通信配置 HGS
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: f0cbf6a6dc1970499758a6a48bfaadb95c464ec1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403673"
---
# <a name="configure-hgs-for-https-communications"></a>为 HTTPS 通信配置 HGS

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

默认情况下，初始化 HGS 服务器时，它会将 IIS 网站配置为仅限 HTTP 通信。
所有与 HGS 传输的敏感材料始终使用消息级加密进行加密，但是，如果你需要更高的安全级别，则还可以通过使用 SSL 证书配置 HGS 来启用 HTTPS。

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)] 

