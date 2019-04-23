---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: 通过使用组策略分发到客户端计算机的证书
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d3a7e05e4d16565b17b69de254e353df749bbc3a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839228"
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>通过使用组策略分发到客户端计算机的证书

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012


可以使用以下过程来推送适当的安全套接字层\(SSL\)证书\(等效项链接到受信任的根证书或\)帐户联合身份验证服务器资源联合身份验证服务器，并通过使用组策略的帐户伙伴林中每个客户端计算机的 Web 服务器。  
  
中的成员身份**Domain Admins**或**Enterprise Admins**，或，Active Directory 域服务中的等效\(AD DS\)身份是完成此过程所需的最低。  查看详细了解如何使用适当帐户和组成员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/？LinkId\=83477\)。   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>将证书分发到客户端计算机通过使用组策略  
  
1.  在帐户伙伴组织的林中的域控制器上, 启动**组策略管理**对齐\-中。  
  
2.  找不到现有的组策略对象\(GPO\)或创建新的 GPO 以便包含证书设置。 确保 GPO 与域、 站点或组织单位相关联\(OU\)适当的用户和计算机帐户所在的位置。  
  
3.  右\-单击该 GPO，然后单击**编辑**。  
  
4.  在控制台树中，打开**计算机配置\\策略\\Windows 设置\\安全设置\\公钥策略**，右键\-单击**受信任的根证书颁发机构**，然后单击**导入**。  
  
5.  上**欢迎使用证书导入向导**页上，单击**下一步**。  
  
6.  上**导入的文件**页上，键入相应的证书文件的路径\(等\\ \\fs1\\c$\\fs1.cer\)，然后单击**下一步**。  
  
7.  上**证书存储区**页上，单击**将所有证书都放入下列存储**，然后单击**下一步**。  
  
8.  上**正在完成证书导入向导**页上，验证你提供的信息是准确的，然后单击**完成**。  
  
9. 重复步骤 2 到 6 来为每个联合服务器场中添加额外的证书。  
