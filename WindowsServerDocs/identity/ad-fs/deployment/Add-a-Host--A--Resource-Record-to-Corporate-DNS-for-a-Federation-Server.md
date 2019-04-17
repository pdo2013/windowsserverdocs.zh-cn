---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: "添加联盟服务器 (A) 主机资源记录公司 DNS"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 425cfe794095f1515eb3fae2f1a5e5db90ba3d00
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>添加联盟服务器 (A) 主机资源记录公司 DNS

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012


客户端上公司成功访问的网络使用 Windows 的集成身份验证的联盟服务器，主机 \(A\) 资源记录必须先在创建解决了主机名称帐户联盟服务器的公司域名系统 \(DNS\) \ (例如，fs.fabrikam.com\) 联合服务器或联合身份验证的服务器群集的 IP 地址。 可以使用下面的过程中添加到公司 DNS 服务器联盟主机 \(A\) 资源记录。  
  
在会员**管理员**，或等效的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>若要添加到公司 DNS 服务器联盟主机 \(A\) 资源记录  
  
1.  公司的网络 DNS 服务器，在打开 snap\ 中 DNS。  
  
2.  在该控制台树，right\ 单击相应的前进查找区域中，，然后单击**新主机 \(A or AAAA\)**。  
  
3.  在**名称**，键入仅联合身份验证的服务器或联合身份验证的服务器群集; 的计算机名称例如，对完整的域命名 \(FQDN\) fs.fabrikam.com，类型**fs**。  
  
4.  在**IP 地址**，键入联合服务器或联合身份验证的服务器群集，例如，192.168.1.4 的 IP 地址。  
  
5.  单击**添加主机**。  
  
## <a name="additional-references"></a>其他参考  
[清单：联合服务器设置](Checklist--Setting-Up-a-Federation-Server.md)  
  
[联合身份验证的服务器的名称分辨率要求](https://technet.microsoft.com/library/dd807055.aspx)  
  

