---
title: 在扩展中使用 PowerShell
description: 在扩展 Windows Admin Center SDK (项目 Honolulu) 中使用 PowerShell
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ae5004104150c510a56c06161c9280e029968298
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867598"
---
# <a name="using-powershell-in-your-extension"></a>在扩展中使用 PowerShell #

>适用于：Windows Admin Center，Windows Admin Center 预览版

让我们更深入到 Windows Admin Center 扩展 SDK-让我们讨论一下将 PowerShell 命令添加到你的扩展。

## <a name="powershell-in-typescript"></a>在 TypeScript 中的 PowerShell ##

Gulp 生成过程将只包含需要较长的任何".ps1"的"/ src/资源/scripts"文件夹中放置和内置"/ src/生成"文件夹下的"powershell 脚本"类的生成步骤。

>[!NOTE] 
> 不手动更新"powershell-scripts.ts"和"strings.ts"文件。 将在下一步生成覆盖所做的任何更改。

### <a name="adding-your-own-powershell-script"></a>添加您自己的 PowerShell 脚本 ##

我们将添加有关即将添加您自己的 PowerShell 脚本的详细信息。

### <a name="managing-powershell-sessions"></a>管理 PowerShell 会话 ###

使用 PowerShell 在 Windows Admin Center 中的过程中，会话时，脚本执行过程的必需的组件。 若要在远程托管服务器上运行脚本，PowerShell，请使用运行空间。 Windows Admin Center 包装在一个 PowerShellSession 对象来管理生存期，并启用执行顺序的脚本的运行空间重复使用的运行空间的创建和管理。

每个组件需要创建对 AppContextService 类使用三个不同的选项创建一个会话对象的引用：
<!-- I don't 100% get this part - it looks like you're adding 3 arguments - nodeName, <session key>, and <PowerShellSessionRequestOptions>. I got that from looking at the examples, not the text. We need to rework those paras explaining. -->
``` ts
this.psSession = this.appContextService.powerShell.createSession(this.appContextService.activeConnection.nodeName);
```

通过提供到 createSession 方法的节点名称，新的 PowerShell 会话创建、 使用，并立即销毁 PowerShell 调用完成后。 此功能非常适合进行一次性调用，但重复的使用可能会影响性能。 会话需要大约 1 秒钟，若要创建，因此持续回收会话可能会导致速度变慢。

``` ts
this.psSession = this.appContextService.powerShell.createSession(this.appContextService.activeConnection.nodeName, '<session key>');
```

第一个可选参数是\<会话密钥\>参数。 这将创建可以查找和重复使用，即使跨组件 （即 Component1 可以具有键"SME-ROCKS，"创建会话和 Component2 可以使用该会话） 的加密的会话。  

``` ts
this.psSession = this.appContextService.powerShell.createSession(this.appContextService.activeConnection.nodeName, '<session key>', <PowerShellSessionRequestOptions>);
```
<!-- The second optional parameter is \<PowerShellSessionRequestOptions\> that does ... ? -->
### <a name="common-patterns"></a>常见模式 ###

在大多数情况下，创建中的加密的会话**ngOnInit**方法，并在然后释放**ngOnDestroy**。 有一个组件，但不是在组件之间共享基础会话中的多个 PowerShell 脚本时，请遵循此模式。

为获得最佳结果，请确保会话创建内部组件而不是服务管理-这有助于确保该生存期和清理可以正确管理。