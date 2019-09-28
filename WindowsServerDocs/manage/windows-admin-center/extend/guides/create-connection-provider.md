---
title: 为解决方案扩展创建连接提供程序
description: 开发解决方案扩展 Windows 管理中心 SDK （项目 Honolulu）-创建连接提供程序
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 9c04db3196d1e806e50af9164b3c8bcdfb19b079
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406883"
---
# <a name="create-a-connection-provider-for-a-solution-extension"></a>为解决方案扩展创建连接提供程序

>适用于：Windows Admin Center、Windows Admin Center 预览版

连接提供程序在 Windows 管理中心中心如何定义可连接对象或目标之间扮演着重要的角色。 连接提供程序主要是在建立连接时执行操作，如确保目标处于联机状态且可用，同时确保连接用户有权访问目标。

默认情况下，Windows 管理中心附带以下连接提供程序：

* Server
* Windows 客户端
* 故障转移群集
* HCI 群集

若要创建自己的自定义连接提供程序，请执行以下步骤：

* 将连接提供程序详细信息添加到```manifest.json```
* 定义连接状态提供程序
* 在应用程序层中实现连接提供程序

## <a name="add-connection-provider-details-to-manifestjson"></a>向 manifest 添加连接提供程序详细信息

接下来，我们将逐步介绍在项目```manifest.json```文件中定义连接提供程序所需了解的内容。

### <a name="create-entry-in-manifestjson"></a>在 manifest 中创建项

此```manifest.json```文件位于 \src 文件夹中，并包含入口点在项目中的定义。 入口点的类型包括工具、解决方案和连接提供程序。 我们将定义一个连接提供程序。

清单中的连接提供程序条目的示例如下所示：

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

类型为 "connnectionProvider" 的入口点向 Windows 管理中心 shell 指示正在配置的项是一个将由解决方案用于验证连接状态的提供程序。 连接提供程序入口点包含若干重要属性，定义如下：

| 属性 | 描述 |
| -------- | ----------- |
| EntryPointType | 这是一个必需的属性。 有三个有效值： "工具"、"解决方案" 和 "connectionProvider"。 | 
| name | 标识解决方案范围内的连接提供程序。 此值在完整的 Windows 管理中心实例（而不仅仅是解决方案）中必须是唯一的。 |
| path | 表示 "添加连接" UI 的 URL 路径（如果它将由解决方案配置）。 此值必须映射到在 app.config 文件中配置的路由。 当解决方案入口点配置为使用连接 rootNavigationBehavior 时，此路由将加载 Shell 用来显示 "添加连接" UI 的模块。 有关详细信息，请访问 rootNavigationBehavior 上的部分。 |
| displayName | 此处输入的值将显示在 shell 的右侧，在用户加载解决方案的 "连接" 页时，位于黑色 Windows 管理中心栏的下方。 |
| 图标 | 表示解决方案下拉菜单中用于表示解决方案的图标。 |
| description | 输入入口点的简短说明。 |
| connectionType | 表示提供程序将加载的连接类型。 此处输入的值还将用于解决方案入口点，以指定解决方案可加载这些连接。 此处输入的值还将用于工具入口点，以指示该工具与此类型兼容。 此处输入的值还将用于在应用程序层实现步骤中提交给 RPC 调用的连接对象。 |
| connectionTypeName | 用于连接表，用于表示使用连接提供程序的连接。 此名称应为类型的复数名称。 |
| connectionTypeUrlName | 在 Windows 管理中心连接到实例之后，用于创建 URL 以表示已加载的解决方案。 此项在连接之后、在目标之前使用。 在此示例中，"connectionexample" 是此值出现在 URL 中的位置：`http://localhost:6516/solutionexample/connections/connectionexample/con-fake1.corp.contoso.com` |
| connectionTypeDefaultSolution | 表示连接提供程序应加载的默认组件。 此值由以下内容组成： <br>[a] 在清单顶部定义的扩展包的名称; <br>[b] 惊叹号（！）; <br>[c] 解决方案入口点名称。    <br>对于名为 "mySample" 的项目和名称为 "example" 的解决方案入口点，此值将为 "solutionExample-extension！ example"。 |
| connectionTypeDefaultTool | 表示应在成功连接时加载的默认工具。 此属性值由两部分组成，类似于 connectionTypeDefaultSolution。 此值由以下内容组成： <br>[a] 在清单顶部定义的扩展包的名称; <br>[b] 惊叹号（！）; <br>[c] 应最初加载的工具的工具入口点名称。 <br>对于名为 "solutionExample" 的项目和名称为 "example" 的解决方案入口点，此值将为 "solutionExample-extension！ example"。 |
| connectionStatusProvider | 请参阅 "定义连接状态提供程序" 部分 |

