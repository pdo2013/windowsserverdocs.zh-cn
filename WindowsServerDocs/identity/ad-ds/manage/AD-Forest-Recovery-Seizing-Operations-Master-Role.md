---
title: AD 林恢复-占用操作主机角色
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.technology: identity-adds
ms.openlocfilehash: 672dc119845acbe9cf38f82c793bd377d31db3b2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390285"
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>AD 林恢复-占用操作主机角色  

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

使用以下过程来占用操作主机角色（也称为灵活单主机操作（FSMO）角色）。 可以使用 Ntdsutil.exe，它是在所有 Dc 上自动安装的命令行工具。  
  
## <a name="to-seize-an-operations-master-role"></a>占用操作主机角色  
  
1. 在命令提示符下，键入以下命令，然后按 Enter：  

   ```  
   ntdsutil  
   ```  

2. 在**ntdsutil：** 提示符下，键入以下命令，然后按 enter：  

   ```  
   roles  
   ```  

3. 在 " **FSMO 维护：** " 提示符下，键入以下命令，然后按 enter：  

   ```  
   connections  
   ```  

4. 在 "**服务器连接：** " 提示符下，键入以下命令，然后按 enter：  

   ```  
   Connect to server ServerFQDN  
   ```  

   其中， *ServerFQDN*是此 DC 的完全限定的域名（FQDN），例如：**连接到服务器 nycdc01.example.com**。  

   如果*ServerFQDN*不成功，请使用 DC 的 NetBIOS 名称。  

5. 在 "**服务器连接：** " 提示符下，键入以下命令，然后按 enter：  

   ```  
   quit  
   ```  

6. 根据要占用的角色，在**FSMO 维护：** 提示符下，键入下表中所述的相应命令，然后按 enter。  
  
|Role|凭据|Command|  
|----------|-----------------|-------------|  
|域命名主机|Enterprise Admins|**占用命名主机**|  
|架构主机|Schema Admins|**占用架构主机**|  
|基础结构主机**说明：** 在占用基础结构主机角色后，如果需要运行 Adprep/Rodcprep.，则可能会收到错误。 有关详细信息，请参阅知识库文章[949257](https://support.microsoft.com/kb/949257)。|Domain Admins|**占用基础结构主机**|  
|PDC 模拟器主机|Domain Admins|**占用 pdc**|  
|RID 主机|Domain Admins|**占用 rid 主机**|  

确认请求后，Active Directory 或 AD DS 尝试传输角色。 当传输失败时，将显示一些错误信息，并 Active Directory 或 AD DS 继续进行强制。 在捕获完成后，将显示一个列表，其中包含当前包含每个角色的服务器的角色和轻型目录访问协议（LDAP）名称。 你还可以在提升的命令提示符下运行**Netdom QUERY FSMO**来验证当前角色持有者。  
  
> [!NOTE]
> 如果此计算机不是发生故障之前的 RID 主机，而你试图占用 RID 主机角色，则计算机会在接受此角色之前尝试与复制伙伴同步。 但是，由于在隔离计算机时执行此步骤，因此不会成功与合作伙伴同步。 因此，会出现一个对话框，询问您是否要继续该操作，而不考虑此计算机是否无法与合作伙伴同步。 单击 **“是”** 。  
  
## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)
