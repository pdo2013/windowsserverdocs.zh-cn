---
title: 用于管理数据中心网络流量访问 Acl)
description: 本主题介绍的软件定义网络指南如何管理租户工作负载和 Windows Server 2016 中的虚拟网络的一部分。
manager: brianlic
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
ms.openlocfilehash: 64b7e1abf1ddb8132a8c6692fe82521c589f32df
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="use-access-control-lists-acls-to-manage-datacenter-network-traffic-flow"></a>用于管理数据中心网络流量访问 Acl)

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解如何配置访问控制列表管理虚拟子网使用 Datacenter 防火墙和 Acl 数据流量。  
  
你可以启用并创建应用到虚拟子网或网络接口 Acl 配置 Datacenter 防火墙。  
  
下面示例展示如何使用 Windows PowerShell 创建这些 Acl。  
  
## <a name="configure-datacenter-firewall-to-allow-all-traffic"></a>配置数据中心防火墙，以允许所有通信  
  
后部署 SDN，建议你在新环境测试基本网络连接。  
  
为此，你可以允许无限制的所有网络通信的数据中心防火墙创建为规则。   
  
可以使用下表中的条目创建一套允许所有传入和传出网络通信的规则。  
  
  
  
源 IP|目标 IP|协议|源端口|目标端口|方向|操作|优先级   
---------|---------|---------|---------|---------|---------|---------|---------  
*    |   *      |   所有      |    *     |      *   |     归巢的    |   允许      |   100        
*    |     *    |     所有    |     *    |     *    |   站      |  允许       |  110         
  
  
包含两个规则 ACL 创建了此示例脚本：    
- 第一个规则"AllowAll_Inbound"允许所有网络通信将传递到网络接口此 ACL 配置的位置。    
- 该第二个规则"AllowAllOutbound"允许通过网络接口退出所有通信。  
此 ACL，具体由资源 id"AllowAll 1"现在就可以用于虚拟子网和网络接口中。  
  
下面的示例脚本使用 Windows PowerShell 命令导出从**NetworkController**模块创建此 ACL。  
  
  
```  
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
>网络控制器 Windows PowerShell 命令参考位于主题[网络控制器 Cmdlet](https://technet.microsoft.com/library/mt576401.aspx)。  
  
## <a name="use-acls-to-limit-traffic-on-a-subnet"></a>使用 Acl 限制子网上通信  
  
你可以使用此示例中创建阻止虚拟机 (Vm) ACL 192.168.0.0/24 子网从彼此通信内。   
  
这种类型的 ACL 可用于限制攻击能够子网内, 横向传播同时仍然允许 Vm 接收来自请求之外子网，以及与其他服务其他子网进行通信。  
  
  
源 IP|目标 IP|协议|源端口|目标端口|方向|操作|优先级   
---------|---------|---------|---------|---------|---------|---------|---------  
192.168.0.1    | * | 所有 | * | * | 归巢的|   允许      |   100        
* |192.168.0.1 | 所有 | * | * | 站|  允许       |  101         
192.168.0.0/24 | * | 所有 | * | * | 归巢的| 阻止 | 102  
* | 192.168.0.0/24 |所有| * | * | 站 | 阻止 |103  
* | *  |所有| * | * | 归巢的 | 允许 |104  
* | *  |所有| * | * | 站 | 允许 |105  
  
示例脚本下方，具体由资源 id 创建的 ACL**子网-192-168-0-0**，现在可以将应用到使用"192.168.0.0/24"子网地址虚拟网。  自动连接到该虚拟子网任何网络接口获取上述 ACL 规则应用。  
  
以下是使用 Windows Powershell 命令创建使用网络控制器 REST API 此 ACL 一个示例脚本：  
  
```  
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
  
