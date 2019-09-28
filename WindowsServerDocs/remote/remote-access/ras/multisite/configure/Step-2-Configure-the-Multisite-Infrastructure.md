---
title: 步骤2配置多站点基础结构
description: 本主题是在 Windows Server 2016 中的多站点部署中部署多台远程访问服务器指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: faec70ac-88c0-4b0a-85c7-f0fe21e28257
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b345ce7cdbb0cf9ff91ec99275232da5ba34edb0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367204"
---
# <a name="step-2-configure-the-multisite-infrastructure"></a>步骤2配置多站点基础结构

>适用于：Windows Server 2012 R2、Windows Server 2012

若要配置多站点部署，需要执行许多步骤来修改网络基础结构设置，包括配置其他 Active Directory 站点和域控制器、配置其他安全组以及配置如果未使用自动配置的 Gpo，则组策略对象（Gpo）。  
  
|任务|描述|  
|----|--------|  
|2.1. 配置其他 Active Directory 站点|为部署配置其他 Active Directory 站点。|  
|2.2. 配置其他域控制器|根据需要配置其他 Active Directory 域控制器。|  
|2.3. 配置安全组|为任何 Windows 7 客户端计算机配置安全组。|  
|2.4. 配置 GPO|根据需要配置其他组策略对象。|  
  
> [!NOTE]  
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="BKMK_ConfigAD"></a>2.1。 配置其他 Active Directory 站点  
所有入口点都可以驻留在单个 Active Directory 站点中。 因此，在多站点配置中，必须至少有一个 Active Directory 站点来实现远程访问服务器。 如果需要创建第一个 Active Directory 站点，或如果想要为多站点部署使用其他 Active Directory 站点，请使用此步骤。 使用 "Active Directory 站点和服务" 管理单元在组织的网络中创建新站点。  

林中的**Enterprise Admins**组或林根域中的**Domain admins**组的成员身份或同等身份至少需要完成此过程。 可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。  

有关详细信息，请参阅向[林中添加站点](https://technet.microsoft.com/library/cc732761.aspx)。  

### <a name="to-configure-additional-active-directory-sites"></a>配置其他 Active Directory 站点  
  
1.  在主域控制器上，单击 "**开始**"，然后单击 " **Active Directory 站点和服务**"。  
  
2.  在 "Active Directory 站点和服务" 控制台的控制台树中，右键单击 "**站点**"，然后单击 "**新建站点**"。  
  
3.  在 "**新建对象-站点**" 对话框的 "**名称**" 框中，输入新站点的名称。  
  
4.  在 "**链接名称**" 中，单击站点链接对象，然后单击 **"确定"** 两次。  
  
5.  在控制台树中，展开 "**站点**"，右键单击 "**子网**"，然后单击 "**新建子网**"。  
  
6.  在 "**新建对象-子网**" 对话框中的 "**前缀**" 下，键入 IPv4 或 IPv6 子网前缀，在 "**为此前缀选择一个站点对象**" 列表中，单击要与此子网关联的站点，然后单击 **"确定"** 。  
  
7.  重复步骤5和步骤6，直到创建了部署中所需的所有子网。  
  
8.  关闭 Active Directory 站点和服务。  
  
@no__t 0Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)***<em>Windows powershell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
若要安装 Windows 功能 "Active Directory module for Windows PowerShell"：  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
或通过 OptionalFeatures 添加 "Active Directory PowerShell 管理单元"。  
  
如果在 Windows 7 上运行以下 cmdlet 或 Windows Server 2008 R2，则必须导入 Active Directory PowerShell 模块：  
  
```  
Import-Module ActiveDirectory  
```  
  
使用内置 DEFAULTIPSITELINK 配置名为 "Second" 的 Active Directory 网站：  
  
```  
New-ADReplicationSite -Name "Second-Site"  
Set-ADReplicationSiteLink -Identity "DEFAULTIPSITELINK" -sitesIncluded @{Add="Second-Site"}  
```  
  
若要为第二站点配置 IPv4 和 IPv6 子网，请执行以下操作：  
  
