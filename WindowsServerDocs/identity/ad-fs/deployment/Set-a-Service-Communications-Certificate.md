---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: 设置服务通信证书
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d0464853c73f88ed76545921ffc8a4bf8551c800
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408321"
---
# <a name="set-a-service-communications-certificate"></a>设置服务通信证书


Active Directory 联合身份验证服务中的联合服务器 \(AD FS @ no__t-1 使用服务通信证书来保护与 Web 客户端或联合服务器的安全套接字层 \(SSL @ no__t 通信的 Web 服务流量转发.

> [!NOTE]  
> 服务通信证书与 SSL 证书不同。 若要更改 AD FS SSL 证书，你将需要使用 Powershell。 按照[本文](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap)中的指导进行操作。


你可以使用以下过程来更改服务通信证书与 AD FS 管理 snap @ no__t-0in。  

> [!NOTE]  
> AD FS 管理 snap @ no__t-0in 是指作为服务通信证书的联合服务器的服务器身份验证证书。  

本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  有关使用适当帐户和组成员身份的详细信息，请参阅[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477) \(http\/：\/\/go.microsoft.com\/fwlink？LinkId\=83477\)。   

### <a name="to-set-a-service-communications-certificate"></a>设置服务通信证书  

1.  在 "**开始**" 屏幕上，键入 "**AD FS 管理**"，然后按 enter。  

2.  在控制台树中，双击 "no__t-0click **Service**"，然后单击 "**证书**"。  

3.  在 "**操作**" 窗格中，单击 "**设置服务通信证书**" 链接。  

4.  在 "**选择服务通信证书**" 对话框中，导航到要设置为服务通信证书的证书文件，选择证书文件，然后单击 "**打开**"。  

## <a name="additional-references"></a>其他参考  
[清单：设置联合服务器](Checklist--Setting-Up-a-Federation-Server.md)  

[联合服务器的证书要求](https://technet.microsoft.com/library/dd807040.aspx)  
