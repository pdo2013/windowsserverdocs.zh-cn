---
title: 在扩展中使用 PowerShell
description: 在扩展 Windows Admin Center SDK (Project Honolulu) 中使用 PowerShell
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b1be4fe7639d913243cc28371dff9e98e0f5827e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296719"
---
# 在扩展中使用 PowerShell #

>适用于：Windows Admin Center、Windows Admin Center 预览版

让我们更深入地了解到 Windows Admin Center 扩展 SDK-我们来讨论一下将 PowerShell 命令添加到你的扩展。

## TypeScript 中的 PowerShell ##

Gulp 生成过程已采取任何一个生成步骤```{!ScriptName}.ps1```放置在```\src\resources\scripts```文件夹并将它们到生成```powershell-scripts```下类```\src\generated```文件夹。

>[!NOTE] 
> 不要手动更新```powershell-scripts.ts```和```strings.ts```文件。 你所做的任何更改将在下一步生成被覆盖。

## 运行 Powerhell 脚本 ##
任何你想要在节点上运行的脚本可以放置在```\src\resources\scripts\{!ScriptName}.ps1```。 
>[!IMPORTANT] 
> 在进行的任何更改```{!ScriptName}.ps1```文件将不会反映在你的项目除非生成 

该 API 的工作原理是第一个是面向、 使用任何需要在中，传递的参数创建 PowerShell 脚本，然后已创建的会话上运行该脚本在节点上创建的 PowerShell 会话。

例如，我们有此脚本```\src\resources\scripts\Get-NodeName.ps1```:
``` ps1
Param
 (
    [String] $stringFormat
 )
 $nodeName = [string]::Format($stringFormat,$env:COMPUTERNAME)
 Write-Output $nodeName
```

我们将创建我们目标节点的 PowerShell 会话：
``` ts
const session = this.appContextService.powerShell.createSession('{!TargetNode}'); 
```
然后我们将使用一个输入参数创建 PowerShell 脚本：
```ts
const script = PowerShell.createScript(PowerShellScripts.Get_NodeName, {stringFormat: 'The name of the node is {0}!'});
```
最后，我们需要在我们创建的会话中运行该脚本：
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
现在我们将需要订阅刚创建的可观测函数。 放入此，你需要调用函数以运行 PowerShell 脚本：
```ts
this.getNodeName().subscribe(
     response => {
    console.log(response)
     }
);
```
通过提供 createSession 方法的节点名称，新的 PowerShell 会话是创建、 使用并立即销毁 PowerShell 调用完成时。 

### 键选项 ###
调用 PowerShell API 时，可以使用几个选项。 每次创建会话时可以创建具有或没有密钥。 

**密钥：** 这将创建一个键控的会话进行查找和重复使用，甚至是在组件 （意味着 Component1 可以创建与密钥"SME-岩石，"的会话和 Component2 可以使用该同一会话）。如果提供了一个键，创建的会话必须丢弃通过调用 dispose （） 为已完成在上面的示例。 会话应不会保持不释放超过 5 分钟。 
```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNode}', '{!Key}');
```

**键：** 将自动为会话创建一个密钥。 此会话与 3 分钟后自动丢弃。 使用键允许你回收任何运行空间，则可在创建会话时使用的扩展。 如果没有运行空间是将创建一个新的可用。 此功能适用于一次性调用，但是重复的使用可能会影响性能。 会话 （采用大约 1 秒钟的时间创建），因此不断重新启动会话可能会导致速度下降。

```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNodeName}');
```
或 
``` ts 
const session = this.appContextService.powerShell.createAutomaticSession('{!TargetNodeName}');
```
在大多数情况下，创建中的键控的会话```ngOnInit()```方法，并在处理然后```ngOnDestroy()```。 当存在多个 PowerShell 脚本组件而不是在组件之间共享基础会话时，请遵循此模式。
为获得最佳结果，确保会话创建托管内部组件，而不是服务-这有助于确保该生命周期，可正确管理清理。

为获得最佳结果，确保会话创建托管内部组件，而不是服务-这有助于确保该生命周期，可正确管理清理。

### PowerShell 流 ###
如果你有一个长时间运行的脚本和数据被逐步，输出 PowerShell 流将允许你处理的数据，而无需等待完成脚本。 只要收到的数据时，将调用可观测 next （）。
```ts
this.appContextService.powerShellStream.run(session, script);
```

### 多长时间运行脚本 ###
如果你有一个长时间运行的脚本，你想要在后台运行，可以提交工作项。 将由网关跟踪脚本的状态并更新状态可以发送到通知。 
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
> 若要显示的进度，写入进度必须包括在你编写的脚本。 例如：
> ``` ps1
>  Write-Progress -Activity ‘The script is almost done!’ -percentComplete 95
>```

#### 工作项选项 ####

| function | 说明 |
| ----- | ----------- |
| submit() | 提交工作项 
| submitAndWait() | 提交工作项并等待完成其执行
| wait() | 等待现有的工作项完成
| query （) | 查询的现有工作项的 id
| find() | 查找和现有的工作项 TargetNodeName、 ModuleName，或类型 Id。

### PowerShell 批处理 Api # # #
如果你需要在多个节点上运行相同的脚本，则可以使用批处理 PowerShell 会话。 例如：
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


#### PowerShellBatch 选项 ####
| 选项 | 说明 |
| ----- | ----------- |
| runSingleCommand | 对数组中的所有节点运行单个命令 
| 运行 | 在配对的节点上运行对应的命令
| 取消 | 取消数组中的所有节点上的命令