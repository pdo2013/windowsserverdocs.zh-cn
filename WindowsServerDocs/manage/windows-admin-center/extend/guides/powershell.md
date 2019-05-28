---
title: 在扩展中使用 PowerShell
description: 在扩展 Windows Admin Center SDK (项目 Honolulu) 中使用 PowerShell
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 05/09/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 7375732fd464519cd1533043d271065e488fd46a
ms.sourcegitcommit: 7cb939320fa2613b7582163a19727d7b77debe4b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2019
ms.locfileid: "65621356"
---
# <a name="using-powershell-in-your-extension"></a>在扩展中使用 PowerShell #

>适用于：Windows Admin Center，Windows Admin Center 预览版

让我们更深入到 Windows Admin Center 扩展 SDK-让我们讨论一下将 PowerShell 命令添加到你的扩展。

## <a name="powershell-in-typescript"></a>在 TypeScript 中的 PowerShell ##

Gulp 生成过程有任何需要的生成步骤```{!ScriptName}.ps1```，将置于```\src\resources\scripts```文件夹并生成其```powershell-scripts```类下```\src\generated```文件夹。

>[!NOTE] 
> 不手动更新```powershell-scripts.ts```也不```strings.ts```文件。 将在下一步生成覆盖所做的任何更改。

## <a name="running-a-powershell-script"></a>运行 PowerShell 脚本 ##
你想要在节点上运行任何脚本可以放入```\src\resources\scripts\{!ScriptName}.ps1```。 
>[!IMPORTANT] 
> 在中进行任何更改```{!ScriptName}.ps1```文件将不会反映在你的项目之前```gulp generate```已运行。

API 通过第一个目标、 使用任何需要在中，传递的参数创建 PowerShell 脚本，然后创建的会话上运行该脚本在节点上创建的 PowerShell 会话的工作原理。

例如，我们有此脚本```\src\resources\scripts\Get-NodeName.ps1```:
``` ps1
Param
 (
    [String] $stringFormat
 )
 $nodeName = [string]::Format($stringFormat,$env:COMPUTERNAME)
 Write-Output $nodeName
```

我们将创建我们的目标节点的 PowerShell 会话：
``` ts
const session = this.appContextService.powerShell.createSession('{!TargetNode}'); 
```
然后我们将创建具有一个输入参数的 PowerShell 脚本：
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
现在我们将需要订阅到可观察我们刚刚创建的函数。 放入此，需要调用该函数来运行 PowerShell 脚本：
```ts
this.getNodeName().subscribe(
     response => {
    console.log(response)
     }
);
```
通过提供到 createSession 方法的节点名称，新的 PowerShell 会话创建、 使用，并立即销毁 PowerShell 调用完成后。 

### <a name="key-options"></a>密钥选项 ###
调用 PowerShell API 时提供了几个选项。 每次创建会话时它可以创建带有或不带密钥。 

**密钥：** 这将创建可以查找和重复使用，即使跨组件 （即 Component1 可以具有键"SME-ROCKS，"创建会话和 Component2 可以使用该会话） 的加密的会话。如果提供一个键，则创建该会话必须释放的由调用 dispose （） 就像在上面的示例。 会话不应保留而未释放的时间超过 5 分钟。 
```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNode}', '{!Key}');
```

**无密钥：** 将自动为会话创建一个密钥。 3 分钟后自动释放与此会话。 使用无密钥允许您回收使用在创建会话时已提供任何运行空间的扩展。 如果不是将创建一个新，则不运行空间不可用。 此功能非常适合进行一次性调用，但重复的使用可能会影响性能。 会话需要大约 1 秒钟，若要创建，因此持续回收会话可能会导致速度变慢。

```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNodeName}');
```
或 
``` ts 
const session = this.appContextService.powerShell.createAutomaticSession('{!TargetNodeName}');
```
在大多数情况下，创建中的加密的会话```ngOnInit()```方法，然后释放它在```ngOnDestroy()```。 有一个组件，但不是在组件之间共享基础会话中的多个 PowerShell 脚本时，请遵循此模式。
为获得最佳结果，请确保会话创建内部组件而不是服务管理-这有助于确保该生存期和清理可以正确管理。

为获得最佳结果，请确保会话创建内部组件而不是服务管理-这有助于确保该生存期和清理可以正确管理。

### <a name="powershell-stream"></a>PowerShell Stream ###
如果你有长时间运行的脚本和数据输出渐进式，PowerShell 流便可以处理的数据，而无需等待要完成的脚本。 只要收到数据，将调用可观察的 next （）。
```ts
this.appContextService.powerShellStream.run(session, script);
```

### <a name="long-running-scripts"></a>长时间运行的脚本 ###
如果您具有想要在后台运行的长时间运行脚本，可提交工作项。 将网关跟踪脚本的状态和状态的更新可以发送通知。 
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
> 对于要显示的过程，请写入进度必须包括在已编写脚本。 例如：
> ``` ps1
>  Write-Progress -Activity ‘The script is almost done!’ -percentComplete 95
>```

#### <a name="workitem-options"></a>工作项选项 ####

| function | 说明 |
| ----- | ----------- |
| submit() | 提交工作项 
| submitAndWait() | 将工作项提交并等待其执行完成
| wait() | 等待现有的工作项完成
| query() | 按 id 的现有工作项查询
| find() | 查找和现有工作项通过 TargetNodeName、 模块名称或 typeId。

### <a name="powershell-batch-apis"></a>PowerShell Batch Api ###
如果需要多个节点上运行同一个脚本，则可以使用批处理 PowerShell 会话。 例如：
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
| 选项 | 说明 |
| ----- | ----------- |
| runSingleCommand | 对数组中的所有节点运行单个命令 
| 运行 | 在配对节点上运行相应的命令
| 取消 | 取消该数组中的所有节点上的命令
