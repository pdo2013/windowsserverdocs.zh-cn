---
title: AD FS 故障排除-循环检测
description: 本文档介绍如何进行故障排除的循环检测
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cc8eeb11e44da3b8f26b1ab94143c189bca9ed38
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830908"
---
# <a name="ad-fs-troubleshooting---loop-detection"></a>AD FS 故障排除-循环检测 
 
当信赖方持续拒绝的有效安全令牌，并将重定向回 AD FS，AD FS 中的循环时发生。

## <a name="loop-detection-cookie"></a>循环检测 cookie
若要避免这种情况发生，AD FS 已实现所谓的循环检测 cookie。 默认情况下，AD FS 将 cookie 写入到名为 web 被动客户端**MSISLoopDetectionCookie**。 此 cookie 包含时间戳值和多种令牌颁发值。  这样 AD FS 以跟踪记录的频率和如何访问客户端很多时候过特定的时间范围中的联合身份验证服务。

如果被动客户端访问令牌五 （5） 时间内 20 秒，AD FS 联合身份验证服务会引发以下错误：

**MSIS7042:进行了相同的客户端浏览器会话{0}请求中最后一个{1}秒。有关详细信息，请与你的管理员联系。**

进入无限循环通常由导致产生错误行为信赖方应用程序未成功使用 AD FS 颁发的令牌和应用程序发送被动客户端返回到 AD FS 中，重复，新的令牌。  AD FS 是将颁发被动客户端的新令牌每次，只要它们不超过 5 次请求，在 20 秒内。 

## <a name="adjusting-the-loop-detection-cookie"></a>调整循环检测 cookie
可以使用 PowerShell 来更改值和时间跨度值所颁发的令牌的数量。

```powershell
Set-AdfsProperties -LoopDetectionMaximumTokensIssuedInterval 5  -LoopDetectionTimeIntervalInSeconds 20
```
最小值**LoopDetectionMaximumTokensIssuedInterval**为 1。

最小值**LoopDetectionTimeIntervalInSeconds**为 5。

在执行性能测试时，也可以禁用循环检测。

```powershell
Set-AdfsProperties -EnableLoopDetection $false
```

>[!IMPORTANT]
>它是不建议以永久禁用循环检测，因为这样可以防止用户输入到无限循环状态。


## <a name="next-steps"></a>后续步骤

- [AD FS 进行故障排除](ad-fs-tshoot-overview.md)



