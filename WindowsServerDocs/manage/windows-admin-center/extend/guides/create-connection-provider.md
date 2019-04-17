---
title: 创建的连接提供程序的解决方案扩展
description: '制定解决方案扩展 Windows Admin Center SDK (Project Honolulu): 创建连接提供程序'
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 883fba96fcb71cb1c6e8162c1564d66924c4e24d
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081134"
---
# 创建的连接提供程序的解决方案扩展

>适用于：Windows Admin Center、Windows Admin Center 预览版

连接提供商扮演重要角色在 Windows Admin Center 如何定义和与可连接的对象或目标通信。 主要，连接提供程序将在进行连接，如确保该目标联机并且可用，并确保连接的用户有权访问目标时执行操作。

默认情况下，Windows Admin Center 附带下列连接提供程序：

* Server
* Windows 客户端
* 故障转移群集
* HCI 群集

若要创建自己的自定义连接提供程序，请按照下列步骤：

* 添加连接提供程序的详细信息 ```manifest.json```
* 定义连接状态提供程序
* 在应用程序层实现连接提供程序

## 向 manifest.json 添加连接提供程序详细信息

现在，我们将演练你需要知道你的项目中定义连接提供程序```manifest.json```文件。

### Manifest.json 中创建条目

```manifest.json```文件位于 \src 文件夹，并包含，除了其他方面之外，定义的入口点到你的项目。 入口点的类型包括工具、 解决方案和连接提供程序。 我们将定义连接提供程序。

下面是在 manifest.json 的连接提供程序条目的示例：

``` json
    {
      "entryPointType": "connectionProvider",
      "name": "addServer",
      "path": "/add",
      "displayName": "resources:strings:addServer_displayName",
      "icon": "sme-icon:icon-win-server",
      "description": "resources:strings:description",
      "connectionType": "msft.sme.connection-type.server",
      "connectionTypeName": "resources:strings:addServer_connectionTypeName",
      "connectionTypeUrlName": "server",
      "connectionTypeDefaultSolution": "msft.sme.server-manager!servers",
      "connectionTypeDefaultTool": "msft.sme.server-manager!overview",
      "connectionStatusProvider": {
        "powerShell": {
          "script": "## Get-My-Status ##\nfunction Get-Status()\n{\n# A function like this would be where logic would exist to identify if a node is connectable.\n$status = @{label = $null; type = 0; details = $null; }\n$caption = \"MyConstCaption\"\n$productType = \"MyProductType\"\n# A result object needs to conform to the following object structure to be interpreted properly by the Windows Admin Center shell.\n$result = @{ status = $status; caption = $caption; productType = $productType; version = $version }\n# DO FANCY LOGIC #\n# Once the logic is complete, the following fields need to be populated:\n$status.label = \"Display Thing\"\n$status.type = 0 # This value needs to conform to the LiveConnectionStatusType enum. >= 3 represents a failure.\n$status.details = \"success stuff\"\nreturn $result}\nGet-Status"
        },
        "displayValueMap": {
          "wmfMissing-label": "resources:strings:addServer_status_wmfMissing_label",
          "wmfMissing-details": "resources:strings:addServer_status_wmfMissing_details",
          "unsupported-label": "resources:strings:addServer_status_unsupported_label",
          "unsupported-details": "resources:strings:addServer_status_unsupported_details"
        }
      }
    },
```

类型"connnectionProvider"入口点指示到 Windows Admin Center shell 正在配置的项的提供程序的解决方案将用于验证连接状态。 连接提供程序的入口点包含了大量的重要的属性，定义如下：

| 属性 | 说明 |
| -------- | ----------- |
| entryPointType | 这是必需的属性。 有三个有效的值:"工具"、"解决方案"和"一样"。 | 
| name | 标识解决方案的范围内的连接提供。 此值必须是唯一一个完整的 Windows Admin Center 实例 （而不只是一个解决方案） 内。 |
| path | "添加连接"用户界面，表示的 URL 路径，如果它将配置的解决方案。 此值必须映射到应用 routing.module.ts 文件中配置的路线。 配置的解决方案入口点时要使用连接 rootNavigationBehavior，该路由将加载 Shell 用来显示在添加连接 UI 的模块。 有关详细信息提供的部分中 rootNavigationBehavior。 |
| 显示名称 | 此处输入的值将显示在 shell，当用户加载解决方案的连接页面的黑色 Windows Admin Center 栏下方的右侧。 |
| icon | 表示在解决方案下拉菜单中用于表示该解决方案的图标。 |
| description | 输入的入口点的简短描述。 |
| 连接 | 表示该提供商将加载的连接类型。 此处输入的值将还用于在解决方案入口点指定解决方案可以加载这些连接。 此处输入的值将还用于在工具入口点指示该工具是与此类型兼容。 此外将在提交到 RPC 连接对象使用此处输入此值在"添加窗口"，在应用程序层实现步骤中调用。 |
| connectionTypeName | 连接表中用来表示使用你的连接提供程序的连接。 这应会将以复数类型的名称。 |
| connectionTypeUrlName | 创建 URL 中用于表示加载的解决方案之后 Windows Admin Center 已连接到实例。 后连接，并在目标之前，会使用此项。 在此示例中，"connectionexample"为此值在 URL 中显示的位置：http://localhost:6516/solutionexample/connections/connectionexample/con-fake1.corp.contoso.com |
| connectionTypeDefaultSolution | 表示应加载连接提供程序的默认分量。 此值是的组合: [] 定义顶部的清单; 扩展包的名称[b] 感叹号 （！）;[c] 解决方案入口点名称。    此值将是"msft.sme.mySample-扩展名"，一个项目和解决方案的入口点名称"example"，"msft.sme.solutionExample 扩展 ！ 示例"。 |
| connectionTypeDefaultTool | 表示默认应加载成功的连接的工具。 此属性值是两个部分，类似于 connectionTypeDefaultSolution 组成。 此值是的组合: [] 定义顶部的清单; 扩展包的名称[b] 感叹号 （！）;[c] 工具入口点应最初加载的工具的名称。 此值将是"msft.sme.solutionExample-扩展名"，一个项目和解决方案的入口点名称"example"，"msft.sme.solutionExample 扩展 ！ 示例"。 |
| connectionStatusProvider | 请参阅"定义连接状态提供程序"部分 |

