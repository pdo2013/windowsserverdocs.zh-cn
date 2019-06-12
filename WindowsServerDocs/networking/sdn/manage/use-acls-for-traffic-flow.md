---
title: 使用访问控制列表 (Acl) 来管理数据中心网络流量流
description: 在本主题中，您学习如何配置访问控制列表 (Acl) 来管理在虚拟子网中使用数据中心防火墙和 Acl 的数据流量流。 启用并配置数据中心防火墙通过创建获取应用于虚拟子网或网络接口的 Acl。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a7ac5af-85e9-4440-a631-6a3a38e9015d
ms.author: pashort
author: shortpatti
ms.date: 08/27/2018
ms.openlocfilehash: 7bfb74e0964735d357226ab1e5af826796c48d81
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446301"
---
# <a name="use-access-control-lists-acls-to-manage-datacenter-network-traffic-flow"></a>使用访问控制列表 (Acl) 来管理数据中心网络流量流

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在本主题中，您学习如何配置访问控制列表 (Acl) 来管理在虚拟子网中使用数据中心防火墙和 Acl 的数据流量流。 启用并配置数据中心防火墙通过创建获取应用于虚拟子网或网络接口的 Acl。   

本主题中的以下示例演示如何使用 Windows PowerShell 来创建这些 Acl。  

## <a name="configure-datacenter-firewall-to-allow-all-traffic"></a>配置数据中心防火墙以允许所有流量  

一旦部署 SDN，应在新环境中测试的基本网络连接。  若要实现此目的，创建允许所有网络流量，不受限制的数据中心防火墙规则。   

使用下表中的条目创建一组允许所有入站和出站网络流量的规则。


| 源 IP | 目标 IP | Protocol | 源端口 | 目标端口 | Direction | 操作 | Priority |
|:---------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|    \*     |       \*       |   全部    |     \*      |        \*        |  入站  | 允许  |   100    |
|    \*     |       \*       |   全部    |     \*      |        \*        | 出站  | 允许  |   110    |

---       

### <a name="example-create-an-acl"></a>例如：创建一个 ACL 
在此示例中，使用两个规则创建一个 ACL:

1. **AllowAll_Inbound** -允许所有网络流量进入其中配置此 ACL 的网络接口。    
2. **AllowAllOutbound** -允许所有流量从网络接口传递。 此 ACL，由"AllowAll-1"的资源 id 标识现在已准备好虚拟子网和网络接口中使用。  

下面的示例脚本使用 Windows PowerShell 命令从导出**NetworkController**模块来创建此 ACL。  


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
>网络控制器的 Windows PowerShell 命令参考位于主题[网络控制器 Cmdlet](https://technet.microsoft.com/library/mt576401.aspx)。  

## <a name="use-acls-to-limit-traffic-on-a-subnet"></a>使用 Acl 来限制子网的流量  
在此示例中，创建阻止 Vm 192.168.0.0/24 子网内相互进行通信的 ACL。 此类型的 ACL 可用于限制攻击者能够在子网内横向分布同时仍允许 Vm 接收来自子网外部的请求，以及与其他子网上的其他服务进行通信。   


|   源 IP    | 目标 IP | Protocol | 源端口 | 目标端口 | Direction | 操作 | Priority |
|:--------------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|  192.168.0.1   |       \*       |   全部    |     \*      |        \*        |  入站  | 允许  |   100    |
|       \*       |  192.168.0.1   |   全部    |     \*      |        \*        | 出站  | 允许  |   101    |
| 192.168.0.0/24 |       \*       |   全部    |     \*      |        \*        |  入站  | 阻止  |   102    |
|       \*       | 192.168.0.0/24 |   全部    |     \*      |        \*        | 出站  | 阻止  |   103    |
|       \*       |       \*       |   全部    |     \*      |        \*        |  入站  | 允许  |   104    |
|       \*       |       \*       |   全部    |     \*      |        \*        | 出站  | 允许  |   105    |

--- 

下面的示例脚本按资源 id 标识创建的 ACL**子网-192-168-0-0**，现在可以应用到使用"192.168.0.0/24"子网地址的虚拟网络子网。  任何网络接口附加到该虚拟网络子网自动获取应用的更高版本的 ACL 规则。  

下面是使用 Windows Powershell 命令来创建使用网络控制器 REST API 此 ACL 的示例脚本：  

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

