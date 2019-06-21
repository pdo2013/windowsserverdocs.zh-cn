---
title: 步骤 2 配置多站点基础结构
description: 本主题是指南的一部分部署多台远程访问服务器在 Windows Server 2016 中的多站点部署中。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: faec70ac-88c0-4b0a-85c7-f0fe21e28257
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2d8d5fbe427f25e9e26eac96d89dc5fae17e197b
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281054"
---
# <a name="step-2-configure-the-multisite-infrastructure"></a>步骤 2 配置多站点基础结构

>适用于：Windows Server 2012 R2, Windows Server 2012

若要配置多站点部署中，有多个修改网络基础结构设置包括所需的步骤： 配置其他 Active Directory 站点和域控制器、 配置额外的安全组和配置组策略对象 (Gpo)，如果你不使用自动配置的 Gpo。  
  
|任务|描述|  
|----|--------|  
|2.1. 配置其他 Active Directory 站点|配置的部署的其他 Active Directory 站点。|  
|2.2. 配置其他域控制器|根据需要配置其他 Active Directory 域控制器。|  
|2.3. 配置安全组|配置任何 Windows 7 客户端计算机的安全组。|  
|2.4. 配置 GPO|根据需要配置其他组策略对象。|  
  
> [!NOTE]  
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="BKMK_ConfigAD"></a>2.1. 配置其他 Active Directory 站点  
所有入口点可以都驻留在单个 Active Directory 站点。 因此，至少一个 Active Directory 站点是多站点配置中的远程访问服务器的实现所必需的。 如果需要创建第一个 Active Directory 站点，或如果要使用的多站点部署的其他 Active Directory 站点，请使用此过程。 使用 Active Directory 站点和服务管理单元"s 网络你组织中创建新站点。  

中的成员身份**Enterprise Admins**林中的组或**Domain Admins**完成此过程所需的目录林根域或同等最少的组。 可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。  

