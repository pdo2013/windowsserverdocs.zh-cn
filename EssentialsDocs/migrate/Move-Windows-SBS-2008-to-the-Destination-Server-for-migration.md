---
title: 将 Windows SBS 2008 设置和数据移到目标服务器以进行 Windows Server Essentials 迁移
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4950469d-d800-430d-8d10-53bafc4a9932
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 82a7a5b3ce3662574260379bc893da484baf1caa
ms.sourcegitcommit: 02f1e11ba37a83e12d8ffa3372e3b64b20d90d00
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68863412"
---
# <a name="move-windows-sbs-2008-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>将 Windows SBS 2008 设置和数据移到目标服务器以进行 Windows Server Essentials 迁移

>适用于：Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

将设置和数据移到目标服务器，如下所示：：

1. [将数据复制到目标服务器](#copy-data-to-the-destination-server)

2. [将 Active Directory 用户帐户导入 Windows Server Essentials 仪表板 (可选)](#import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard)

3. [将 DHCP 服务器角色从源服务器移到路由器](#move-the-dhcp-server-role-from-the-source-server-to-the-router)

4. [配置网络](#configure-the-network)

5. [删除旧版 Active Directory 组策略对象 (可选)](#remove-legacy-active-directory-group-policy-objects)

6. [将允许的计算机映射到用户帐户](#map-permitted-computers-to-user-accounts)

## <a name="copy-data-to-the-destination-server"></a>将数据复制到目标服务器
在将数据从源服务器复制到目标服务器之前，请执行以下任务：

- 在源服务器上查看共享文件夹列表（包括每个文件夹的权限）。 在目标服务器上创建或自定义这些文件夹，使其与要从源服务器迁移的文件夹结构相匹配。 

- 查看每个文件夹的大小，并确保目标服务器具有足够的存储空间。 

- 使源服务器上的共享文件夹对所有用户限制为只读，以便在将文件复制到目标服务器时，不会在驱动器上发生写入操作。

#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>将数据从源服务器复制到目标服务器

1. 以域管理员身份登录到目标服务器，然后打开一个命令窗口。 

2. 在命令提示符下，键入以下命令，然后按 Enter： 

    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt` 

 其中：
 - \<SourceServerName\>是源服务器的名称
 - \<Sharedsourcefoldername&gt\>是源服务器上共享文件夹的名称
 - \<Destinationservername&gt\>是目标服务器的名称,
 - \<Shareddestinationfoldername&gt\>是将数据复制到的目标服务器上的共享文件夹。 

3. 对每个要从源服务器迁移的共享文件夹重复上一步。 

## <a name="import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard"></a>将 Active Directory 用户帐户导入到 Windows Server Essentials 仪表板
 默认情况下, 在源服务器上创建的所有用户帐户都会自动迁移到 Windows Server Essentials 中的仪表板。 但是，如果某些属性无法满足迁移要求，则 Active Directory 用户帐户的自动迁移将会失败。 可以使用以下 Windows PowerShell cmdlet 导入 Active Directory 用户。 

#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>将 Active Directory 用户帐户导入到 Windows Server Essentials 仪表板 
 
1. 以域管理员身份登录到目标服务器。 
 
2. 以管理员身份打开 Windows PowerShell。 
 
3. 运行以下 cmdlet，其中 `[AD username]` 是你要导入的 Active Directory 用户帐户的名称： 
 
 `Import-WssUser SamAccountName [AD username]` 
 
## <a name="move-the-dhcp-server-role-from-the-source-server-to-the-router"></a>将 DHCP 服务器角色从源服务器移到路由器
 如果源服务器正在运行 DHCP 角色，则请执行下列步骤以将 DHCP 角色移到路由器。 
 
#### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>将 DHCP 角色从源服务器移到路由器 
 
1. 关闭源服务器上的 DHCP 服务，如下所示： 

    1. 在源服务器上，依次单击“开始”、“管理工具”，然后单击“服务”。 

    2. 在当前运行的服务列表中，右键单击“DHCP 服务器”，然后单击“属性”。 

    3. 对于“启动类型”，选择“禁用”。

    4. 停止服务。

2. 启用路由器上的 DHCP 角色。

    1. 按照路由器文档中的说明，启用路由器上的 DHCP 角色。 

    2. 若要确保源服务器颁发的 IP 地址保持不变，请按照路由器文档中的说明，将路由器上的 DHCP 范围配置为与源服务器上的 DHCP 范围相同。 

 > [!IMPORTANT]
 > 如果你尚未为目标服务器在路由器上设置静态 IP 或 DHCP 保留，并且 DHCP 范围与源服务器不同，则可能路由器将为目标服务器颁发一个新的 IP 地址。 如果发生这种情况，则请重置路由器的端口转发规则，以转发到目标服务器的新 IP 地址。 

## <a name="configure-the-network"></a>配置网络
 将 DHCP 角色移到路由器之后，请在目标服务器上配置网络设置。 
 
#### <a name="to-configure-the-network"></a>配置网络
 
1. 在目标服务器上，打开仪表板。
 
2. 在仪表板“主页” 页面上，单击“设置”，单击“设置随处访问”，然后选择“单击以配置随处访问” 选项。 
 
3. 完成向导中的说明，配置你的路由器名和域名。 
 
 如果路由器不支持 UPnP 框架，或如果 UPnP 框架已禁用，则路由器名称旁边可能会出现一个黄色的警告图标。 请确保下列端口为打开状态，并且已定向到目标服务器的 IP 地址： 
 
- 端口 80：HTTP Web 流量 
 
- 端口 443:HTTPS Web 流量 
 
> [!NOTE]
> 如果已在第二个服务器上设置了本地 Exchange 服务器，则必须确保端口 25（适用于 SMTP）也已打开，并且被重定向到本地 Exchange 服务器的 IP 地址。
 
## <a name="remove-legacy-active-directory-group-policy-objects"></a>删除旧 Active Directory 组策略对象
为 Windows Server Essentials 更新组策略对象 (Gpo)。 它们是包含 Windows SBS 2008 GPO 的一个超集。 对于 Windows Server Essentials, 必须手动删除大量 Windows SBS 2008 Gpo 和 Windows Management Instrumentation (WMI) 筛选器, 以防止与 Windows Server Essentials Gpo 和 WMI 筛选器发生冲突。 
 
> [!NOTE]
> 如果已修改原始的 Windows SBS 2008 组策略对象，则应该将其副本保存到其他位置，然后将其从 Windows SBS 2008 删除。 
 
#### <a name="to-remove-old-group-policy-objects-from-windows-sbs-2008"></a>从 Windows SBS 2008 删除旧的组策略对象 
 
1. 使用管理员帐户登录到源服务器。 
 
2. 单击“开始”，然后单击“服务器管理”。 
 
3. 在导航窗格中, 依次单击 "**高级管理**"、"**组策略管理**", 然后单击 "**林:** _\>< YourDomainName_"。 
 
4. 单击 "**域**", 单击 " *<\>YourDomainName*", 然后单击 "**组策略对象**"。 
 
5. 右键单击“Small Business Server 审核策略”，单击“删除”，然后单击“确定”。 
 
6. 重复步骤 5，删除下列应用于你的网络的 GPO： 
 
 - Small Business Server 客户端计算机 
 
 - Small Business Server 域密码策略 
 
建议你在 Windows Server Essentials 中配置密码策略以强制实施强密码。 若要配置密码策略，请使用仪表板，后者会将配置写入默认域策略。 密码策略配置不会写入 Small Business Server 域密码策略对象中（与 Windows SBS 2008 中的情况相同）。 
 
 - Small Business Server Internet 连接防火墙 
 
 - Small Business Server 锁定策略 
 
 - Small Business Server 远程协助策略 
 
 - Small Business Server Windows 防火墙 
 
 - Small Business Server 更新服务客户端计算机策略 
 
 如果从 Windows SBS 2008 R2 进行迁移，则将会显示此 GPO。 
 
 - Small Business Server 更新服务常见设置策略 
 
 如果从 Windows SBS 2008 R2 进行迁移，则将会显示此 GPO。 
 
 - Small Business Server 更新服务服务器计算机策略 
 
 如果从 Windows SBS 2008 R2 进行迁移，则将会显示此 GPO。 
 
7. 确认已删除所有的 GPO。 
 
#### <a name="to-remove-wmi-filters-from-windows-sbs-2008"></a>从 Windows SBS 2008 删除 WMI 过滤器
 
1. 使用管理员帐户登录到源服务器。 
 
2. 单击“开始”，然后单击“服务器管理”。 
 
3. 在导航窗格中, 依次单击 "**高级管理**"、"**组策略管理**", 然后单击 "**林:** _\> < YourNetworkDomainName_ 
 
4. 单击 "**域**", 单击 " *< YourNetworkDomainName\>* ", 然后单击 " **WMI 筛选器**"。 
 
5. 右键单击“PostSP2”，单击“删除”，然后单击“是”。 
 
6. 右键单击“PreSP2”，单击“删除”，然后单击“是”。 
 
7. 确认已删除这三个 WMI 过滤器。 
 
## <a name="map-permitted-computers-to-user-accounts"></a>将允许的计算机映射到用户帐户
在 Windows SBS 2008 中，如果用户连接到远程 Web 访问，则将显示网络中的所有计算机。 这可能包括用户没有权限进行访问的计算机。 在 Windows Server Essentials 中, 必须将用户明确分配给计算机, 以使它显示在远程 Web 访问中。 从 Windows SBS 2008 迁移的每个用户帐户都必须映射到一台或多台计算机。 
 
#### <a name="to-map-user-accounts-to-computers"></a>将用户帐户映射到计算机 
 
1. 打开 Windows Server Essentials 仪表板。
 
2. 在导航栏中，单击“用户”。
 
3. 在用户帐户列表中，右键单击用户帐户，然后单击“查看帐户属性”。
 
4. 单击“随处访问” 选项卡，然后单击”允许远程 Web 访问和访问 Web 服务应用程序”。
 
5. 依次选择“共享文件夹”、“计算机”和“主页链接”，然后单击“应用”。
 
6. 单击“计算机访问”选项卡，然后单击要允许访问的计算机的名称。
 
7. 为每个用户帐户重复步骤 3、4、5 和 6。 

> [!NOTE]
> 不必更改客户端计算机的配置。 它将进行自动配置。 
>
> 完成迁移后，如果你在目标服务器上创建第一个新的用户帐户时遇到问题，则请删除你已添加的用户帐户，然后再次创建它。
