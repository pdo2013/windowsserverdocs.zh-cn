---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: 为联合服务器将主机 (A) 资源记录添加到企业 DNS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 132e71cec134d17dd73be998683c09f752fdc414
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360329"
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>为联合服务器将主机 (A) 资源记录添加到企业 DNS



要使企业网络上的客户端成功地使用 Windows 集成身份验证访问联合服务器，必须先在公司域名系统 \(DNS @ no__t-3 中创建一个主机 \(A @ no__t联合服务器或联合服务器群集的 IP 地址的帐户联合服务器 @no__t 的主机名（4for 示例，no__t）。 你可以使用以下过程向联合服务器的企业 DNS 添加主机 @no__t 0A @ no__t 资源记录。  
  
**Administrators**中的成员身份或同等身份是完成此过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>向联合服务器的企业 DNS 添加主机 @no__t 0A @ no__t 资源记录  
  
1.  在企业网络的 DNS 服务器上，打开 DNS snap @ no__t-0in。  
  
2.  在控制台树中，右键 @ no__t-0click 适用的正向查找区域，然后单击 "**新建主机 \(a 或 AAAA @ no__t**"。  
  
3.  在 "**名称**" 中，仅键入联合服务器或联合服务器群集的计算机名称;例如，对于完全限定的域名 \(FQDN @ no__t-2 fs.fabrikam.com，请键入**fs**。  
  
4.  在 " **ip 地址**" 中，键入联合服务器或联合服务器群集的 IP 地址，例如192.168.1.4。  
  
5.  单击“添加主机”。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合服务器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[联合服务器的名称解析要求](https://technet.microsoft.com/library/dd807055.aspx)  
  

