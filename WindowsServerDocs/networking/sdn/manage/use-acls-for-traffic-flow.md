---
title: 使用访问控制列表（Acl）管理数据中心网络流量流
description: 本主题介绍如何配置访问控制列表（Acl），以便在虚拟子网上使用数据中心防火墙和 Acl 管理数据流量流。 通过创建应用于虚拟子网或网络接口的 Acl 来启用和配置数据中心防火墙。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a7ac5af-85e9-4440-a631-6a3a38e9015d
ms.author: pashort
author: shortpatti
ms.date: 08/27/2018
ms.openlocfilehash: 6a1d210d25309be322359add20da4eb8d0eee091
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355806"
---
# <a name="use-access-control-lists-acls-to-manage-datacenter-network-traffic-flow"></a>使用访问控制列表（Acl）管理数据中心网络流量流

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍如何配置访问控制列表（Acl），以便在虚拟子网上使用数据中心防火墙和 Acl 管理数据流量流。 通过创建应用于虚拟子网或网络接口的 Acl 来启用和配置数据中心防火墙。   

本主题中的以下示例演示如何使用 Windows PowerShell 来创建这些 Acl。  

## <a name="configure-datacenter-firewall-to-allow-all-traffic"></a>配置 Datacenter 防火墙以允许所有流量  

部署 SDN 后，应在新环境中测试基本网络连接。  为实现此目的，为数据中心防火墙创建一个规则，该规则允许所有网络流量，而无需限制。   

使用下表中的条目创建一组允许所有入站和出站网络流量的规则。


| 源 IP | 目标 IP | Protocol | 源端口 | 目标端口 | Direction | 操作 | Priority |
|:---------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|    \*     |       \*       |   全部    |     \*      |        \*        |  入站  | Allow  |   100    |
|    \*     |       \*       |   全部    |     \*      |        \*        | 出站  | Allow  |   110    |

---       

### <a name="example-create-an-acl"></a>例如：创建 ACL 
在此示例中，你将创建一个包含两个规则的 ACL：

1. **AllowAll_Inbound** -允许所有网络流量传递到配置了此 ACL 的网络接口。    
2. **AllowAllOutbound** -允许所有流量通过网络接口。 现已准备好在虚拟子网和网络接口中使用由资源 id "AllowAll-1" 标识的 ACL。  

下面的示例脚本使用从**NetworkController**模块导出的 Windows PowerShell 命令创建此 ACL。  


```PowerShell
$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "100"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  
$aclrule1 = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule1.Properties = $ruleproperties  
$aclrule1.ResourceId = "AllowAll_Inbound"  
$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "110"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  
$aclrule2 = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule2.Properties = $ruleproperties  
$aclrule2.ResourceId = "AllowAll_Outbound"  
$acllistproperties = new-object Microsoft.Windows.NetworkController.AccessControlListProperties  
$acllistproperties.AclRules = @($aclrule1, $aclrule2)  
New-NetworkControllerAccessControlList -ResourceId "AllowAll" -Properties $acllistproperties -ConnectionUri <NC REST FQDN>  
```  

>[!NOTE]  
>网络控制器的 Windows PowerShell 命令参考位于主题[网络控制器 cmdlet](https://technet.microsoft.com/library/mt576401.aspx)中。  

## <a name="use-acls-to-limit-traffic-on-a-subnet"></a>使用 Acl 限制子网上的流量  
在此示例中，将创建一个 ACL，以防止 192.168.0.0/24 子网中的 Vm 相互通信。 这种类型的 ACL 适用于限制攻击者在子网内传播横向的能力，同时仍允许 Vm 从子网外部接收请求，以及与其他子网上的其他服务通信。   


|   源 IP    | 目标 IP | Protocol | 源端口 | 目标端口 | Direction | 操作 | Priority |
|:--------------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|  192.168.0.1   |       \*       |   全部    |     \*      |        \*        |  入站  | Allow  |   100    |
|       \*       |  192.168.0.1   |   全部    |     \*      |        \*        | 出站  | Allow  |   101    |
| 192.168.0.0/24 |       \*       |   全部    |     \*      |        \*        |  入站  | 阻止  |   102    |
|       \*       | 192.168.0.0/24 |   全部    |     \*      |        \*        | 出站  | 阻止  |   103    |
|       \*       |       \*       |   全部    |     \*      |        \*        |  入站  | Allow  |   104    |
|       \*       |       \*       |   全部    |     \*      |        \*        | 出站  | Allow  |   105    |

--- 

以下示例脚本创建的 ACL 由资源 id**子网-192-168-0-0**标识，现在可以应用于使用 "192.168.0.0/24" 子网地址的虚拟网络子网。  附加到该虚拟网络子网的任何网络接口会自动获取上述 ACL 规则。  

下面是一个示例脚本，该脚本使用 Windows Powershell 命令，以使用网络控制器创建此 ACL REST API：  

```PowerShell  
import-module networkcontroller  
$ncURI = "https://mync.contoso.local"  
$aclrules = @()  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "192.168.0.1"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "100"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowRouter_Inbound"  
$aclrules += $aclrule  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "192.168.0.1"  
$ruleproperties.Priority = "101"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowRouter_Outbound"  
$aclrules += $aclrule  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Deny"  
$ruleproperties.SourceAddressPrefix = "192.168.0.0/24"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "102"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "DenySubnet_Inbound"  
$aclrules += $aclrule  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Deny"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "192.168.0.0/24"  
$ruleproperties.Priority = "103"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "DenySubnet_Outbound"  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "104"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowAll_Inbound"  
$aclrules += $aclrule  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "105"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowAll_Outbound"  
$aclrules += $aclrule  

$acllistproperties = new-object Microsoft.Windows.NetworkController.AccessControlListProperties  
$acllistproperties.AclRules = $aclrules  

New-NetworkControllerAccessControlList -ResourceId "Subnet-192-168-0-0" -Properties $acllistproperties -ConnectionUri $ncURI   
```  