## 定义连接状态提供程序

连接状态提供程序的机制的目标会验证联机并且可用，也可以确保连接的用户有权访问目标。 当前有两种类型的连接状态提供程序： PowerShell 和 RelativeGatewayUrl。

*   PowerShell 连接状态提供程序
    *   确定目标是否联机和可访问的 PowerShell 脚本。 必须在使用下面定义的单个属性"status"对象中返回的结果。
*   RelativeGatewayUrl 连接状态提供程序
    *   确定目标是否联机和可访问的 rest 调用。 必须在使用下面定义的单个属性"status"对象中返回的结果。

### 定义状态

连接状态提供程序所需返回具有单个属性的对象的```status```符合以下格式：

``` json
{
    status: {
        label: string;
        type: int;
        details: string;
    }
}
```

状态属性：

* 标签
    * 标签描述状态返回类型。 请注意，可以在运行时中映射值标签。 请参阅下面的运行时中的映射值的条目。

* 类型
    * 状态返回类型。 类型都具有以下枚举值。 任何值 2 或更高，平台将不导航到已连接的对象，并将在 UI 中显示错误。

类型：

| 值 | 说明 |
| ----- | ----------- |
| 0 | Online |
| 1 | 警告 |
| 2 | 未授权 |
| 3 | 错误 |
| 4 | 严重 |
| 5 | Unknown |

* 详细信息
    * 描述状态的其他详细信息的返回类型。

### PowerShell 连接状态提供程序脚本

连接状态提供程序 PowerShell 脚本确定目标是否联机和可访问的 PowerShell 脚本。 必须在具有单个属性"status"的对象中返回的结果。 一个示例脚本，如下所示。

示例 PowerShell 脚本：

``` ts
## Get-My-Status ##

function Get-Status()
{
    # A function like this would be where logic would exist to identify if a node is connectable.
    $status = @{label = $null; type = 0; details = $null; }
    $caption = "MyConstCaption"
    $productType = "MyProductType"

    # A result object needs to conform to the following object structure to be interperated properly by the Windows Admin Center shell.
    $result = @{ status = $status; caption = $caption; productType = $productType; version = $version }

    # DO FANCY LOGIC #

    # Once the logic is complete, the following fields need to be populated:
    $status.label = "Display Thing"
    $status.type = 0 # This value needs to conform to the LiveConnectionStatusType enum. >= 3 represents a failure.
    $status.details = "success stuff"

    return $result
}

Get-Status
```

### 定义 RelativeGatewayUrl 连接状态提供程序方法

连接状态提供程序```RelativeGatewayUrl```方法调用 rest API，以确定目标是否联机和可访问。 必须在具有单个属性"status"的对象中返回的结果。 如下所示的 RelativeGatewayUrl 示例 manifest.json 中的连接提供程序条目。

``` json
    {
      "entryPointType": "connectionProvider",
      "name": "addServer",
      "path": "/add/server",
      "displayName": "resources:strings:addServer_displayName",
      "icon": "sme-icon:icon-win-server",
      "description": "resources:strings:description",
      "connectionType": "msft.sme.connection-type.server",
      "connectionTypeName": "resources:strings:addServer_connectionTypeName",
      "connectionTypeUrlName": "server",
      "connectionTypeDefaultSolution": "msft.sme.server-manager!servers",
      "connectionTypeDefaultTool": "msft.sme.server-manager!overview",
      "connectionStatusProvider": {
        "relativeGatewayUrl": "<URL here post /api>",
        "displayValueMap": {
          "wmfMissing-label": "resources:strings:addServer_status_wmfMissing_label",
          "wmfMissing-details": "resources:strings:addServer_status_wmfMissing_details",
          "unsupported-label": "resources:strings:addServer_status_unsupported_label",
          "unsupported-details": "resources:strings:addServer_status_unsupported_details"
        }
      }
    },
```

