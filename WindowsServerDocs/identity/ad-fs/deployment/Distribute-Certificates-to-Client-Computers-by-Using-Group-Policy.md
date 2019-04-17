---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: "使用组策略分发给客户端计算机证书"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d3a7e05e4d16565b17b69de254e353df749bbc3a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>使用组策略分发给客户端计算机证书

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012


你可以使用下面的过程推送合适的安全套接字层 \(SSL\) 证书 \（或等效的证书链为受信任的 root\）的帐户联合身份验证的服务器、资源联合身份验证的服务器，并为每个客户端计算机使用组策略帐户合作伙伴森林中的 Web 服务器。  
  
在会员**域管理员**或**企业管理员**，或、 Active Directory 域服务中的等效 \(AD DS\) 的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)\ (http:///\/ go.microsoft.com\/fwlink\ /？LinkId\ = 83477\)。   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>若要向客户端计算机证书分发使用组策略  
  
1.  在帐户合作伙伴公司的森林中的域控制器，开始**组策略管理**snap\ 中。  
  
2.  查找现有的组策略对象 \(GPO\) 或创建一个新的 GPO 包含证书设置。 确保 GPO 与域、网站或单位 \(OU\) 适当的用户和计算机帐户所在的位置。  
  
3.  Right\ 单击 GPO，然后单击**编辑**。  
  
4.  控制台树中，在打开**计算机 Configuration\\Policies\\Windows Settings\\Security Settings\\Public 键策略**，right\ 单击**信任根证书颁发机构**，然后单击**导入**。  
  
5.  在**欢迎证书导入向导**页上，单击**下一步**。  
  
6.  在**文件导入到**页上，键入相应证书文件路径 \ (例如，\\\fs1\\c$\\fs1.cer\)，然后单击**下一步**。  
  
7.  在**证书官方商城**页上，单击**放置在以下应用商店中的所有证书**，然后单击**下一步**。  
  
8.  在**完成证书导入向导**页上，你所提供的信息不准确，验证，然后单击**完成**。  
  
9. 重复执行步骤 2 到 6 添加其他证书的每一场中联合身份验证的服务器。  