```  
New-ADReplicationSubnet -Name "10.2.0.0/24" -Site "Second-Site"  
New-ADReplicationSubnet -Name "2001:db8:2::/64" -Site "Second-Site"  
```  
  
## <a name="BKMK_AddDC"></a>2.2。 配置其他域控制器  
若要在单个域中配置多站点部署，建议你在部署中为每个站点至少有一个可写域控制器。  
  
若要执行此过程，至少必须是要在其中安装域控制器的域中的 Domain Admins 组的成员。  
  
有关详细信息，请参阅[安装附加的域控制器](https://technet.microsoft.com/library/cc733027.aspx)。
  
### <a name="to-configure-additional-domain-controllers"></a>配置其他域控制器  
  
1.  在将充当域控制器的服务器上，在**服务器管理器**的**仪表板**上，单击 "**添加角色和功能**"。  
  
2.  单击 "**下一步**" 三次以转到服务器角色选择屏幕  
  
3.  在 "**选择服务器角色**" 页上，选择**Active Directory 域服务**。 在出现提示时单击 "**添加功能**"，然后单击 "**下一步**" 三次。  
  
4.  在“确认”页上，单击“安装”。  
  
5.  安装成功完成后，单击 "**将此服务器提升为域控制器**"。  
  
6.  在 Active Directory 域服务配置向导的 "**部署配置**" 页上，单击 "向**现有域添加域控制器**"。  
  
7.  在 "**域**" 中输入域名;例如，corp.contoso.com。  
  
8.  在 "**提供用于执行此操作的凭据**" 下，单击 "**更改**"。 在 " **Windows 安全性**" 对话框中，提供可安装其他域控制器的帐户的用户名和密码。 若要安装其他域控制器，你必须是 Enterprise Admins 组或 Domain Admins 组的成员。 提供凭据后，单击 **“下一步”** 。  
  
9. 在 "**域控制器选项**" 页上，执行以下操作：  
  
    1.  进行以下选择：  
  
        -   **域名系统（DNS）服务器**"此选项默认情况下处于选中状态，以便域控制器可以充当域名系统（DNS）服务器。 如果你不希望该域控制器成为 DNS 服务器，则清除该选项。  
  
            如果未在目录林根级域中的主域控制器（PDC）模拟器上安装 DNS 服务器角色，则在其他域控制器上安装 DNS 服务器的选项将不可用。 作为此情况的一种解决方法，你可以在安装 AD DS 之前或之后安装 DNS 服务器角色。  
  
            > [!NOTE]  
            > 如果选择 "安装 DNS 服务器" 选项，则可能会收到一条消息，指示无法创建 DNS 服务器的 DNS 委派，你应手动创建 dns 服务器的 DNS 委派，以确保可靠的名称解析。 如果在目录林根级域或树根域中安装其他域控制器，则无需创建 DNS 委派。 在这种情况下，请单击 **"是"** 并忽略该消息。  
  
        -   **全局编录（GC）** "此选项默认情况下处于选中状态。 它会将全局编录、只读目录分区添加到域控制器，并且将启用全局编录搜索功能。  
  
        -   **只读域控制器（RODC）** "默认情况下不选择此选项。 它使其他域控制器成为只读的;也就是说，它使域控制器成为 RODC。  
  
    2.  在 "**站点名称**" 中，从列表中选择一个站点。  
  
    3.  在 **"键入目录服务还原模式（DSRM）密码**" 下的 "**密码**" 和 "**确认密码**" 中，键入强密码两次，然后单击 "**下一步**"。 对于必须脱机执行的任务，必须使用此密码在 DSRM 中启动 AD DS。  
  
10. 如果要在角色安装过程中更新 DNS 委派，请在 " **Dns 选项**" 页上选中 "**更新 dns 委派**" 复选框，然后单击 "**下一步**"。  
  
11. 在 "**其他选项**" 页上，键入或浏览到数据库文件、目录服务日志文件和系统卷（SYSVOL）文件的卷和文件夹位置。 根据需要指定复制选项，然后单击 "**下一步**"。  
  
12. 在 "**查看选项**" 页上，查看安装选项，然后单击 "**下一步**"。  
  
13. 在 "**先决条件检查**" 页上，验证必备项后，单击 "**安装**"。  
  
14. 等待向导完成配置，然后单击 "**关闭**"。  
  
15. 如果计算机未自动重新启动，则重新启动计算机。  
  
## <a name="BKMK_ConfigSG"></a>2.3。 配置安全组  
多站点部署需要适用于 Windows 7 客户端计算机的其他安全组，该安全组允许对 Windows 7 客户端计算机进行访问。 如果有多个域包含 Windows 7 客户端计算机，则建议在每个域中为同一入口点创建一个安全组。 另外，还可以使用一个包含两个域中的客户端计算机的通用安全组。 例如，在包含两个域的环境中，如果你希望允许在入口点1和3中访问 Windows 7 客户端计算机，但不允许在入口点2中访问，则创建两个新的安全组以包含 Windows 7 客户端计算机，以便为每个 网域.  
  
### <a name="to-configure-additional-security-groups"></a>配置其他安全组  
  
1.  在主域控制器上，单击 "**开始**"，然后单击 " **Active Directory 用户和计算机**"。  
  
2.  在控制台树中，右键单击要在其中添加新组的文件夹，例如，corp.contoso.com/Users。 指向“新建”，然后单击“组”。  
  
3.  在 "**新建对象-组**" 对话框中的 "**组名称**" 下，键入新组的名称，例如 Win7_Clients_Entrypoint1。  
  
4.  在 "**组作用域**" 下，单击 "**通用**"，在 "**组类型**" 下，单击 "**安全**"，然后单击 **"**  
  
5.  若要将计算机添加到新的安全组，请双击安全组，然后在 " **< Group_Name" > 属性**"对话框中，单击"**成员**"选项卡。  
  
6.  在“成员” 选项卡上，单击“添加”。  
  
7.  选择要添加到此安全组的 Windows 7 计算机，然后单击 **"确定"** 。  
  
8.  重复此过程以根据需要为每个入口点创建一个安全组。  
  
@no__t 0Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)***<em>Windows powershell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
若要安装 Windows 功能 "Active Directory module for Windows PowerShell"：  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
或通过 OptionalFeatures 添加 "Active Directory PowerShell 管理单元"。  
  
