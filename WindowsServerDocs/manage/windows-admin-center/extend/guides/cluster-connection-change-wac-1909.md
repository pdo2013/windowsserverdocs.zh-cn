---
title: Windows 管理中心中的群集连接类型更改 v1909
description: Windows 管理中心中的群集连接类型更改 v1909
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 10/01/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a07b30517f0d45b7e6f4f41f0ef9a6549e6e2117
ms.sourcegitcommit: de71970be7d81b95610a0977c12d456c3917c331
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2019
ms.locfileid: "71952768"
---
# <a name="cluster-connection-type-changes-in-windows-admin-center-v1909"></a>Windows 管理中心中的群集连接类型更改 v1909

>适用于：Windows Admin Center、Windows Admin Center 预览版

> [!IMPORTANT]
> 本文档介绍了为故障转移群集和超聚合群集（HCI）解决方案开发 Windows 管理中心工具时所需的更改。 这是你的扩展所必需的更改，以与 Windows 管理中心 v1909 Preview 版本和未来的 GA 版本兼容。

在 Windows 管理中心 v1909 中，我们已将两个不同的群集连接类型（故障转移群集和超聚合群集连接）统一为一个群集连接类型。 用户将不再需要提前标识群集的配置以决定将群集添加到哪种连接类型，也不需要将群集添加两次以不同的连接类型来访问不同的工具集。 现在可以将群集作为 "Windows Server 群集" 添加，并将根据是否启用存储空间直通来加载相应的工具。

由于这需要更改连接类型定义以及与群集相关的工具如何决定何时加载，因此，为群集（HCI 或非 HCI 群集，或两者）提供工具的扩展将需要实现中的更改，如所述在线订购.

## <a name="manifestjson---solutionsids-and-connectiontypes"></a>solutionsIds 和 connectionTypes

之前，若要为故障转移群集或 HCI 群集连接类型显示工具，你应在 ```manifest.json``` 文件中使用以下定义之一。

对于故障转移群集：
``` json
    {
        "entryPointType": "tool",
        "name": "MyToolName",
        "urlName": "MyToolUrl",
        "displayName": "MyToolDisplayName",
        "description": "MyToolDescription",
        "icon": "MyToolIcon",
        "path": "MyToolPath",
        "iframeId": "MyToolIframeId",
        "requirements": [
        {
            "solutionIds": [
                "msft.sme.failover-cluster!failover-cluster"
            ],
            "connectionTypes": [
                "msft.sme.connection-type.cluster"
            ]
        }
        ]
    }
```

对于 HCI 群集，上面突出显示的部分将被替换为以下内容：
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!SDDC"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.hyper-converged-cluster"
        ]
    }
    ]
```

在 Windows 管理中心1909及更高版本中，两个 solutionIds 和 connectionTypes 已替换为以下内容：
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!cluster"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.cluster"
        ]
    }
    ]
```

这是目前支持的唯一与群集相关的 solutionIds 和 connectionTypes 类型。 如果你的工具仅定义了此 solutionIds 和 connectionTypes 类型，则将为任何故障转移群集连接加载该工具，而不管它是否为 HCI 群集。 如果要将工具限制为仅可用于 HCI 群集或非 HCI 群集，则需要另外使用下一部分中所述的新清单属性。

## <a name="manifestjson--inventory-properties"></a>.manifest-清单属性
当连接到服务器或群集时，Windows 管理中心将查询一组清单属性，您可以使用这些属性来构建条件，确定何时可以使用您的工具（请参阅[控件的](dynamic-tool-display.md)详细信息的可见性文档）。 在 Windows 管理中心 v1909 中，我们已向此列表中添加了两个新属性，可用于确定群集是否为超聚合群集。 

### <a name="iss2denabled"></a>isS2dEnabled
从技术上说，超聚合群集定义为启用了存储空间直通（S2D）的故障转移群集。 如果希望工具仅可用于超聚合群集，即启用 S2D 后，请添加以下清单条件：
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!cluster"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.cluster"
        ],
        "conditions": [
        {
            "inventory": {
                "isS2dEnabled": {
                    "type": "boolean",
                    "operator": "is"
                }
            }
        }
        ]
    }
    ]
```
> [!TIP] 
> 如果希望工具仅在未启用 S2D 时可用，请将 "operator" 值更改为 "not"。

### <a name="isbritannicaenabled"></a>isBritannicaEnabled
此外，如果依赖于 SDDC 管理群集资源并使用 SDDC 对象模型，则可以检查以下条件：
``` json
    "conditions": [
        {
            "inventory": {
                "isS2dEnabled": {
                    "type": "boolean",
                    "operator": "is"
                },
                "isBritannicaEnabled": {
                    "type": "boolean",
                    "operator": "is"
                }
            }
        }
    ]
```

## <a name="backward-compatibility-to-support-previous-versions-of-windows-admin-center"></a>支持以前版本的 Windows 管理中心的向后兼容性
若要确保你的扩展继续使用较旧版本的 Windows 管理中心（如 v1904 GA 版本），则可以将上一个 solutionIds 和 connectionTypes 定义与新定义一起使用。 请参阅下面的示例，仅在所有版本的 Windows 管理中心中为 HCI 群集显示你的工具。
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!SDDC"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.hyper-converged-cluster"
        ]
    },
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!SDDC",
            "msft.sme.software-defined-data-center!cluster"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.hyper-converged-cluster",
            "msft.sme.connection-type.cluster"
        ],
        "conditions": [
            {
                "inventory": {
                    "isS2dEnabled": {
                        "type": "boolean",
                        "operator": "is"
                    }
                }
            }
        ]
    }
    ]
```

## <a name="known-issue-appcontextserviceactiveconnectionishyperconvergedclusterisfailovercluster-is-not-set-properly-in-windows-admin-center-v1909"></a>已知问题：AppContextService activeConnection. isHyperConvergedCluster/isFailoverCluster 在 Windows 管理中心 v1909 中未正确设置
最近的更改回归是指未在 Windows 管理中心 v1909 中正确设置 ```AppContextService.activeConnection.isHyperConvergedCluster/isFailoverCluster``` 属性，这些属性将始终为 false。 这将在下一版本的 v1910 中得到修复，但也将在2020的以下 GA 版本中弃用，并且不再可用。 以后，可以将其替换为以下代码，并使用 ```this.connectHCI```。
```
    import { ClusterInventoryCache } from '@msft-sme/core';

    constructor(
        ...
        private appContextService: AppContextService,
        ...
    ) {
        ...
        this.clusterInventoryCache = new ClusterInventoryCache(
            this.appContextService,
            <SharedCacheOptions>{ expiration: Constants.sharedCacheExpiration }
        );

         this.isBritannicEnabled().subscribe(result => this.connectHCI = result);
    }

    private isBritannicEnabled(): Observable<boolean> {
        return this.clusterInventoryCache.query(this.getClusterInventoryQueryParameters())
        .pipe(
            map(inventory => {
                return inventory.instance.isBritannicaEnabled;
        }));
    }

    private getClusterInventoryQueryParameters(): ClusterInventoryParams {
        const nodeName = this.appContextService.activeConnection.validNodeName;
        const endpoint = this.appContextService.authorizationManager.getJeaEndpoint(nodeName);
        const options = { powerShellEndpoint: endpoint };
        return {
            name: nodeName,
            requestOptions: options
        };
    }
```