有关详细信息，请参阅[到该林添加站点](https://technet.microsoft.com/library/cc732761.aspx)。  

### <a name="to-configure-additional-active-directory-sites"></a>若要配置的其他 Active Directory 站点  
  
1.  在主域控制器上，单击**启动**，然后单击**Active Directory 站点和服务**。  
  
2.  在 Active Directory 站点和服务控制台中，在控制台树中，右键单击**站点**，然后单击**新站点**。  
  
3.  上**新建对象-站点**对话框中**名称**框中，输入新的站点的名称。  
  
4.  在中**链接名称**，单击站点链接对象，并单击**确定**两次。  
  
5.  在控制台树中，展开**站点**，右键单击**子网**，然后单击**新子网**。  
  
6.  上**新建对象-子网**对话框中的**前缀**，在键入 IPv4 或 IPv6 子网前缀**选择站点对象为此前缀**列表中，单击要将关联的站点使用此子网，然后单击**确定**。  
  
7.  重复步骤 5 和 6，直到已创建你的部署中所需的所有子网。  
  
8.  关闭 Active Directory 站点和服务。  
  
![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
若要安装 Windows 功能"Windows PowerShell 的 Active Directory 模块":  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
或"Active Directory PowerShell 管理单元"OptionalFeatures 通过添加。  
  
如果在 Windows 7"或 Windows Server 2008 R2 上运行以下 cmdlet，然后 Active Directory PowerShell 模块必须导入：  
  
```  
Import-Module ActiveDirectory  
```  
  
若要配置的 Active Directory 站点名为"第二个站点"使用内置 DEFAULTIPSITELINK:  
  
```  
New-ADReplicationSite -Name "Second-Site"  
Set-ADReplicationSiteLink -Identity "DEFAULTIPSITELINK" -sitesIncluded @{Add="Second-Site"}  
```  
  
若要为第二个站点配置 IPv4 和 IPv6 子网：  
  
```  
New-ADReplicationSubnet -Name "10.2.0.0/24" -Site "Second-Site"  
New-ADReplicationSubnet -Name "2001:db8:2::/64" -Site "Second-Site"  
```  
  
## <a name="BKMK_AddDC"></a>2.2. 配置其他域控制器  
若要在单个域中配置多站点部署，建议您在部署中有每个站点至少一个可写域控制器。  
  
若要执行此过程，至少必须是在其中安装域控制器的域中 Domain Admins 组的成员。  
  
有关详细信息，请参阅[安装其他域控制器](https://technet.microsoft.com/library/cc733027.aspx)。
  
### <a name="to-configure-additional-domain-controllers"></a>若要配置的其他域控制器  
  
1.  将充当域控制器中的服务器上**服务器管理器**，然后在**仪表板**，单击**添加角色和功能**。  
  
2.  单击**下一步**三次以转到服务器角色选择屏幕  
  
3.  上**选择服务器角色**页上，选择**Active Directory 域服务**。 单击**添加功能**当系统提示，然后单击**下一步**三次。  
  
4.  在“确认”  页上，单击“安装”  。  
  
5.  安装成功完成后，单击**提升此服务器的域控制器到**。  
  
6.  在 Active Directory 域服务配置向导中，在**部署配置**页上，单击**的域控制器添加到现有域**。  
  
7.  在中**域**，输入域名称; 例如，corp.contoso.com。  
  
8.  下**提供凭据以执行此操作**，单击**更改**。 上**Windows 安全**对话框框中，可以安装其他域控制器的帐户提供的用户名和密码。 若要安装其他域控制器，你必须是 Enterprise Admins 组或 Domain Admins 组的成员。 提供凭据后，单击 **“下一步”** 。  
  
9. 上**域控制器选项**页上，执行以下操作：  
  
    1.  进行以下选择：  
  
        -   **域名系统 (DNS) 服务器**"选择此选项是默认情况下，以便你的域控制器可以作为域名系统 (DNS) 服务器。 如果你不希望该域控制器成为 DNS 服务器，则清除该选项。  
  
            如果林根级域中在主域控制器 (PDC) 模拟器上未安装 DNS 服务器角色，然后在其他域控制器上安装 DNS 服务器的选项不可用。 在此情况下解决此问题，可以在 AD DS 安装之前或之后安装 DNS 服务器角色。  
  
            > [!NOTE]  
            > 如果选择此选项以安装 DNS 服务器，可能会收到此消息，指明无法创建 DNS 服务器的 DNS 委派和应手动创建 DNS 委派到 DNS 服务器，以确保名称解析可靠。 如果在目录林根域或树的根域中安装其他域控制器，你无需创建 DNS 委派。 在这种情况下，单击**是**并忽略此消息。  
  
        -   **全局编录 (GC)** "默认情况下选择此选项。 它会将全局编录、只读目录分区添加到域控制器，并且将启用全局编录搜索功能。  
  
        -   **只读域控制器 (RODC)** "未默认情况下选择此选项。 它使其他域控制器为只读;也就是说，它使域控制器将 RODC。  
  
    2.  在中**站点名称**，从列表中选择一个站点。  
  
    3.  下**键入目录服务还原模式 (DSRM) 密码**，在**密码**并**确认密码**两次，键入强密码，然后单击**下一步**。 必须使用此密码必须脱机执行的任务，在 dsrm 下启动 AD DS。  
  
10. 上**DNS 选项**页上，选择**更新 DNS 委派**复选框，如果你想要更新 DNS 委派，在角色安装过程中，然后单击**下一步**。  
  
11. 上**其他选项**页上，键入或浏览到数据库文件、 目录服务日志文件和系统卷 (SYSVOL) 文件的卷和文件夹位置。 根据需要，指定复制选项，然后单击**下一步**。  
  
12. 上**查看选项**页上，查看安装选项，然后单击**下一步**。  
  
13. 上**必备项检查**页上之后验证系统必备组件后，单击**安装**。  
  
14. 等待，直到向导完成配置，然后依次**关闭**。  
  
15. 如果它未重新启动自动重新启动计算机。  
  
## <a name="BKMK_ConfigSG"></a>2.3. 配置安全组  
多站点部署为允许访问 Windows 7 客户端计算机的部署中每个入口点的 Windows 7 客户端计算机需要额外的安全组。 如果有多个域包含 Windows 7 客户端计算机，然后建议在同一个入口点的每个域中创建安全组。 或者，可以使用一个包含这两个域中的客户端计算机的通用安全组。 例如，在环境中使用两个域，如果你想要允许访问 Windows 7 客户端计算机中入口点 1 和 3，但不是在入口点 2，然后创建两个新的安全组以包含每个入口点中的每个 Windows 7 客户端计算机 域。  
  
### <a name="to-configure-additional-security-groups"></a>若要配置额外的安全组  
  
1.  在主域控制器上，单击**启动**，然后单击**Active Directory 用户和计算机**。  
  
2.  在控制台树中，右键单击想要添加新组，例如，corp.contoso.com/Users 的文件夹。 指向“新建”  ，然后单击“组”  。  
  
3.  上**新建对象-组**对话框中的**组名称**，键入新组，例如，Win7_Clients_Entrypoint1 的名称。  
  
4.  下**组作用域**，单击**通用**下**组类型**，单击**安全**，然后单击**确定**.  
  
5.  若要将计算机添加到新的安全组中，双击安全组，然后在 **< 组名 > 属性**对话框中，单击**成员**选项卡。  
  
6.  在“成员”  选项卡上，单击“添加”  。  
  
7.  选择要添加到此安全组，并单击的 Windows 7 计算机**确定**。  
  
8.  重复此过程以创建所需的每个入口点的安全组。  
  
![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
若要安装 Windows 功能"Windows PowerShell 的 Active Directory 模块":  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
或"Active Directory PowerShell 管理单元"OptionalFeatures 通过添加。  
  
如果在 Windows 7"或 Windows Server 2008 R2 上运行以下 cmdlet，然后 Active Directory PowerShell 模块必须导入：  
  
```  
Import-Module ActiveDirectory  
```  
  
若要配置名为 Win7_Clients_Entrypoint1 的安全组并添加名为客户端 2 的客户端计算机：  
  
```  
New-ADGroup -GroupScope universal -Name Win7_Clients_Entrypoint1  
Add-ADGroupMember -Identity Win7_Clients_Entrypoint1 -Members CLIENT2$  
```  
  
## <a name="ConfigGPOs"></a>2.4. 配置 GPO  
多站点远程访问部署需要以下组策略对象：  
  
-   用于远程访问服务器每个入口点的 GPO。  
  
-   用于为每个域的任何 Windows 8 客户端计算机的 GPO。  
  
-   中包含的每个入口点的 Windows 7 客户端计算机的每个域的 GPO 配置为支持 Windows 7 客户端。  
  
    > [!NOTE]  
    > 如果还没有任何 Windows 7 客户端计算机，您不需要创建 Gpo 适用于 Windows 7 的计算机。  
  
如果它们不"t 已存在在配置远程访问时，向导会自动创建所需组策略对象。 如果您没有所需的权限来创建组策略对象，则必须配置远程访问之前创建它。 DirectAccess 管理员必须对 gpo 具有完全权限 （编辑 + 修改安全性 + 删除）。  
  
> [!IMPORTANT]  
> 手动创建 Gpo，为远程访问后必须为 Active Directory 和 DFS 复制到与远程访问服务器相关联的 Active Directory 站点中的域控制器允许足够的时间。 如果远程访问自动创建组策略对象，则需要没有等待时间。  
  
若要创建组策略对象，请参阅[创建和编辑组策略对象](https://technet.microsoft.com/library/cc754740.aspx)。  
  
### <a name="DCMaintandDowntime"></a>域控制器维护和停机时间  
当与 PDC 仿真器运行的域控制器或管理服务器 Gpo 的域控制器将经历停机时间时，不能加载或修改远程访问配置。 如果其他域控制器都不可用，这不影响客户端连接。  
  
若要加载或修改远程访问配置，可以将 PDC 仿真器角色传输到另一个域控制器为客户端或应用程序服务器 Gpo;对于服务器 Gpo，更改管理服务器 Gpo 的域控制器。  
  
> [!IMPORTANT]  
> 只能由域管理员可以执行此操作。 更改主域控制器的影响并不局限于远程访问;因此，将 PDC 模拟器角色传输时要格外小心。  
  
> [!NOTE]  
> 在修改之前域控制器关联，请确保的所有远程访问部署中的 Gpo 已被复制到所有域中的域控制器。 如果不同步该 GPO，修改域控制器关联，这可能会导致损坏的配置后可能丢失最近的配置更改。 若要验证 GPO 同步，请参阅[检查组策略基础结构状态](https://technet.microsoft.com/library/jj134176.aspx)。  
  
#### <a name="TransferPDC"></a>若要将 PDC 仿真器角色传输  
  
1.  上**启动**屏幕上，键入**dsa.msc**，然后按 ENTER。  
  
2.  在 Active Directory 用户和计算机控制台的左窗格中，右键单击**Active Directory 用户和计算机**，然后单击**更改域控制器**。 在更改目录服务器对话框中，单击**此域控制器或 AD LDS 实例**，请在列表中单击域控制器，为新角色拥有者，然后单击**确定**。  
  
    > [!NOTE]  
    > 如果您不想要将此角色转移到的域控制器上，必须执行此步骤。 如果已连接到你想要将此角色转移的域控制器，则不执行此步骤。  
  
3.  在控制台树中，右键单击**Active Directory 用户和计算机**，依次指向**的所有任务**，然后单击**操作主机**。  
  
4.  在操作主机对话框中，单击**PDC**选项卡，然后依次**更改**。  
  
5.  单击**是**以确认你想要将此角色转移，然后单击**关闭**。  
  
#### <a name="ChangeDC"></a>若要更改域控制器的管理服务器 Gpo  
  
-   运行 Windows PowerShell cmdlet`HYPERLINK "https://technet.microsoft.com/library/hh918412.aspx" Set-DAEntryPointDC`远程访问服务器上，并指定为无法访问的域控制器名称*ExistingDC*参数。 此命令将修改为服务器 Gpo 的域控制器关联的当前管理的域控制器的入口点。  
  
    -   若要替换的域控制器"dc2.corp.contoso.com"无法访问的域控制器"dc1.corp.contoso.com"，执行以下操作：  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "NewDC 'dc2.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
    -   若要替换为远程访问服务器"DA1.corp.contoso.com"最近的 Active Directory 站点中的域控制器无法访问的域控制器"dc1.corp.contoso.com"，执行以下操作：  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
### <a name="ChangeTwoDCs"></a>更改两个或多个管理服务器 Gpo 的域控制器  
在最少的情况下，两个或多个管理服务器 Gpo 的域控制器都不可用。 如果发生这种情况，更多的步骤所需更改服务器 Gpo 的域控制器关联。  
  
域控制器关联信息存储在远程访问服务器的注册表和所有服务器 Gpo 中。 在以下示例中，有两个入口点有两个远程访问服务器，"DA1"中"入口点 1" 和"DA2"中"入口点 2"。 服务器 GPO 的"入口点 1"时的服务器 GPO 的域控制器"DC1"，以管理"中的域控制器"DC2"托管入口点 2"。 "DC1"和"DC2"不可用。 第三个域控制器是仍可在域中，"DC3"和"DC1"和"DC2"中的数据已复制到"DC3"。  
  
![配置多站点基础结构](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc1.png)  
  
##### <a name="to-change-two-or-more-domain-controllers-that-manage-server-gpos"></a>若要更改两个或多个管理服务器 Gpo 的域控制器  
  
1.  若要使用的域控制器"DC3"替换不可用的域控制器"DC2"，运行以下命令：  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    此命令将更新的域控制器关联的"入口点 2"服务器注册表中 GPO DA2 的和中的"入口点 2"的服务器 GPO，然后重试。但是，它不会不更新"入口点 1"的服务器 GPO，因为管理它的域控制器不可用。  
  
    > [!TIP]  
    > 此命令使用的继续值*ErrorAction*参数，尽管未能更新"入口点 1"的服务器 GPO 将"入口点 2"服务器 GPO 更新。  
  
    生成的配置是在下图中所示。  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc2.png)  
  
2.  若要使用的域控制器"DC3"替换不可用的域控制器"DC1"，运行以下命令：  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC1' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    此命令将更新为"入口点 1"的服务器 GPO 中，"入口点 1"和"入口点 2"内的 DA1 注册表服务器 Gpo 的域控制器关联。 生成的配置是在下图中所示。  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc3.png)  
  
3.  若要同步的"入口点 2"服务器中的"入口点 1"的服务器 GPO，运行命令以替换"DC2"与"DC3"，并指定其服务器 GPO 未同步，在这种情况下的远程访问服务器 GPO 的域控制器关联的"DA1"*ComputerName*参数。  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA1' "ErrorAction Continue  
    ```  
  
    最终配置如下图所示。  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssocFinal.png)  
  
### <a name="ConfigDistOptimization"></a>配置分发的优化  
进行配置更改时，只有在服务器 Gpo 传播到远程访问服务器后，才应用所做的更改。 若要减少配置分发时，远程访问，会自动选择为超链接的可写域控制器"<https://technet.microsoft.com/library/cc978016.aspx>"远程访问服务器创建其服务器 GPO 时最接近。  
  
在某些情况下，它可能需要手动修改才能优化配置分发时间管理服务器 GPO 的域控制器：  
  
-   没有任何可写域控制器在 Active Directory 站点中的远程访问服务器时将其添加作为入口点。 正在将可写域控制器添加到远程访问服务器的 Active Directory 站点中。  
  
-   IP 地址更改或 Active Directory 站点和子网更改可能会远程访问服务器移动到另一个 Active Directory 站点。  
  
-   入口点的域控制器关联已被手动修改由于域控制器上的维护工作和现在的域控制器已重新联机。  
  
在这些情况下，运行 PowerShell cmdlet`Set-DAEntryPointDC`远程访问服务器上，并指定你想要充分利用了该参数的入口点的名称*EntryPointName*。 你应执行此操作的 GPO 数据后才从当前存储的服务器 GPO 已完全复制到所需的新域控制器的域控制器。  
  
> [!NOTE]  
> 在修改之前域控制器关联，请确保的所有远程访问部署中的 Gpo 已被复制到所有域中的域控制器。 如果不同步该 GPO，修改域控制器关联，这可能会导致损坏的配置后可能丢失最近的配置更改。 若要验证 GPO 同步，请参阅[检查组策略基础结构状态](https://technet.microsoft.com/library/jj134176.aspx)。  
  
若要优化配置分发时，请执行以下操作：  
  
-   若要管理服务器 GPO 的条目指向"入口点 1"最近的 Active Directory 站点中的域控制器上远程访问服务器"DA1.corp.contoso.com"，运行以下命令：  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
-   管理服务器 GPO 的条目指向"入口点 1"上的域控制器"dc2.corp.contoso.com"，运行以下命令：  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "NewDC 'dc2.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
    > [!NOTE]  
    > 在修改与特定入口点相关的域控制器，必须指定属于该入口点的远程访问服务器*ComputerName*参数。  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   [步骤 3：配置多站点部署](Step-3-Configure-the-Multisite-Deployment.md)  
-   [步骤 1：实现单个服务器，远程访问部署](Step-1-Implement-a-Single-Server-Remote-Access-Deployment.md)  

