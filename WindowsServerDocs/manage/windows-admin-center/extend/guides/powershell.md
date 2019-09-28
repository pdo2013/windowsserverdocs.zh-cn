---
title: 在扩展中使用 PowerShell
description: 在扩展 Windows 管理中心 SDK 中使用 PowerShell （Project Honolulu）
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 05/09/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 6e99fc43d4acb7a70dfd3a8ba19dae6492c41b2b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357049"
---
# <a name="using-powershell-in-your-extension"></a>在扩展中使用 PowerShell #

>适用于：Windows Admin Center、Windows Admin Center 预览版

让我们更深入地了解 Windows 管理中心扩展 SDK，我们来谈谈如何向扩展添加 PowerShell 命令。

## <a name="powershell-in-typescript"></a>TypeScript 中的 PowerShell ##

Gulp 生成过程有一个生成步骤，该步骤将使用```{!ScriptName}.ps1``` ```\src\resources\scripts```文件夹中的任何一个， ```powershell-scripts```并将其生成```\src\generated```到文件夹下的类中。

>[!NOTE] 
> 不要手动更新```powershell-scripts.ts``` ```strings.ts```文件或文件。 你所做的任何更改将在下一次生成时被覆盖。

## <a name="running-a-powershell-script"></a>运行 PowerShell 脚本 ##
要在节点上运行的任何脚本都可以放置在中```\src\resources\scripts\{!ScriptName}.ps1```。 
>[!IMPORTANT] 
> 在运行之前```gulp generate``` ，在```{!ScriptName}.ps1```文件中进行的任何更改都不会反映在您的项目中。

该 API 的工作原理是：首先在目标节点上创建 PowerShell 会话，使用需要传入的任何参数创建 PowerShell 脚本，然后在已创建的会话上运行该脚本。

例如，我们有此脚本```\src\resources\scripts\Get-NodeName.ps1```：
``` ps1
Param
 (
    [String] $stringFormat
 )
 $nodeName = [string]::Format($stringFormat,$env:COMPUTERNAME)
 Write-Output $nodeName
```

我们将为目标节点创建 PowerShell 会话：
``` ts
const session = this.appContextService.powerShell.createSession('{!TargetNode}'); 
```
然后，我们将使用输入参数创建 PowerShell 脚本：
```ts
const script = PowerShell.createScript(PowerShellScripts.Get_NodeName, {stringFormat: 'The name of the node is {0}!'});
```
最后，我们需要在创建的会话中运行该脚本：
``` ts
  public ngOnInit(): void {
    this.session = this.appContextService.powerShell.createAutomaticSession('{!TargetNode}');
  }

  public getNodeName(): Observable<any> {
    const script = PowerShell.createScript(PowerShellScripts.Get_NodeName.script, { stringFormat: 'The name of the node is {0}!'});
    return this.appContextService.powerShell.run(this.session, script)
    .pipe(
        map(
        response => {
            if (response && response.results) {
                return response.results;
            }
            return 'no response';
        }
      ) 
    );
  }

  public ngOnDestroy(): void {
    this.session.dispose()
  }

```
现在，我们需要订阅刚刚创建的可观察函数。 将其放在需要调用函数以运行 PowerShell 脚本的位置：
```ts
this.getNodeName().subscribe(
     response => {
    console.log(response)
     }
);
```
通过向 createSession 方法提供节点名称，将创建并使用一个新的 PowerShell 会话，然后在 PowerShell 调用完成后立即将其销毁。 

### <a name="key-options"></a>密钥选项 ###
在调用 PowerShell API 时，有几个选项可用。 每次创建会话时，都可以使用或不使用密钥来创建会话。 

**键:** 这将创建一个可在多个组件上进行查找和重复使用的键控会话（也就是说，Component1 可以创建一个具有密钥 "ROCKS" 的会话，而 Component2 可以使用相同的会话）。如果提供了密钥，则必须通过调用 dispose （）来释放创建的会话，就像上面的示例中所做的那样。 不应将会话保留的时间超过5分钟。 
```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNode}', '{!Key}');
```

