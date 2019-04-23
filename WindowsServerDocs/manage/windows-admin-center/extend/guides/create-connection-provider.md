---
title: 创建解决方案扩展的连接提供程序
description: 开发解决方案扩展 Windows Admin Center SDK (项目 Honolulu)-创建连接提供程序
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 883fba96fcb71cb1c6e8162c1564d66924c4e24d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885648"
---
# <a name="create-a-connection-provider-for-a-solution-extension"></a>创建解决方案扩展的连接提供程序

>适用于：Windows Admin Center，Windows Admin Center 预览版

连接提供程序发挥重要作用中 Windows Admin Center 如何定义和与可连接对象或多个目标进行通信。 首先，连接提供程序执行操作，在进行连接，如确保目标处于联机状态且可用，并确保连接的用户有权访问目标时。

默认情况下，Windows Admin Center 附带以下连接提供程序：

* Server
* Windows 客户端
* 故障转移群集
* HCI 群集

若要创建自己的自定义连接提供程序，请执行以下步骤：

* 添加到连接提供程序详细信息 ```manifest.json```
* 定义连接状态提供程序
* 在应用程序层中实现连接提供程序

## <a name="add-connection-provider-details-to-manifestjson"></a>将连接提供程序详细信息添加到 manifest.json

现在，我们将需要知道要在项目中定义的连接提供程序通过```manifest.json```文件。

### <a name="create-entry-in-manifestjson"></a>Manifest.json 中创建的条目

```manifest.json```文件 \src 文件夹中，除了别的之外，包含到你的项目的入口点定义。 入口点的类型包括工具、 解决方案和连接提供程序。 我们将定义连接提供程序。

在 manifest.json 连接提供程序条目的示例如下所示：

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

类型"connnectionProvider"的入口点指示 Windows Admin Center shell 正在配置的项，是一种解决方案将用于验证连接状态的提供程序。 连接提供程序入口点包含多个重要属性，定义如下：

| 属性 | 描述 |
| -------- | ----------- |
| entryPointType | 这是必需的属性。 有三个有效的值:"工具"、"解决方案"和"connectionProvider"。 | 
| name | 标识解决方案的作用域内的连接提供程序。 此值必须是完整的 Windows Admin Center 实例 （而不仅仅是解决方案） 中唯一的。 |
| path | 如果将解决方案配置"添加连接"用户界面，表示 URL 路径。 此值必须映射到应用 routing.module.ts 文件中配置的路由。 在解决方案入口点配置为使用连接 rootNavigationBehavior，此路由将加载的模块的命令行程序用来显示将添加连接用户界面。 有关详细信息可在部分 rootNavigationBehavior。 |
| displayName | 在 shell 中，当用户加载解决方案的连接页，黑色 Windows Admin Center 栏下方的右侧显示在此处输入的值。 |
| 图标 | 表示在解决方案下拉列表菜单中用于表示解决方案的图标。 |
| description | 输入的入口点的简短说明。 |
| connectionType | 表示提供程序将加载的连接类型。 此处输入的值还用于在解决方案入口点中指定该解决方案可以加载这些连接。 此处输入的值还用于在工具入口点中指示该工具是与此类型兼容。 在此处输入此值还将使用的连接对象中的提交到 RPC 调用"添加窗口"，在应用程序层实现步骤。 |
| connectionTypeName | 在连接表中用于表示使用连接提供程序的连接。 这被应为类型的复数名称。 |
| connectionTypeUrlName | 在创建该 URL 用于 Windows Admin Center 具有连接到实例后表示加载的解决方案。 后的连接，并在目标之前，会使用此项。 在此示例中，"connectionexample"为此值显示在 URL 中的位置： http://localhost:6516/solutionexample/connections/connectionexample/con-fake1.corp.contoso.com |
| connectionTypeDefaultSolution | 表示应加载的连接提供程序的默认组件。 此值是的组合: [a] 顶部的清单; 定义的扩展包的名称[b] 感叹号 （！）;[c] 解决方案入口点名称。    对于具有名称"msft.sme.mySample-扩展"，项目名称"示例"使用一个解决方案入口点，此值将为"msft.sme.solutionExample 扩展 ！ 示例"。 |
| connectionTypeDefaultTool | 表示默认值应成功连接时加载的工具。 此属性的值由两个部分，类似于 connectionTypeDefaultSolution 组成。 此值是的组合: [a] 顶部的清单; 定义的扩展包的名称[b] 感叹号 （！）;[最初应加载该工具 c] 工具入口点名称。 对于具有名称"msft.sme.solutionExample-扩展"，项目名称"示例"使用一个解决方案入口点，此值将为"msft.sme.solutionExample 扩展 ！ 示例"。 |
| connectionStatusProvider | 请参阅"定义连接状态提供程序"一节 |