如果在 Windows 7 上运行以下 cmdlet 或 Windows Server 2008 R2，则必须导入 Active Directory PowerShell 模块：  
  
```  
Import-Module ActiveDirectory  
```  
  
若要配置名为 Win7_Clients_Entrypoint1 的安全组并添加名为 CLIENT2 的客户端计算机：  
  
```  
New-ADGroup -GroupScope universal -Name Win7_Clients_Entrypoint1  
Add-ADGroupMember -Identity Win7_Clients_Entrypoint1 -Members CLIENT2$  
```  
  
## <a name="ConfigGPOs"></a>2.4。 配置 GPO  
多站点远程访问部署需要以下组策略对象：  
  
-   远程访问服务器的每个入口点的 GPO。  
  
-   适用于每个域的任何 Windows 8 客户端计算机的 GPO。  
  
-   每个域中包含 Windows 7 客户端计算机的 GPO，每个入口点都配置为支持 Windows 7 客户端。  
  
    > [!NOTE]  
    > 如果没有任何 Windows 7 客户端计算机，则不需要为 Windows 7 计算机创建 Gpo。  
  
当你配置远程访问时，向导将自动创建所需的组策略对象（如果它们不存在）。 如果你没有创建组策略对象所需的权限，则必须在配置远程访问之前创建它们。 DirectAccess 管理员必须对 Gpo 具有完全权限（"编辑 + 修改安全性 + 删除"）。  
  
> [!IMPORTANT]  
> 在手动创建用于远程访问的 Gpo 后，您必须为与远程访问服务器相关联的 Active Directory 站点中的域控制器 Active Directory 和 DFS 复制留出足够的时间。 如果远程访问自动创建了组策略对象，则无需等待时间。  
  
