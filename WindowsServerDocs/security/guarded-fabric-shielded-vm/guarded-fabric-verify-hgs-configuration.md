---
title: 验证 HGS 配置
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 8f01df37-f18e-4386-ae73-ecf84feaa9df
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8098edd1eea475cea1face5541459b262364a07b
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469547"
---
# <a name="verify-the-hgs-configuration"></a>验证 HGS 配置

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016


接下来，我们需要验证一切按预期方式运行。 若要执行此操作，请在提升的 Windows PowerShell 控制台中运行以下命令：

```powershell
Get-HgsTrace -RunDiagnostics
```

HGS 配置尚未包含有关将受保护的构造中的主机的信息，因为诊断将指示没有主机将能够尚未成功证明。 忽略此结果，并查看诊断提供的其他信息。

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

HGS 群集中每个节点上运行诊断程序。

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [部署受保护的主机](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)