有关使用 RelativeGatewayUrl 注意事项：

* "relativeGatewayUrl"指定从网关 URL 获取连接状态的位置。 此 URI 是从 /api 相对的。 如果 $connectionName URL 中找到，它将替换连接的名称。
* 必须对主机网关，这可以通过创建一个网关扩展执行所有 relativeGatewayUrl 属性

### 在运行时中的值映射

返回的对象可以进行格式化处的状态中的标签和详细信息值调整通过提供程序的"defaultValueMap"属性中包含项和值的时间。

例如，如果你添加下面的值，任何时间的"defaultConnection_test"显示作为值标签或详细信息，Windows Admin Center 将自动替换该密钥的配置的资源字符串值。

``` json
    "defaultConnection_test": "resources:strings:addServer_status_defaultConnection_label"
```

## 在应用程序层实现连接提供程序

现在，我们将通过创建一个实现 OnInit TypeScript 类在应用程序层中，实现连接提供程序。 该类具有以下功能：

| 函数 | 说明 |
| -------- | ----------- |
| 构造函数 (专用 appContextService: AppContextService，专用路由： ActivatedRoute) |  |
| 公共 ngOnInit() |  |
| 公共 onSubmit() | 包含逻辑时添加连接尝试更新 shell |
| 公共 onCancel() | 包含用于更新 shell 取消添加连接尝试时逻辑 |

### 定义 onSubmit

```onSubmit``` 问题 RPC 回调到要通知的"添加连接"shell 的应用上下文中。 基本的调用使用"updateData"如下：

``` ts
this.appContextService.rpc.updateData(
    EnvironmentModule.nameOfShell,
    '##',
    <RpcUpdateData>{
        results: {
            connections: connections,
            credentials: this.useCredentials ? this.creds : null
        }
    }
);
```

结果是连接属性，这是符合以下结构的对象数组：

``` ts

/**
 * The connection attributes class.
 */
export interface ConnectionAttribute {

    /**
     * The id string of this attribute
     */
    id: string;

    /**
     * The value of the attribute. used for attributes that can have variable values such as Operating System
     */
    value?: string | number;
}

/**
 * The connection class.
 */
export interface Connection {

    /**
     * The id of the connection, this is unique per connection
     */
    id: string;

    /**
     * The type of connection
     */
    type: string;

    /**
     * The name of the connection, this is unique per connection type
     */
    name: string;

    /**
     * The property bag of the connection
     */
    properties?: ConnectionProperties;

    /**
     * The ids of attributes identified for this connection
     */
    attributes?: ConnectionAttribute[];

    /**
     * The tags the user(s) have assigned to this connection
     */
    tags?: string[];
}

/**
 * Defines connection type strings known by core
 * Be careful that these strings match what is defined by the manifest of @msft-sme/server-manager
 */
export const connectionTypeConstants = {
    server: 'msft.sme.connection-type.server',
    cluster: 'msft.sme.connection-type.cluster',
    hyperConvergedCluster: 'msft.sme.connection-type.hyper-converged-cluster',
    windowsClient: 'msft.sme.connection-type.windows-client',
    clusterNodesProperty: 'nodes'
};
```

### 定义 onCancel

```onCancel``` 取消"添加连接"尝试通过传递空连接数组：

``` ts
this.appContextService.rpc.updateData(EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
```

## 连接提供程序示例

下面是实现连接提供程序的完整 TypeScript 类。 请注意"连接"字符串匹配"连接 manifest.json 中的连接提供程序中定义。

``` ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { AppContextService } from '@msft-sme/shell/angular';
import { Connection, ConnectionUtility } from '@msft-sme/shell/core';
import { EnvironmentModule } from '@msft-sme/shell/dist/core/manifest/environment-modules';
import { RpcUpdateData } from '@msft-sme/shell/dist/core/rpc/rpc-base';
import { Strings } from '../../generated/strings';

@Component({
  selector: 'add-example',
  templateUrl: './add-example.component.html',
  styleUrls: ['./add-example.component.css']
})
export class AddExampleComponent implements OnInit {
  public newConnectionName: string;
  public strings = MsftSme.resourcesStrings<Strings>().SolutionExample;
  private connectionType = 'msft.sme.connection-type.example'; // This needs to match the connectionTypes value used in the manifest.json.
  
  constructor(private appContextService: AppContextService, private route: ActivatedRoute) {
    // TODO:
  }

  public ngOnInit() {
    // TODO
  }

  public onSubmit() {
    let connections: Connection[] = [];

    let connection = <Connection> {
      id: ConnectionUtility.createConnectionId(this.connectionType, this.newConnectionName),
      type: this.connectionType,
      name: this.newConnectionName
    };

    connections.push(connection);

    this.appContextService.rpc.updateData(
      EnvironmentModule.nameOfShell,
      '##',
      <RpcUpdateData> {
        results: {
          connections: connections,
          credentials: null
        }
      }
    );
  }

  public onCancel() {
    this.appContextService.rpc.updateData(
      EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
  }
}

```
