---
title: 验证 HGS 配置
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 8f01df37-f18e-4386-ae73-ecf84feaa9df
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: d219b7aa9ca1e17df3281fd756106a6f07864116
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386346"
---
# <a name="verify-the-hgs-configuration"></a>验证 HGS 配置

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016


接下来，我们需要验证一切是否按预期工作。 为此，请在提升的 Windows PowerShell 控制台中运行以下命令：

```powershell
Get-HgsTrace -RunDiagnostics
```

因为 HGS 配置还不包含有关将在受保护的构造中的主机的信息，所以，诊断将指示任何主机都无法成功证明。 忽略此结果，并查看诊断提供的其他信息。

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

在 HGS 群集中的每个节点上运行诊断。

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [部署受保护的主机](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)

