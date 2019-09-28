---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: 将联合服务器添加到联合服务器场
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 57c59241c36ba4ed657b83e7c691931f57978a7c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408529"
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>将联合服务器添加到联合服务器场


安装联合身份验证服务角色服务并在计算机上配置所需的证书后，你就可以将计算机配置为联合服务器了。 可以使用以下过程将计算机加入新的联合服务器场中。  
  
使用 AD FS 联合服务器配置向导将计算机加入到场。 当你使用此向导将计算机加入现有场时，计算机将配置 AD FS 配置数据库的 read @ no__t，并且它必须从主联合服务器接收更新。  
  
> [!NOTE]  
> 对于联合 Web one @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3 设计，在帐户伙伴组织中必须至少有一台联合服务器，在资源伙伴组织中必须至少有一个联合服务器。 有关详细信息，请参阅 [Where to Place a Federation Server](https://technet.microsoft.com/library/dd807127.aspx)。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  有关使用适当帐户和组成员身份的详细信息，请参阅[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477) \(http\/：\/\/go.microsoft.com\/fwlink？LinkId\=83477\)。   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>将联合服务器添加到联合服务器场  
  
1.  可以通过两种方式启动 AD FS 联合服务器配置向导。 若要启动该向导，请执行下列操作之一：  
  
    -   联合身份验证服务角色服务安装完成后，打开 AD FS 管理 snap @ no__t-0in，并单击 "**概述**" 页上或 "**操作**" 窗格中的 " **AD FS 联合服务器配置向导**" 链接。  
  
    -   安装向导完成后，请打开 Windows 资源管理器，导航到**C @no__t： 1Windows @ no__t-2ADFS**文件夹，并双击 @ no__t-3click **fsconfigwizard.exe**。  
  
2.  在“欢迎”页上，验证选择了“将联合服务器添加到现有联合身份验证服务”，然后单击“下一步”。  
  
3.  如果您选择的 AD FS 数据库已经存在，则会出现 "**检测到现有 AD FS 配置数据库**" 页。 如果显示该页，则单击“删除数据库”，然后单击“下一步”。  
  
    > [!CAUTION]  
    > 仅当你确定此 AD FS 数据库中的数据不重要，或者不在生产联合服务器场中使用时，才选择此选项。  
  
4.  在“指定主联合服务器和服务帐户”页的“主联合服务器名称”下，键入场中主联合服务器的计算机名，然后单击“浏览”。 在“浏览”对话框中，找到由现有联合服务器场中的所有其他联合服务器用作服务帐户的域帐户，然后单击“确定”。 键入密码并确认，然后单击 "**下一步**"：  
  
    > [!NOTE]  
    > 有关指定联合服务器场的服务帐户的详细信息，请参阅[手动配置联合服务器场的服务帐户](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)。 联合服务器场中的每个联合服务器都必须为要操作的场指定相同的服务帐户。 例如，如果创建的服务帐户是 contoso @ no__t-0ADFS2SVC，则为联合服务器角色配置的每台计算机以及将参与同一个场的计算机必须在联合服务器的此步骤中指定 contoso @ no__t-1ADFS2SVC要使场正常工作的配置向导。  
  
5.  在“已准备好应用设置”页上，查看详细信息。 如果设置正确，请单击 "**下一步**" 以开始配置具有这些设置的 AD FS。  
  
6.  在“配置结果” 页上，查看结果。 完成所有配置步骤后，单击“关闭”  以退出向导。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合服务器](Checklist--Setting-Up-a-Federation-Server.md)  
  

