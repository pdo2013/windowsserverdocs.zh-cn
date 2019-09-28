---
title: AD 林恢复-配置 DNS 服务器服务
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2c1f2f68509c9136735fb13e24c86a1da40660eb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369249"
---
# <a name="ad-forest-recovery---configuring-the-dns-server-service"></a>AD 林恢复-配置 DNS 服务器服务

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

如果未在从备份还原的 DC 上安装 DNS 服务器角色，则必须安装和配置 DNS 服务器。 

## <a name="install-and-configure-the-dns-server-service"></a>安装和配置 DNS 服务器服务

完成还原后，为未作为 DNS 服务器运行的每个已还原 DC 完成此步骤。 

> [!NOTE]
> 如果从备份还原的 DC 正在运行 Windows Server 2008 R2，则必须将 DC 连接到隔离的网络，才能安装 DNS 服务器。 然后，将每个已还原的 DNS 服务器连接到相互共享的隔离网络。 运行 repadmin/replsum，以验证已还原的 DNS 服务器之间的复制是否正常工作。 验证复制后，你可以将还原的 Dc 连接到生产网络。如果已安装 DNS 服务器角色，你可以应用一个修补程序，使 DNS 服务器在服务器未连接到任何网络时能够启动。 应在自动生成过程中将修补程序补充到操作系统安装映像中。 有关修补程序的详细信息，请参阅 Microsoft 知识库中的[文章 975654](https://go.microsoft.com/fwlink/?LinkId=184691) （ https://go.microsoft.com/fwlink/?LinkId=184691) 。 

完成下面的安装和配置步骤。

### <a name="to-install-and-the-dns-server-service-using-server-manager"></a>使用服务器管理器安装和 DNS 服务器服务  

1. 打开服务器管理器，然后单击 "**添加角色和功能**"。 
2. 在“添加角色向导”中，如果出现“开始之前”页，请单击“下一步”。 
3. 在 "**安装类型**" 屏幕上选择 "**基于角色或基于功能的安装**"，然后单击 "**下一步**"。
4. 在 "**服务器选择**" 屏幕上，选择服务器，然后单击 "**下一步**"。
5. 如果系统提示，请在 "**服务器角色**" 屏幕**上，单击**"**添加功能**"，并单击 "**下一步**"。
6. 在**功能**屏幕上，单击 "**下一步**"。
7. 阅读 " **DNS 服务器**" 页上的信息，然后单击 "**下一步**"。
   ![DNS Server @ no__t-1  
8. 在 "**确认**" 页上，验证是否将安装 DNS 服务器角色，然后单击 "**安装**"。 

### <a name="to-configure-the-dns-server-service"></a>配置 DNS 服务器服务

1. 打开服务器管理器，单击 "**工具**"，然后单击 " **DNS**"。
   ![DNS Server @ no__t-1
2. 为在关键故障发生之前在 DNS 服务器上托管的相同 DNS 域名创建 DNS 区域。 有关详细信息，请参阅添加正向查找区域（[https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)）。
3. 配置在发生严重故障之前存在的 DNS 数据。 例如：  

   - 配置要存储在 AD DS 中的 DNS 区域。 有关详细信息，请参阅更改区域类型（[https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)）。
   - 将域控制器定位器（DC 定位程序）资源记录的权威 DNS 区域配置为允许安全动态更新。 有关详细信息，请参阅只允许安全动态更新（[https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)）。

4. 确保父 DNS 区域包含此 DNS 服务器上托管的子区域的委派资源记录（名称服务器（NS）和粘附主机（A）资源记录）。 有关详细信息，请参阅创建区域委派（[https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)）。
5. 配置 DNS 后，可以加快对 NETLOGON 记录的注册。

   > [!NOTE]
   > 只有全局编录服务器可用时，安全动态更新才起作用。 

   在命令提示符下，键入以下命令，然后按 Enter：  

   **net stop netlogon**  

6. 键入以下命令，然后按 Enter：  

   **net start netlogon**  

   ![DNS 服务器](media/AD-Forest-Recovery-Configure-DNS/dns3.png)  

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)
