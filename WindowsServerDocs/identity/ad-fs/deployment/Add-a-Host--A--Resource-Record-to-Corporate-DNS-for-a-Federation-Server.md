---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: 为联合服务器将主机 (A) 资源记录添加到企业 DNS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 425cfe794095f1515eb3fae2f1a5e5db90ba3d00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856178"
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>为联合服务器将主机 (A) 资源记录添加到企业 DNS

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012


适用于使用 Windows 集成身份验证，主机的联合身份验证服务器的成功访问网络上的公司的客户端\(A\)资源记录必须首先在公司域名系统中创建\(DNS\)解析帐户联合身份验证服务器的主机名\(例如，fs.fabrikam.com\)到联合身份验证服务器或联合身份验证服务器群集的 IP 地址。 可以使用以下过程来添加主机\(A\)公司 dns 中为联合身份验证服务器的资源记录。  
  
中的成员身份**管理员**，或等效身份是完成此过程所需的最小。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>若要将主机添加\(A\)公司 dns 中为联合身份验证服务器的资源记录  
  
1.  在企业网络的 DNS 服务器上，打开 DNS 管理单元\-中。  
  
2.  在控制台树中，右键\-单击适用的正向查找区域，然后单击**新的主机\(A 或 AAAA\)**。  
  
3.  在中**名称**，仅键入计算机名称的联合身份验证服务器或联合身份验证服务器群集; 例如，为完全限定的域名\(FQDN\) fs.fabrikam.com，键入**fs**.  
  
4.  在中**IP 地址**，键入联合身份验证服务器或联合身份验证服务器群集，例如 192.168.1.4 的 IP 地址。  
  
5.  单击“添加主机” 。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合身份验证服务器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[联合身份验证服务器的名称解析要求](https://technet.microsoft.com/library/dd807055.aspx)  
  

