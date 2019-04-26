---
title: 为 Vm 和容器托管计算网络 (HCN) 服务 API
description: 主机计算网络 (HCN) 服务 API 是一个面向公众的 Win32 API，提供平台级别访问权限来管理虚拟网络、 虚拟网络终结点和关联的策略。 一起这样连接和安全的虚拟机 (Vm) 和 Windows 主机上运行的容器。
ms.author: jmesser
author: jmesser81
ms.date: 11/05/2018
ms.openlocfilehash: 50af0dab69633aa6e07ded68e9246aa0315377f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844978"
---
# <a name="host-compute-network-hcn-service-api-for-vms-and-containers"></a>为 Vm 和容器托管计算网络 (HCN) 服务 API

>适用于：Windows 服务器 （半年频道），Windows Server 2016

主机计算网络 (HCN) 服务 API 是一个面向公众的 Win32 API，提供平台级别访问权限来管理虚拟网络、 虚拟网络终结点和关联的策略。 一起这样连接和安全的虚拟机 (Vm) 和 Windows 主机上运行的容器。 

开发人员使用 HCN 服务 API 来管理 Vm 的网络和在其应用程序工作流中的容器。 HCN API 旨在为开发人员提供最佳体验。 最终用户并直接与这些 Api 交互。  

## <a name="features-of-the-hcn-service-api"></a>HCN 服务 API 的功能
-   作为托管 OnCore/VM 上的主机网络服务 (HNS) 的 C API 实现。

-   提供创建、 修改、 删除和枚举 HCN 对象，如网络、 终结点、 命名空间和策略的能力。 对对象 （例如，网络处理） 的句柄执行的操作，并在内部使用 RPC 上下文句柄实现这些句柄。

-   基于架构的。 大多数函数的 api 定义输入和输出参数将包含作为 JSON 文档的函数调用的参数的字符串。 JSON 文档均基于强类型和版本控制架构，这些架构是公共的文档的一部分。 

-   提供订阅/回调 API，使客户端注册通知的服务级事件，例如网络的创建和删除。

-   HCN API （也称为适用于桌面桥 运行系统服务在 centennial) 应用。 通过从调用方检索用户令牌的 API 检查 ACL。

>[!TIP]
>HCN 服务 API 支持后台任务和非前台窗口中。 

## <a name="terminology-host-vs-compute"></a>术语：主机 vs。计算
主机计算服务允许调用方创建和管理虚拟机和一台物理计算机上的容器。 它名为遵循行业术语。 

- **主机**广泛用于虚拟化行业中引用提供了虚拟化的资源的操作系统。

- **计算**用于引用比只是虚拟机的更多的虚拟化方法。 计算主机的网络服务允许调用方创建和管理虚拟机和容器在单一物理计算机上的网络。

