---
title: "广告森林恢复-占用操作母版角色"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.technology: identity-adfs
ms.openlocfilehash: 7ca6e3746586feeb3573b1ad6ba02831d1f5addd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>广告森林恢复-占用操作主角色  

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 使用以下步骤占用运营主机角色（也称为了灵活一个主机操作 (FSMO) 作用）。 你可以使用 Ntdsutil.exe，自动安装所有域控制器一个命令行工具。  
  
## <a name="to-seize-an-operations-master-role"></a>若要占用操作主角色  
  
1.  在命令提示符下，键入以下命令，，然后按 ENTER:  
  
    ```  
    ntdsutil  
    ```  
  
2.  在**ntdsutil:**提示，键入以下命令，然后按 enter 键：  
  
    ```  
    roles  
    ```  
  
3.  在**FSMO 维护：**提示，键入以下命令，然后按 enter 键：  
  
    ```  
    connections  
    ```  
  
4.  在**服务器连接：**提示，键入以下命令，然后按 enter 键：  
  
    ```  
    Connect to server ServerFQDN  
    ```  
  
     其中*ServerFQDN*是完整的域名 (FQDN) 此 dc，例如：**连接到服务器 nycdc01.example.com**。  
  
     如果*ServerFQDN*不成功，则使用 NetBIOS DC 的名称。  
  
5.  在**服务器连接：**提示，键入以下命令，然后按 enter 键：  
  
    ```  
    quit  
    ```  
  
6.  这取决于你希望占用，角色在**FSMO 维护：**提示键入相应的命令，在下表中所述，然后按 ENTER。  
  
    |角色|凭据|命令|  
    |----------|-----------------|-------------|  
    |域名主机|企业管理员|**占用命名主机**|  
    |方案母版|方案管理员|**占用方案母版**|  
    |基础结构母版**注意：**占用主基础角色后，你可能会收到错误稍后如果需要运行 Adprep /Rodcprep。 有关详细信息，请参阅知识库文章[949257](https://support.microsoft.com/kb/949257)。|域管理员|**占用的基础结构母版**|  
    |PDC 仿真器母版|域管理员|**抓住 pdc**|  
    |删除母版|域管理员|**占用去掉母版**|  
  
     确认请求后，Active Directory 或广告 DS 尝试转移角色。 传输失败时，会出现错误的一些信息，并 Active Directory 或广告 DS 将继续在占用。 占用完成后，将显示列表中的角色和当前包含每个角色服务器轻型目录访问协议 (LDAP) 名称。 你也可以运行**Netdom 查询 FSMO**在提升了权限的命令提示符下，若要验证当前角色持有者。  
  
    > [!NOTE]
    >  如果这台计算机不是故障之前 RID 主机，尝试占用 RID 主角色计算机试图接受此角色前与复制合作伙伴同步。 但是，独立计算机时执行此步骤，因为它将无法成功地与合作伙伴同步。 因此，显示一个对话框询问你是否想要继续操作尽管不能够与合作伙伴同步此计算机。 单击**是**。  
  
## <a name="next-steps"></a>后续步骤

- [广告林恢复指南](AD-Forest-Recovery-Guide.md)
- [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)
