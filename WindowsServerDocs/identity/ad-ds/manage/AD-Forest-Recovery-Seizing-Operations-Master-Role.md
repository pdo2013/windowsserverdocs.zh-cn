---
title: AD 林恢复-占用操作主机角色
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.technology: identity-adds
ms.openlocfilehash: 1994d49652ee9eb10f6afc73cf5b4630b4718e77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858458"
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>AD 林恢复-占用操作主机角色  

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

使用以下过程占用操作主机角色 （也称为灵活单主机操作 (FSMO) 角色）。 您可以使用 Ntdsutil.exe，自动在所有域控制器安装的命令行工具。  
  
## <a name="to-seize-an-operations-master-role"></a>若要占用操作主机角色  
  
1. 在命令提示符下，键入以下命令，然后按 Enter：  

   ```  
   ntdsutil  
   ```  

2. 在**ntdsutil:** 提示符下，键入以下命令，然后按 ENTER:  

   ```  
   roles  
   ```  

3. 在**FSMO maintenance:** 提示符下，键入以下命令，然后按 ENTER:  

   ```  
   connections  
   ```  

4. 在**服务器连接：** 提示符下，键入以下命令，然后按 ENTER:  

   ```  
   Connect to server ServerFQDN  
   ```  

   其中*ServerFQDN*是此 DC 上的完全限定的域名 (FQDN)，例如：**连接到服务器 nycdc01.example.com**。  

   如果*ServerFQDN*不成功，则使用 DC 的 NetBIOS 名称。  

5. 在**服务器连接：** 提示符下，键入以下命令，然后按 ENTER:  

   ```  
   quit  
   ```  

6. 根据要占用，角色在**FSMO maintenance:** 提示符下，键入相应的命令下, 表中所述，然后按 ENTER。  
  
|角色|凭据|Command|  
|----------|-----------------|-------------|  
|域命名主机|Enterprise Admins|**占用命名主机**|  
|架构主机|Schema Admins|**占用架构主机**|  
|基础结构主机**注意：** 占用基础结构主机角色后，您可能如果你需要运行 Adprep /Rodcprep 更高版本会出错。 有关详细信息，请参阅知识库文章[949257](https://support.microsoft.com/kb/949257)。|Domain Admins|**占用结构主机**|  
|PDC 仿真器主机|Domain Admins|**占用 pdc**|  
|RID 主机|Domain Admins|**占用 rid 的主机**|  

确认请求后，Active Directory 或 AD DS 会尝试将此角色转移。 当传输失败时，将显示错误信息，和 Active Directory 或 AD DS 继续执行占用过程。 占用过程完成后，将显示角色以及当前包含的每个角色的服务器的轻型目录访问协议 (LDAP) 名称的列表。 您还可以运行**Netdom Query FSMO**的提升的命令提示符下，若要验证当前角色持有者。  
  
> [!NOTE]
> 如果此计算机不是在发生故障之前 RID 主机，而您试图占用 RID 主机角色，计算机会尝试再接受此角色与复制伙伴同步。 但是，在计算机隔离时执行此步骤，因为它不会成功同步和合作伙伴。 因此，一个对话框将出现询问您是否要继续操作而不考虑此计算机无法与合作伙伴同步。 单击 **“是”**。  
  
## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)