**无键**系统将自动为该会话创建一个密钥。 此会话将在3分钟后自动释放。 使用无键，扩展可以回收在创建会话时已可用的任何运行空间。 如果没有可用的运行空间，则将创建一个新的运行空间。 此功能适用于一次性调用，但重复使用会影响性能。 创建会话大约需要1秒钟，因此持续回收会话可能会导致瓶颈。

```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNodeName}');
```
或 
``` ts 
const session = this.appContextService.powerShell.createAutomaticSession('{!TargetNodeName}');
```
在大多数情况下，请在```ngOnInit()```方法中创建一个键控会话，然后在中```ngOnDestroy()```释放它。 如果组件中有多个 PowerShell 脚本，但基础会话不是跨组件共享的，请遵循此模式。
为了获得最佳结果，请确保在组件（而不是服务）内管理会话创建，这有助于确保可以正确管理生存期和清理。

为了获得最佳结果，请确保在组件（而不是服务）内管理会话创建，这有助于确保可以正确管理生存期和清理。

### <a name="powershell-stream"></a>PowerShell 流 ###
如果你有长时间运行的脚本，并且数据是渐进式输出的，则 PowerShell 流将允许你处理数据，而不必等待脚本完成。 接收到数据后，将立即调用可观察的下一个（）。
```ts
this.appContextService.powerShellStream.run(session, script);
```

### <a name="long-running-scripts"></a>长时间运行的脚本 ###
如果你有想要在后台运行的长时间运行的脚本，则可以提交工作项。 脚本的状态将由网关跟踪，状态的更新可以发送到通知。 
```ts
const workItem: WorkItemSubmitRequest = {
    typeId: 'Long Running Script',
    objectName: 'My long running service',
    powerShellScript: script,
    
    //in progress notifications
    inProgressTitle: 'Executing long running request',
    startedMessage: 'The long running request has been started',
    progressMessage: 'Working on long running script – {{ percent }} %',

    //success notification
    successTitle: 'Successfully executed a long running script!',
    successMessage: '{{objectName}} was successful',
    successLinkText: 'Bing',
    successLink: 'http://www.bing.com',
    successLinkType: NotificationLinkType.Absolute,

    //error notification
    errorTitle: 'Failed to execute long running script',
    errorMessage: 'Error: {{ message }}'

    nodeRequestOptions: {
       logAudit: true,
       logTelemetry: true
    }
};

return this.appContextService.workItem.submit('{!TargetNode}', workItem);
```

>[!NOTE] 
> 要显示进度，必须在编写的脚本中包括写入进度。 例如：
> ``` ps1
>  Write-Progress -Activity ‘The script is almost done!' -percentComplete 95
>```

#### <a name="workitem-options"></a>工作项选项 ####

| function | 说明 |
| ----- | ----------- |
| submit （） | 提交工作项 
| submitAndWait() | 提交工作项并等待其执行完成
| wait （） | 等待现有工作项完成
| query （） | 按 id 查询现有工作项
| find （） | 按 TargetNodeName、ModuleName 或 typeId 查找和现有工作项。

### <a name="powershell-batch-apis"></a>PowerShell 批处理 Api ###
如果需要在多个节点上运行同一个脚本，则可以使用批处理 PowerShell 会话。 例如：
```ts
const batchSession = this.appContextService.powerShell.createBatchSession(
    ['{!TargetNode1}', '{!TargetNode2}', sessionKey);
  this.appContextService.powerShell.runBatchSingleCommand(batchSession, command).subscribe((responses: PowerShellBatchResponseItem[]) => {
    for (const response of responses) { 
      if (response.error || response.errors) {
        //handle error
      } else {
        const results = response.properties && response.properties.results;
        //response.nodeName
        //results[0]
      }
    }
     },
     Error => { /* handle error */ });  

```


#### <a name="powershellbatch-options"></a>PowerShellBatch 选项 ####
| 选 | 说明 |
| ----- | ----------- |
| runSingleCommand | 对数组中的所有节点运行单个命令 
| 用 | 在配对节点上运行相应的命令
| 取消 | 在数组中的所有节点上取消该命令