## <a name="define-connection-status-provider"></a>定义连接状态提供程序

连接状态提供程序是一种机制，通过该机制可以验证目标是否处于联机状态并且可用，同时确保连接用户有权访问目标。 目前有两种类型的连接状态提供程序：PowerShell 和 RelativeGatewayUrl。

*   <strong>Powershell 连接状态提供程序</strong>-确定目标是否处于联机状态并且可通过 PowerShell 脚本访问。 结果必须在具有以下定义的单个属性 "status" 的对象中返回。
*   <strong>RelativeGatewayUrl 连接状态提供程序</strong>-确定目标是否处于联机状态，并可通过 rest 调用访问。 结果必须在具有以下定义的单个属性 "status" 的对象中返回。

### <a name="define-status"></a>定义状态

连接状态提供程序需要返回具有单个属性```status```且符合以下格式的对象：

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

* <strong>标签</strong>-描述状态返回类型的标签。 请注意，可在运行时中映射标签的值。 请参阅下面的条目以获取运行时中的映射值。

* <strong>类型</strong>-状态返回类型。 类型具有以下枚举值。 对于任何值2或更高版本，平台将不导航到连接的对象，并且会在 UI 中显示错误。

   各种

  | ReplTest1 | Description |
  | ----- | ----------- |
  | 0 | 联机 |
  | 1 | 警告 |
  | 2 | 未经授权 |
  | 3 | Error |
  | 4 | 出现 |
  | 5 | Unknown |

* <strong>详细信息</strong>-描述状态返回类型的其他详细信息。

### <a name="powershell-connection-status-provider-script"></a>PowerShell 连接状态提供程序脚本

连接状态提供程序 PowerShell 脚本确定目标是否处于联机状态，并可通过 PowerShell 脚本进行访问。 必须在具有单个属性 "status" 的对象中返回结果。 下面显示了一个示例脚本。

PowerShell 脚本示例：

```PowerShell
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

连接状态提供程序```RelativeGatewayUrl```方法调用 rest API，以确定目标是否处于联机状态并且可访问。 必须在具有单个属性 "status" 的对象中返回结果。 下面显示了 RelativeGatewayUrl 的示例连接提供程序条目。

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

有关使用 RelativeGatewayUrl 的说明：

* "relativeGatewayUrl" 指定从网关 URL 获取连接状态的位置。 此 URI 相对于/api。 如果在 URL 中找到 $connectionName，则会将其替换为连接的名称。
* 必须针对主机网关执行所有 relativeGatewayUrl 属性，这可以通过创建网关扩展来完成

### <a name="map-values-in-runtime"></a>在运行时中映射值

通过在提供程序的 "defaultValueMap" 属性中包含键和值，可以在微调时间设置状态返回对象中的标签和详细信息值的格式。

例如，如果添加以下值，则任何 "defaultConnection_test" 显示为 "标签" 或 "详细信息" 的值的时间，Windows 管理中心都将自动用配置的资源字符串值替换该密钥。

``` json
    "defaultConnection_test": "resources:strings:addServer_status_defaultConnection_label"
```

## <a name="implement-connection-provider-in-application-layer"></a>在应用程序层中实现连接提供程序

现在，我们将通过创建实现 OnInit 的 TypeScript 类来实现应用程序层中的连接提供程序。 类具有以下功能：

| Functions | 描述 |
| -------- | ----------- |
| 构造函数（private appContextService：AppContextService，专用路由：ActivatedRoute) |  |
| public ngOnInit （） |  |
| public onSubmit （） | 在进行添加连接尝试时包含用于更新 shell 的逻辑 |
| public onCancel （） | 在取消添加连接尝试时包含用于更新 shell 的逻辑 |

### <a name="define-onsubmit"></a>定义 onSubmit

```onSubmit```向应用上下文发出 RPC 回调，以通知 shell 出现 "添加连接"。 基本调用使用如下所示的 "updateData"：

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

结果就是一个连接属性，它是一个对象的数组，这些对象符合以下结构：

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

```onCancel```通过传递空的连接数组来取消 "添加连接" 尝试：

``` ts
this.appContextService.rpc.updateData(EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
```

## <a name="connection-provider-example"></a>连接提供程序示例

用于实现连接提供程序的完整 TypeScript 类如下所示。 请注意，"connectionType" 字符串与 connectionType 中连接提供程序中定义的 "" 匹配。

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