## <a name="schema-based-configuration-documents"></a>基于架构的配置文档
基于妥善定义的架构的配置文档是已建立的行业标准的虚拟化空间中。 大多数虚拟化解决方案，例如 Docker 和 Kubernetes，提供 Api 基础配置文档。 多项行业计划，Microsoft，参与驱动器来定义和验证这些架构，如生态系统[OpenAPI](https://www.openapis.org/)。  这些计划还驱动器对于容器，如使用的架构的特定架构定义的标准化[开放容器计划 (OCI)](https://www.opencontainers.org/)。

用于创作配置文档的语言是[JSON](https://tools.ietf.org/html/rfc8259)，结合使用：
-   定义为文档对象模型的架构定义
-   JSON 文档是否符合架构验证
-   自动转换的 JSON 文档与其他这些架构中使用的 Api 调用方的编程语言中的本机表示形式 

频繁使用的架构定义[OpenAPI](https://www.openapis.org/)并[JSON 架构](http://json-schema.org/)，可用于在文档中，例如指定属性的详细的定义：
-   有效的属性，如 0-100 表示百分比的属性的值集。
-   枚举，表示为一组属性的有效字符串的定义。
-   正则表达式的所需格式的字符串。 

作为记录 HCN Api 的一部分，我们计划发布 OpenAPI 规范作为我们的 JSON 文档的架构。 根据此规范，可以允许的客户端使用的编程语言中的架构对象的类型安全使用的架构的特定于语言的表示形式。 

### <a name="example"></a>示例 

下面是示例工作流的对象，表示 VM 的配置文档中的 SCSI 控制器。 

在 Windows 源代码中，我们可以定义使用.mars 文件的架构： onecore/vm/dv/net/hns/schema/mars/Schema/HCN.Schema.Network.mars

```
enum IpamType
{
    [NewIn("2.0")] Static,
    [NewIn("2.0")] Dhcp,
};

class Ipam
{
    // Type : dhcp
    [NewIn("2.0"),OmitEmpty] IpamType   Type;
    [NewIn("2.0"),OmitEmpty] Subnet     Subnets[];
};

class Subnet : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] string         IpAddressPrefix;
    [NewIn("2.0"),OmitEmpty] SubnetPolicy   Policies[];
    [NewIn("2.0"),OmitEmpty] Route          Routes[];
};


enum SubnetPolicyType
{
    [NewIn("2.0")] VLAN
};

class SubnetPolicy
{
    [NewIn("2.0"),OmitEmpty] SubnetPolicyType                 Type;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Common.PolicySettings Data;
};

class PolicySettings
{
    [NewIn("2.0"),OmitEmpty]  string      Name;
};

class VlanPolicy : HCN.Schema.Common.PolicySettings 
{
    [NewIn("2.0")] uint32 IsolationId;
};

class Route 
{
    [NewIn("2.0"),OmitEmpty] string NextHop;
    [NewIn("2.0"),OmitEmpty] string DestinationPrefix;
    [NewIn("2.0"),OmitEmpty] uint16 Metric;
};

```

>[!TIP]
>[NewIn("2.0") 批注是架构定义的版本控制支持的一部分。

此内部定义中，我们生成架构的 OpenAPI 的规范：

```
{ 
    "swagger" : "2.0", 
    "info" : { 
       "version" : "2.1", 
       "title" : "HCN API" 
    },
    "definitions": {        
        "Ipam": {
            "type": "object",
            "properties": {
                "Type": {
                    "type": "string",
                    "enum": [
                        "Static",
                        "Dhcp"
                    ],
                    "description": " Type : dhcp"
                },
                "Subnets": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/Subnet"
                    }
                }
            }
        },
        "Subnet": {
            "type": "object",
            "properties": {
                "ID": {
                    "type": "string",
                    "pattern": "^[0-9A-Fa-f]{8}-([0-9A-Fa-f]{4}-){3}[0-9A-Fa-f]{12}$"
                },                
                "IpAddressPrefix": {
                    "type": "string"
                },
                "Policies": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/SubnetPolicy"
                    }
                },
                "Routes": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/Route"
                    }
                }
            }
        },
        "SubnetPolicy": {
            "type": "object",
            "properties": {
                "Type": {
                    "type": "string",
                    "enum": [
                        "VLAN",
                        "VSID"
                    ]
                },
                "Data": {
                    "$ref": "#/definitions/PolicySettings"
                }
            }
        },
        "PolicySettings": {
            "type": "object",
            "properties": {
                "Name": {
                    "type": "string"
                }
            }
        },                      
        "VlanPolicy": {
            "type": "object",
            "properties": {
                "Name": {
                    "type": "string"
                },
                "IsolationId": {
                    "type": "integer",
                    "format": "uint32"
                }
            }
        },
        "Route": {
            "type": "object",
            "properties": {
                "NextHop": {
                    "type": "string"
                },
                "DestinationPrefix": {
                    "type": "string"
                },
                "Metric": {
                    "type": "integer",
                    "format": "uint16"
                }
            }
        }        
    }
} 
```

您可以使用工具，如[Swagger](https://swagger.io/)，来生成特定于语言的表示形式的编程语言客户端使用的架构。 Swagger 支持各种不同的语言，如C#，Go、 Javascript 和 Python)。

- [示例生成C#代码](example-c-sharp.md)顶部级别 IPAM 和子网对象。

- [生成的 Go 代码示例](example-go.md) 顶部级别 IPAM 子网对象。 转到使用 Docker 和 Kubernetes，这是两个主机计算网络服务 Api 的使用者。 转到提供有关封送处理转到类型与 JSON 文档的内置支持。

除了生成和验证代码，您可以使用工具来简化与 JSON 文档的工作 — 即， [Visual Studio Code](https://code.visualstudio.com/Docs/languages/json)。

### <a name="top-level-objects-defined-in-the-hcnschemasmars-file"></a>HCN 中定义的顶级对象。Schemas.mars 文件
如上所述，您可以找到使用的一组下的.mars 文件中的 HCN Api 文档的文档架构： onecore/vm/dv/net/hns/架构/mars/架构

顶级对象有：
- [HostComputeNetwork](hcn-scenarios.md#scenario-hcn)
- [HostComputeEndpoint](hcn-scenarios.md#scenario-hcn-endpoint)
- [HostComputeNamespace](hcn-scenarios.md#scenario-hcn-namespace)
- [HostComputeLoadBalancer](hcn-scenarios.md#scenario-hcn-load-balancer)

```
class HostComputeNetwork : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.NetworkMode          Type;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.NetworkPolicy        Policies[];
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.MacPool              MacPool;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.DNS                  Dns;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Ipam                 Ipams[];
};

class HostComputeEndpoint : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] string                                     HostComputeNetwork;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Endpoint.EndpointPolicy Policies[];
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Endpoint.IpConfig       IpConfigurations[];
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.DNS                     Dns;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Route                   Routes[];
    [NewIn("2.0"),OmitEmpty] string                                     MacAddress;
};

class HostComputeNamespace : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] uint32                                    NamespaceId;
    [NewIn("2.0"),OmitEmpty] Guid                                      NamespaceGuid;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Namespace.NamespaceType        Type;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Namespace.NamespaceResource    Resources[];
};

class HostComputeLoadBalancer : HCN.Schema.Common.Base
{
    [NewIn("2.0"), OmitEmpty] string                                               HostComputeEndpoints[]; 
    [NewIn("2.0"), OmitEmpty] string                                               VirtualIPs[]; 
    [NewIn("2.0"), OmitEmpty] HCN.Schema.Network.Endpoint.Policy.PortMappingPolicy PortMappings[]; 
    [NewIn("2.0"), OmitEmpty] HCN.Schema.LoadBalancer.LoadBalancerPolicy           Policies[];
};
```

## <a name="next-steps"></a>后续步骤

- 详细了解如何[常见 HCN 方案](hcn-scenarios.md)。

- 详细了解如何[RPC 上下文处理 HCN](hcn-declaration-handles.md)。

- 详细了解如何[HCN JSON 文档架构](hcn-json-document-schemas.md)。