## <a name="define-connection-status-provider"></a>定义连接状态提供程序

连接状态提供程序是依据目标进行验证以连接到网络并且可用，该机制还可确保连接的用户有权访问目标。 目前有两种类型的连接状态提供程序：PowerShell 和 RelativeGatewayUrl。

*   PowerShell 连接状态提供程序
    *   确定目标是否联机并可通过 PowerShell 脚本访问。 必须在具有下面定义的单个属性"status"的对象中返回的结果。
*   RelativeGatewayUrl 连接状态提供程序
    *   确定目标是否联机并可通过 rest 调用访问。 必须在具有下面定义的单个属性"status"的对象中返回的结果。

### <a name="define-status"></a>定义状态

连接状态提供程序所需返回具有单个属性的对象```status```符合以下格式：

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

* Label
    * 描述状态的标签的返回类型。 请注意，可以在运行时映射标签的值。 请参阅下面的运行时中的映射值的条目。

* 在任务栏的搜索框中键入
    * 状态返回类型。 类型具有以下枚举值。 任何值 2 或更高版本，该平台将导航到已连接的对象，并将在 UI 中显示错误。

类型：

| ReplTest1 | Description |
| ----- | ----------- |
| 0 | 联机 |
| 1 | 警告 |
| 2 | 未经授权 |
| 3 | 错误 |
| 4 | 严重 |
| 5 | Unknown |

* 详细信息
    * 描述状态的更多详细信息的返回类型。

### <a name="powershell-connection-status-provider-script"></a>PowerShell 连接状态提供程序脚本

连接状态提供程序 PowerShell 脚本确定目标是否联机并可通过 PowerShell 脚本访问。 必须在具有单个属性"status"的对象中返回的结果。 如下所示的示例脚本。

PowerShell 脚本示例：

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

### <a name="define-relativegatewayurl-connection-status-provider-method"></a>定义 RelativeGatewayUrl 连接状态提供程序方法

连接状态提供程序```RelativeGatewayUrl```方法调用 rest API 来确定目标是否联机且可访问。 必须在具有单个属性"status"的对象中返回的结果。 如下所示的 RelativeGatewayUrl 示例 manifest.json 中的连接提供程序条目。

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

有关使用 RelativeGatewayUrl 说明：

* "relativeGatewayUrl"指定从网关 URL 获取的连接状态的位置。 此 URI 是相对于/api。 如果在 URL 中找到 $connectionName，则它将替换连接的名称。
* 必须对主机网关，这可以通过创建网关扩展完成执行所有 relativeGatewayUrl 属性

### <a name="map-values-in-runtime"></a>运行时中的值映射

返回对象可在格式设置的状态中的标签和详细信息值通过提供程序的"defaultValueMap"属性中包括键和值来优化时间。

例如，如果添加下面的值，只要该"defaultConnection_test"显示作为的值的标签或详细信息，Windows Admin Center 自动将配置的资源字符串值替换为密钥。

``` json
    "defaultConnection_test": "resources:strings:addServer_status_defaultConnection_label"
```

## <a name="implement-connection-provider-in-application-layer"></a>在应用程序层中实现连接提供程序

现在，我们将通过创建实现 OnInit 的 TypeScript 类在应用程序层中实现连接提供程序。 类具有以下功能：

| 函数 | 描述 |
| -------- | ----------- |
| constructor(private appContextService:AppContextService，专用路由：ActivatedRoute) |  |
| public ngOnInit() |  |
| public onSubmit() | 包含逻辑，以便更新命令行程序时添加连接尝试 |
| public onCancel() | 包含逻辑，以便更新 shell 添加连接尝试被取消时 |

### <a name="define-onsubmit"></a>定义 onSubmit

```onSubmit``` 问题 RPC 回调到要通知的"添加连接"shell 的应用程序上下文。 基本的调用都将使用"updateData"像这样：

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

结果为连接属性，它是符合以下结构的对象的数组：

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

### <a name="define-oncancel"></a>定义 onCancel

```onCancel``` 取消的"添加连接"尝试通过传递数组为空的连接：

``` ts
this.appContextService.rpc.updateData(EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
```

## <a name="connection-provider-example"></a>连接提供程序示例

下面是实现连接提供程序的完整 TypeScript 类。 请注意"连接类型"字符串匹配"connectionType manifest.json 中的连接提供程序中定义。

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