若要创建组策略对象，请参阅[创建和编辑组策略对象](https://technet.microsoft.com/library/cc754740.aspx)。  
  
### <a name="DCMaintandDowntime"></a>域控制器维护和停机时间  
如果域控制器作为 PDC 模拟器运行，或者域控制器管理服务器 Gpo 时遇到停机，则不可能加载或修改远程访问配置。 如果其他域控制器可用，则这不会影响客户端连接。  
  
若要加载或修改远程访问配置，可以将 PDC 仿真器角色转移到客户端或应用程序服务器 Gpo 的其他域控制器;对于服务器 Gpo，请更改用于管理服务器 Gpo 的域控制器。  
  
> [!IMPORTANT]  
> 此操作只能由域管理员执行。 更改主域控制器的影响并不局限于远程访问;因此，在传输 PDC 仿真器角色时要格外小心。  
  
> [!NOTE]  
> 在修改域控制器关联之前，请确保已将远程访问部署中的所有 Gpo 复制到域中的所有域控制器。 如果未同步 GPO，则在修改域控制器关联后可能会丢失最近的配置更改，这可能会导致配置损坏。 若要验证 GPO 同步，请参阅[检查组策略基础结构状态](https://technet.microsoft.com/library/jj134176.aspx)。  
  
#### <a name="TransferPDC"></a>传输 PDC 仿真器角色  
  
1.  在 "**开始**" 屏幕上，键入**dsa.msc**，然后按 enter。  
  
2.  在 Active Directory 用户和计算机 "控制台的左窗格中，右键单击"**用户和计算机 "Active Directory**，然后单击"**更改域控制器**"。 在 "更改目录服务器" 对话框中，单击 "**此域控制器" 或 "AD LDS 实例**"，在列表中单击将成为新角色持有者的域控制器，然后单击 **"确定"** 。  
  
    > [!NOTE]  
    > 如果你不在要将角色传输到的域控制器上，则必须执行此步骤。 如果已连接到要将角色传输到的域控制器，请不要执行此步骤。  
  
3.  在控制台树中，右键单击 "**用户和计算机" Active Directory**，指向 "**所有任务**"，然后单击 "**操作主机**"。  
  
4.  在 "操作主机" 对话框中，单击 " **PDC** " 选项卡，然后单击 "**更改**"。  
  
5.  单击 **"是"** 以确认要传输该角色，然后单击 "**关闭**"。  
  
#### <a name="ChangeDC"></a>更改管理服务器 Gpo 的域控制器  
  
-   在远程访问服务器上运行 Windows PowerShell cmdlet `HYPERLINK "https://technet.microsoft.com/library/hh918412.aspx" Set-DAEntryPointDC`，并指定*ExistingDC*参数的无法访问的域控制器名称。 此命令修改当前由该域控制器管理的入口点的服务器 Gpo 的域控制器关联。  
  
    -   若要将无法访问的域控制器 "dc1.corp.contoso.com" 替换为域控制器 "dc2.corp.contoso.com"，请执行以下操作：  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "NewDC 'dc2.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
    -   若要使用最近 Active Directory 站点中的域控制器将无法访问的域控制器 "dc1.corp.contoso.com" 替换到远程访问服务器 "DA1.corp.contoso.com"，请执行以下操作：  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
### <a name="ChangeTwoDCs"></a>更改两个或更多管理服务器 Gpo 的域控制器  
在极少数情况下，管理服务器 Gpo 的两个或多个域控制器不可用。 如果出现这种情况，则需要执行更多步骤来更改服务器 Gpo 的域控制器关联。  
  
域控制器关联信息既存储在远程访问服务器的注册表中，也存储在所有服务器 Gpo 中。 在下面的示例中，有两个入口点，其中包含两个远程访问服务器： "DA1"，在 "入口点 1" 和 "DA2" 中的 "入口点 2" 中。 "入口点 1" 的服务器 GPO 在域控制器 "DC1" 中进行管理，而 "入口点 2" 的服务器 GPO 在域控制器 "DC2" 中进行管理。 "DC1" 和 "DC2" 不可用。 第三个域控制器仍在域 "DC3" 中可用，并且 "DC1" 和 "DC2" 中的数据已复制到 "DC3"。  
  
![配置多站点基础结构](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc1.png)  
  
##### <a name="to-change-two-or-more-domain-controllers-that-manage-server-gpos"></a>更改两个或更多管理服务器 Gpo 的域控制器  
  
1.  若要将不可用的域控制器 "DC2" 替换为域控制器 "DC3"，请运行以下命令：  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    此命令更新 DA2 的注册表和 "入口点 2" 服务器 GPO 本身中的 "入口点 2" 服务器 GPO 的域控制器关联;但是，它不会更新 "入口点 1" 服务器 GPO，因为管理它的域控制器不可用。  
  
    > [!TIP]  
    > 此命令使用*ErrorAction*参数的 Continue 值，该参数将更新 "入口点 2" 服务器 gpo，尽管无法更新 "入口点 1" 服务器 gpo。  
  
    生成的配置如下图所示。  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc2.png)  
  
2.  若要将不可用的域控制器 "DC1" 替换为域控制器 "DC3"，请运行以下命令：  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC1' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    此命令更新 DA1 的注册表中的 "入口点 1" 服务器 GPO 的域控制器关联，并在 "入口点 1" 和 "入口点 2" 服务器 Gpo 中更新。 生成的配置如下图所示。  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc3.png)  
  
3.  若要为 "入口点 1" 服务器 GPO 中的 "入口点 2" 服务器 GPO 同步域控制器关联，请运行命令将 "DC2" 替换为 "DC3"，并指定其服务器 GPO 未同步的远程访问服务器，在本例中为 "DA1"，对于*ComputerName*参数。  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA1' "ErrorAction Continue  
    ```  
  
    最终配置如下图所示。  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssocFinal.png)  
  
### <a name="ConfigDistOptimization"></a>配置分发的优化  
进行配置更改时，仅在服务器 Gpo 传播到远程访问服务器后应用更改。 为了减少配置分发时间，远程访问会自动选择可写域控制器，这是在创建服务器 GPO 时最接近远程访问服务器的超链接 "<https://technet.microsoft.com/library/cc978016.aspx>"。  
  
在某些情况下，可能需要手动修改管理服务器 GPO 的域控制器，以便优化配置分发时间：  
  
-   作为入口点添加时，远程访问服务器的 Active Directory 站点中没有可写的域控制器。 现在，可写域控制器将添加到远程访问服务器的 Active Directory 站点。  
  
-   IP 地址更改或 Active Directory 站点和子网更改可能已将远程访问服务器移至另一个 Active Directory 站点。  
  
-   由于域控制器上的维护工作，入口点的域控制器关联被手动修改，现在域控制器恢复联机。  
  
在这些情况下，请在远程访问服务器上运行 PowerShell cmdlet `Set-DAEntryPointDC`，并使用参数*EntryPointName*指定要优化的入口点的名称。 只有在当前存储服务器 GPO 的域控制器上的 GPO 数据已完全复制到所需的新域控制器之后，才应执行此操作。  
  
> [!NOTE]  
> 在修改域控制器关联之前，请确保已将远程访问部署中的所有 Gpo 复制到域中的所有域控制器。 如果未同步 GPO，则在修改域控制器关联后可能会丢失最近的配置更改，这可能会导致配置损坏。 若要验证 GPO 同步，请参阅[检查组策略基础结构状态](https://technet.microsoft.com/library/jj134176.aspx)。  
  
若要优化配置分发时间，请执行下列操作之一：  
  
-   若要在最近 Active Directory 站点中的域控制器上管理入口点 "入口点 1" 的服务器 GPO，请运行以下命令：  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
-   若要管理域控制器 "dc2.corp.contoso.com" 上入口点 "入口点 1" 的服务器 GPO，请运行以下命令：  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "NewDC 'dc2.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
    > [!NOTE]  
    > 当修改与特定入口点相关联的域控制器时，必须指定作为*ComputerName*参数的入口点成员的远程访问服务器。  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   [步骤 3：配置多站点部署 @ no__t-0  
-   [步骤 1：实现单一服务器远程访问部署 @ no__t-0  

