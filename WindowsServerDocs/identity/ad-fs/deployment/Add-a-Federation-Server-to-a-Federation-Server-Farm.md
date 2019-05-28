---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: 将联合服务器添加到联合服务器场
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 040caf6395b7c70313de900d522241f97699a999
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192500"
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>将联合服务器添加到联合服务器场


在安装联合身份验证服务角色服务并在计算机上配置所需的证书后，你现可配置要成为联合身份验证服务器的计算机。 可以使用以下过程将计算机加入新的联合服务器场中。  
  
计算机加入场的 AD FS 联合身份验证服务器配置向导。 当您使用此向导将计算机加入到现有场时，计算机都配置有读取\-只有 AD FS 配置数据库和它的副本必须从主联合服务器接收更新。  
  
> [!NOTE]  
> 对于联合 Web 单一\-符号\-上\(SSO\)设计中，您必须在帐户伙伴组织中的至少一台联合服务器和资源伙伴组织中的至少一台联合服务器. 有关详细信息，请参阅 [Where to Place a Federation Server](https://technet.microsoft.com/library/dd807127.aspx)。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  查看详细了解如何使用适当帐户和组成员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/？LinkId\=83477\)。   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>若要将联合身份验证服务器添加到联合服务器场  
  
1.  有两种方法启动 AD FS 联合身份验证服务器配置向导。 若要启动该向导，请执行下列操作之一：  
  
    -   联合身份验证服务角色服务安装完成后，打开 AD FS 管理管理单元\-中，单击**AD FS 联合身份验证服务器配置向导**链接**概述**页中或在**操作**窗格。  
  
    -   安装向导已完成，请打开 Windows 资源管理器后，随时导航到**c:\\Windows\\ADFS**文件夹，并双击\-单击**FsConfigWizard.exe**.  
  
2.  在“欢迎”  页上，验证选择了“将联合服务器添加到现有联合身份验证服务”  ，然后单击“下一步”  。  
  
3.  如果已选择的 AD FS 数据库存在，则**AD FS 配置数据库检测到现有**页将出现。 如果显示该页，则单击“删除数据库”  ，然后单击“下一步”  。  
  
    > [!CAUTION]  
    > 仅当你确信此 AD FS 数据库中的数据并不重要，或者它未使用生产联合服务器场中时，请选择此选项。  
  
4.  在“指定主联合服务器和服务帐户”  页的“主联合服务器名称”  下，键入场中主联合服务器的计算机名，然后单击“浏览”  。 在“浏览”  对话框中，找到由现有联合服务器场中的所有其他联合服务器用作服务帐户的域帐户，然后单击“确定”  。 键入的密码和确认，然后依次**下一步**:  
  
    > [!NOTE]  
    > 有关指定联合服务器场的服务帐户的详细信息，请参阅[手动配置联合服务器场服务帐户](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)。 联合服务器场中的每个联合身份验证服务器必须指定可操作的服务器场的相同服务帐户。 例如，如果已创建的服务帐户为 contoso\\ADFS2SVC，您将配置为联合身份验证服务器角色以及将参与相同场每台计算机必须指定 contoso\\ADFS2SVC 在此步骤中的步骤联合身份验证服务器配置向导为场的可操作。  
  
5.  在“已准备好应用设置”  页上，查看详细信息。 如果设置正确，请单击**下一步**以开始使用这些设置配置 AD FS。  
  
6.  在“配置结果”  页上，查看结果。 完成所有配置步骤后，单击“关闭”   以退出向导。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合服务器](Checklist--Setting-Up-a-Federation-Server.md)  
  

