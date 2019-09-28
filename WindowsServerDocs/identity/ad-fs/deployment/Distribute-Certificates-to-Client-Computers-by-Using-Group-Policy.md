---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: 使用组策略将证书分发到客户端计算机
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1d9692bc099174f15b77e792087f4c7055bf85d1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359621"
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>使用组策略将证书分发到客户端计算机


你可以使用以下过程向下推送适当的安全套接字层 \(SSL @ no__t 证书，\(or 等效证书，该证书链接到帐户联合服务器、资源联合服务器和的受信任的根 @ no__t使用组策略将 Web 服务器指向帐户伙伴林中的每台客户端计算机。  
  
**Domain admins**或**Enterprise Admins**中的成员身份或同等身份 ACTIVE DIRECTORY 域服务 \(AD DS @ no__t 是完成此过程的最低要求。  有关使用适当帐户和组成员身份的详细信息，请参阅[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477) \(http\/：\/\/go.microsoft.com\/fwlink？LinkId\=83477\)。   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>使用组策略将证书分发到客户端计算机  
  
1.  在帐户伙伴组织的林中的域控制器上，启动**组策略管理**"" 管理 no__t-1in "。  
  
2.  查找 \(GPO @ no__t 的现有组策略对象，或创建一个新的 GPO 以包含证书设置。 确保 GPO 与相应的用户和计算机帐户所在的域、站点或组织单位 \(OU @ no__t-1 相关联。  
  
3.  右键单击 "no__t-0click"，然后单击 "**编辑**"。  
  
4.  在控制台树中，打开 "**计算机配置 @ no__t-1Policies @ no__t-2Windows Settings @ no__t-3Security 设置 @ no__t-4Public Key 策略**"，右键 @ no__t-5Click**受信任的根证书颁发机构**，然后单击 "**导入"** .  
  
5.  在 "**欢迎使用证书导入向导**" 页上，单击 "**下一步**"。  
  
6.  在 "**要导入的文件**" 页上，键入相应的证书文件的路径 \(for 示例，\\ @ no__t-3fs1 @ no__t-4c $ \\fs1 @ no__t-6，然后单击 "**下一步**"。  
  
7.  在 "**证书存储**" 页上，单击 **"将所有证书放入下列存储**"，然后单击 "**下一步**"。  
  
8.  在 "**正在完成证书导入向导**" 页上，验证提供的信息是否正确，然后单击 "**完成**"。  
  
9. 重复步骤2到步骤6，为场中的每台联合服务器添加其他证书。  
