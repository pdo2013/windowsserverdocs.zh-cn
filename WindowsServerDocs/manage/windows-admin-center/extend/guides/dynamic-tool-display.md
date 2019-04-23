---
title: 控制在解决方案中的工具的可见性
description: 控制解决方案 Windows Admin Center SDK (项目 Honolulu) 中的工具的可见性
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f3f34b4c86854bfc55cf4b1b57a0fd3c2baf2ffc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839248"
---
# <a name="control-your-tools-visibility-in-a-solution"></a>控制在解决方案中的工具的可见性 #

>适用于：Windows Admin Center，Windows Admin Center 预览版

可能您想要排除 （或隐藏） 扩展或从可用工具列表中的工具。 例如，如果您的工具面向仅 Windows Server 2016 （不较旧版本），您可能不希望连接到 Windows Server 2012 R2 的服务器以查看所需的工具在所有的用户。 (假设用户体验-他们单击它，请等待加载，仅将收到一条消息，其功能不适用于其连接的工具。)您可以定义何时显示 （或隐藏） 该工具的 manifest.json 文件中你的功能。

## <a name="options-for-deciding-when-to-show-a-tool"></a>用于确定何时显示一个工具选项 ##

有三个不同的选项可用于确定是否显示，可为特定服务器或群集连接应为所需的工具。

* localhost
* 清单 （一个属性的数组）
* 脚本 (script)

### <a name="localhost"></a>LocalHost ###

条件对象的 localHost 属性或不包含一个布尔值，可以计算来推断如果连接的节点是本地主机 （Windows Admin Center 安装在同一计算机）。 通过将值传递给该属性，指示何时 （条件） 若要显示的工具。 例如，如果你只想用户实际上正在连接到本地主机时要显示的工具，设置如下：

``` json
"conditions": [
{
    "localhost": true
}]
```

或者，如果你只想您的工具时要显示的连接节点*不是*localhost:

``` json
"conditions": [
{
    "localhost": false
}]
```

此处是什么的配置设置的示例只显示一个工具时连接的节点不是 localhost:

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
        "msft.sme.server-manager!windowsClients"
        ],
        "connectionTypes": [
        "msft.sme.connection-type.windows-client"
        ],
        "conditions": [
        {
            "localhost": true
        }
        ]
    }
    ]
}
```

### <a name="inventory-properties"></a>清单属性 ###

SDK 包含一组预先特选的清单属性可用来生成条件以决定时所需的工具应可用。 库存数组中有九个不同的属性：

| 属性名 | 预期值类型 |
| ------------- | ------------------- |
| computerManufacturer | string |
| operatingSystemSKU | 数字 |
| operatingSystemVersion | version_string (例如："10.1.*") |
| productType | 数字 |
| clusterFqdn | string |
| isHyperVRoleInstalled | 布尔值 |
| isHyperVPowershellInstalled | 布尔值 |
| isManagementToolsAvailable | 布尔值 |
| isWmfInstalled | 布尔值 |

清单数组中的每个对象必须符合以下 json 结构：

``` json
"<property name>": {
    "type": "<expected type>",
    "operator": "<defined operator to use>",
    "value": "<expected value to evaluate using the operator>"
}
```

#### <a name="operator-values"></a>运算符值 ####

| 运算符 | 描述 |
| -------- | ----------- |
| gt | 大于 |
| ge | 大于或等于 |
| lt | 小于 |
| le | 小于或等于 |
| eq | 等于 |
| ne | 不等于 |
| 为 | 检查一个值，是否为 true |
| 非 | 检查一个值为 false |
| 包含 | 一个字符串中存在的项 |
| notContains | 一个字符串中不存在项 |

#### <a name="data-types"></a>数据类型 ####

Type 属性的可用选项包括：

| 在任务栏的搜索框中键入 | 描述 |
| ---- | ----------- |
| version | 版本号 (例如：10.1.*) |
| 数字 | 数字值 |
| string | 一个字符串值 |
| 布尔值 | true 或 false |

#### <a name="value-types"></a>值类型 ####

Value 属性接受这些类型：

* string
* 数字
* 布尔值

格式正确的清单条件集如下所示：

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
        "msft.sme.server-manager!servers"
        ],
        "connectionTypes": [
        "msft.sme.connection-type.server"
        ],
        "conditions": [
        {
            "inventory": {
            "operatingSystemVersion": {
                "type": "version",
                "operator": "gt",
                "value": "6.3"
            },
            "operatingSystemSKU": {
                "type": "number",
                "operator": "eq",
                "value": "8"
            }
            }
        }
        ]
    }
    ]
}
```

