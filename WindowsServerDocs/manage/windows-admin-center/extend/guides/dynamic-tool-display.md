---
title: 控制在解决方案中的工具的可见性
description: 控制在 Windows Admin Center SDK (Project Honolulu) 解决方案中的工具的可见性
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f3f34b4c86854bfc55cf4b1b57a0fd3c2baf2ffc
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080964"
---
# 控制在解决方案中的工具的可见性 #

>适用于：Windows Admin Center、Windows Admin Center 预览版

有时可能想要排除 （或隐藏） 你的扩展或从可用的工具列表中的工具。 例如，如果你的工具面向仅 Windows Server 2016 （不较早版本），你可能不希望连接到 Windows Server 2012 R2 服务器以查看你的工具在所有的用户。 (假设用户体验-它们单击它，等待加载，仅以获取其功能并非适用于其连接一条消息的工具。)你可以定义何时显示 （或隐藏） 你工具的 manifest.json 文件中的功能。

## 用于确定何时显示工具的选项 ##

有三种不同的选项可用于确定是否在工具应显示，并可用于特定服务器或群集连接。

* 本地主机
* 清单 （数组的属性）
* 脚本

### 本地主机 ###

条件对象的本地主机属性或不包含一个布尔值，可以通过计算推断如果连接节点是本地主机 （同一台计算机上安装 Windows Admin Center）。 通过将值传递给该属性，指示何时 （条件） 来显示该工具。 例如，如果你仅希望该工具来显示用户实际上连接到本地主机，将其设置所示：

``` json
"conditions": [
{
    "localhost": true
}]
```

或者，如果你仅希望你的工具时要显示连接节点*不是*本地主机：

``` json
"conditions": [
{
    "localhost": false
}]
```

下面是什么的配置设置如下所示到仅显示工具时的连接节点不是本地主机：

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

### 库存属性 ###

SDK 包含一组预特选的库存属性可用于生成条件来确定当你的工具应或不提供。 在清单数组中有 9 个不同的属性：

| 属性名称 | 所需值类型 |
| ------------- | ------------------- |
| computerManufacturer | 字符串 |
| operatingSystemSKU | 数字 |
| operatingSystemVersion | version_string (例如:"10.1。 *") |
| productType | 数字 |
| clusterFqdn | 字符串 |
| isHyperVRoleInstalled | 布尔型 |
| isHyperVPowershellInstalled | 布尔型 |
| isManagementToolsAvailable | 布尔型 |
| isWmfInstalled | 布尔型 |

库存数组中的每个对象必须符合以下 json 结构：

``` json
"<property name>": {
    "type": "<expected type>",
    "operator": "<defined operator to use>",
    "value": "<expected value to evaluate using the operator>"
}
```

#### 运算符值 ####

| 运算符 | 描述 |
| -------- | ----------- |
| gt | 大于 |
| ge | 大于或等于 |
| lt | 小于 |
| le | 小于或等于 |
| eq | 等于 |
| ne | 不等于 |
| 为 | 检查是否值为 true |
| 不 | 如果值为 false 检查 |
| 包含 | 在字符串中存在的项 |
| notContains | 一个字符串中不存在项 |

#### 数据类型 ####

对于类型属性的可用选项：

| 类型 | 说明 |
| ---- | ----------- |
| version | 版本号 (例如： 10.1。 *) |
| 数字 | 数字值 |
| 字符串 | 字符串值 |
| 布尔型 | true 或 false |

#### 值类型 ####

值属性接受这些类型：

* 字符串
* 数字
* 布尔型

格式正确的库存条件集如下所示：

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

### 脚本 ###

最后，你可以运行自定义 PowerShell 脚本来标识的可用性和节点的状态。 所有脚本必须都返回的对象具有以下结构：

``` ps
@{
    State = 'Available' | 'NotSupported' | 'NotConfigured';
    Message = '<Message to explain the reason of state such as not supported and not configured.>';
    Properties =
        @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}
```
状态属性是重要的值将控制要显示或隐藏你在工具列表中的扩展的决定。  允许的值为：
| 值 | 说明 |
| ---- | ----------- |
| 可用 | 扩展应显示在工具列表。 |
| NotSupported | 扩展不应显示在工具列表。 |
| NotConfigured | 这是将来将提示用户输入其他配置，然后再将该工具提供的工作的占位符值。  当前此值将导致显示该工具，并且是在功能上的等同于可用。 |

例如，如果我们希望加载只有远程服务器具有 BitLocker 安装工具，该脚本如下所示：

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

## 支持多个要求集 ##

你可以使用多个组的要求来确定何时通过定义多个"要求"块中显示你的工具。

例如，若要显示你的工具，如果"方案 A"或者"方案 B"为 true 时，定义两个要求块;如果任何一种情况 （即，要求块内的所有条件都满足时），该工具会显示。

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

## 支持的条件范围 ##

你还可以通过定义多个"条件"块具有相同的属性，但具有不同的运算符定义条件的范围。

当与不同的运营商定义了相同的属性时，该工具将显示为值之间的两个条件。

例如，只要操作系统 6.3.0 和 10.0.0 之间的版本，将显示此工具：

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