### <a name="script"></a>脚本 ###

最后，您可以运行自定义 PowerShell 脚本，用于识别的可用性和节点的状态。 所有脚本都必须都返回具有以下结构的对象：

``` ps
@{
    State = 'Available' | 'NotSupported' | 'NotConfigured';
    Message = '<Message to explain the reason of state such as not supported and not configured.>';
    Properties =
        @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}
```
State 属性是将控制决策以显示或隐藏您的扩展工具列表中的重要值。  允许的值包括：
| 值 | 描述 |
| ---- | ----------- |
| 可用 | 该扩展应显示在工具列表。 |
| NotSupported | 不应在工具列表中显示该扩展。 |
| NotConfigured | 这是为将来的工作，将提示用户输入其他配置，然后再将该工具提供一个占位符值。  当前此值将导致显示的工具和功能等效于 'Available'。 |

例如，如果我们想要用于加载仅当远程服务器已安装的 BitLocker 的工具，该脚本如下所示：

``` ps
$response = @{
    State = 'NotSupported';
    Message = 'Not executed';
    Properties = @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}

if (Get-Module -ListAvailable -Name servermanager) {
    Import-module servermanager; 
    $isInstalled = (Get-WindowsFeature -name bitlocker).Installed;
    $isGood = $isInstalled;
}

if($isGood) {
    $response.State = 'Available';
    $response.Message = 'Everything should work.';
}

$response
```

使用脚本选项的入口点配置如下所示：

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
        "msft.sme.server-manager!windowsClients"
        ],
        "connectionTypes": [
        "msft.sme.connection-type.windows-client"
        ],
        "conditions": [
        {
            "localhost": true,
            "inventory": {
            "operatingSystemVersion": {
                "type": "version",
                "operator": "eq",
                "value": "10.0.*"
            },
            "operatingSystemSKU": {
                "type": "number",
                "operator": "eq",
                "value": "4"
            }
            },
            "script": "$response = @{ State = 'NotSupported'; Message = 'Not executed'; Properties = @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' }, @{Name='Prop2'; Value = 12345678; Type='number'; }; }; if (Get-Module -ListAvailable -Name servermanager) { Import-module servermanager; $isInstalled = (Get-WindowsFeature -name bitlocker).Installed; $isGood = $isInstalled; }; if($isGood) { $response.State = 'Available'; $response.Message = 'Everything should work.'; }; $response"
        }
        ]
    }
    ]
}
```

## <a name="supporting-multiple-requirement-sets"></a>支持多个要求集 ##

可以使用多个组要求来确定何时通过定义多个"要求"块中显示所需的工具。

例如，若要显示所需的工具，如果"情况 A"或者"方案 B"为 true 时，定义两个要求块;如果有任一项，则返回 true （也就是说，满足所有条件要求块中的），该工具将显示。

``` json
"entryPoints": [
{
    "requirements": [
    {
        "solutionIds": [
            …"scenario A"…
        ],
        "connectionTypes": [
            …"scenario A"…
        ],
        "conditions": [
            …"scenario A"…
        ]
    },
    {
        "solutionIds": [
            …"scenario B"…
        ],
        "connectionTypes": [
            …"scenario B"…
        ],
        "conditions": [
            …"scenario B"…
        ]
    }
    ]
}

```

## <a name="supporting-condition-ranges"></a>支持条件范围 ##

此外可以通过定义多个具有相同的属性，但使用不同运算符的"条件"块来定义范围的条件。

时相同的属性定义的不同的运算符，该工具会显示，前提是两个条件之间的值。

例如，此工具将显示操作系统为 6.3.0 和 10.0.0 之间的版本：

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
             "msft.sme.server-manager!servers"
        ],
        "connectionTypes": [
             "msft.sme.connection-type.server"
        ],
        "conditions": [
        {
            "inventory": {
                "operatingSystemVersion": {
                    "type": "version",
                    "operator": "gt",
                    "value": "6.3.0"
                },
            }
        },
        {
            "inventory": {
                "operatingSystemVersion": {
                    "type": "version",
                    "operator": "lt",
                    "value": "10.0.0"
                }
            }
        }
        ]
    }
    ]
}

```
