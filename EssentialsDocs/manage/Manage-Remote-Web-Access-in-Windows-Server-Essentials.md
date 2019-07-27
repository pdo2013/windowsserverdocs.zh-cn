---
title: 管理 Windows Server Essentials 中的远程 Web 访问
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f3ea40fa-b6ba-4d66-b754-221ca6271387
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 3eace9281d9fcdea5262274ac7fb20ec30d30fb4
ms.sourcegitcommit: 9f955be34c641b58ae8b3000768caa46ad535d43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/27/2019
ms.locfileid: "68590572"
---
# <a name="manage-remote-web-access-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的远程 Web 访问

>适用于：Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Windows Server Essentials 中的远程 Web 访问, 或在安装了 Windows Server Essentials 体验角色的 Windows Server 2012 R2 中, 为从几乎任何位置访问应用程序和数据提供了一种简单、触摸友好的浏览器体验你使用的是 Internet 连接, 并且使用了几乎任何设备。 要使用远程 Web 访问功能，你必须首先使用“设置随处访问”向导启用该功能，而后再设置路由器和域名。  
  
## <a name="in-this-topic"></a>本主题内容  
  
-   [启用和配置远程 Web 访问](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [设置路由器](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [设置域名](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [自定义远程 Web 访问](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [远程 Web 访问疑难解答](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_5)  
  
##  <a name="BKMK_1"></a>启用和配置远程 Web 访问  
 以下主题将帮助你启用和配置远程 Web 访问：  
  
-   [远程 Web 访问概述](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [打开远程 Web 访问](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA)  
  
-   [更改你所在的区域](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Region)  
  
-   [管理远程 Web 访问权限](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManagePerms)  
  
-   [安全远程 Web 访问](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SecureRWA)  
  
-   [管理远程 Web 访问和 VPN 用户](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManageRWAVPN)  
  
###  <a name="BKMK_Overview"></a>远程 Web 访问概述  
 当你离开办公室时, 你可以打开 web 浏览器, 并从可访问 Internet 的任何位置访问远程 Web 访问。 在远程 Web 访问中, 可以:  
  
- 访问服务器上的共享文件和文件夹。  
  
- 访问网络上的服务器和计算机。 这意味着，你可以访问联网计算机的桌面，就像你正在办公室坐那台计算机前面操作一样。  
  
  默认情况下, 远程 Web 访问未打开。 当运行“设置随处访问”向导时，该向导将尝试设置路由器和 Internet 连接。 启用远程 Web 访问后, 你可以为服务器设置域名, 并自定义远程 Web 访问。 若更改路由器，你还可以设置该路由器。  
  
  当你添加新的用户帐户时, 不会自动授予访问远程 Web 访问的权限。 在添加用户帐户时，你可以选择允许访问共享文件夹、媒体库、计算机、主页链接以及服务器仪表板。 你还可以指定不允许用户使用远程 Web 访问。  
  
  将在 Windows Server Essentials 仪表板的 "**用户**" 选项卡上为每个用户帐户显示 "远程 Web 访问" 设置。 若要更改远程 Web 访问设置, 请右键单击该用户帐户, 然后单击 "**查看帐户属性"** 。  
  
###  <a name="BKMK_TurnOnRWA"></a>打开远程 Web 访问  
 你可以通过从服务器仪表板运行“设置随处访问”向导的方式来启用远程 Web 访问。  
  
##### <a name="to-turn-on-remote-web-access"></a>启用远程 Web 访问的步骤  
  
1.  打开“仪表板”。  
  
2.  单击“设置”，然后单击“随处访问”选项卡。  
  
3.  单击 **“配置”** 。 此时会出现“设置随处访问”向导。  
  
4.  在“选择要启用的随处访问功能”  页上，选中“远程 Web 访问”  复选框。  
  
5.  按照说明完成向导。  
  
###  <a name="BKMK_Region"></a>更改你所在的区域  
 只有以网络管理员身份才能更改 Windows Server Essentials 中的地区设置。  
  
##### <a name="to-change-the-region-setting"></a>更改地区设置  
  
1.  在连接到 Windows Server Essentials 的计算机上打开“仪表板”。  
  
2.  单击“设置”。  
  
3.  在“常规”  选项卡上，单击“服务器的国家/地区位置”  部分中的下拉列表。  
  
4.  从该下拉列表中选择新地区，然后单击“应用”  接受新的地区设置。  
  
###  <a name="BKMK_ManagePerms"></a>管理远程 Web 访问权限  
 当你在 Windows Server Essentials 中添加用户帐户时，默认情况下将允许新用户使用远程 Web 访问。 如果选择不允许用户帐户进行远程 Web 访问, 然后发现用户需要使用远程 Web 访问, 则可以更新用户帐户的属性。  
  
##### <a name="to-manage-remote-web-access-permissions-for-a-user-account"></a>管理用户帐户的远程 Web 访问权限  
  
1. 登录到仪表板，然后单击“用户” 。  
  
2. 单击要管理的用户帐户，然后在“任务”窗格中单击“查看帐户属性”。  
  
3. 在“属性”  对话框中，单击“随处访问”  选项卡。  
  
4. 在“随处访问”  选项卡上，选中“允许远程 Web 访问和对 Web 服务应用程序的访问”  复选框，以允许用户使用远程 Web 访问连接到服务器。  
  
5. 单击“应用”，然后单击“确定”。  
  
   有关详细信息, 请参阅[管理用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md)。  
  
###  <a name="BKMK_SecureRWA"></a>安全远程 Web 访问  
 Windows Server Essentials 使用安全证书以帮助保护在软件和 Web 浏览器之间交换的信息的安全。 在计算机上安装连接器软件后，适用于 Windows Server Essentials 的安全证书将添加到计算机上受信任的证书列表中。 当用户离开办公室时，他们访问远程 Web 访问的最佳方式是使用装有连接器软件的便携式计算机。  
  
> [!WARNING]
>  从公共位置或其他不受信任的计算机使用远程 Web 访问的用户应确保在计算机处于无人使用的状态前或完成其会话后注销网站。  
  
###  <a name="BKMK_ManageRWAVPN"></a>管理远程 Web 访问和 VPN 用户  
 你可以使用 VPN 连接到 Windows Server Essentials 并访问存储在服务器上的所有资源。 如果你有一台通过网络帐户设置的客户端计算机，并且该网络帐户可通过 VPN 连接来连接到托管的 Windows Server Essentials 服务器，则上述方法尤其有用。 在托管 Windows Server Essentials 服务器上新创建的所有用户帐户均必须在首次使用时通过 VPN 登录到客户端计算机。  
  
##### <a name="to-set-vpn-and-remote-web-access-permissions-for-network-users"></a>为网络用户设置 VPN 和远程 Web 访问权限  
  
1.  打开“仪表板”。  
  
2.  在导航栏上，单击“用户” 。  
  
3.  在用户帐户列表中，选择要授予其远程访问桌面的权限的用户帐户。  
  
4.  在 **< 用户帐户\>任务**"窗格中, 单击"**属性**"。  
  
5.  在 **< 用户帐户\>属性**"中, 单击"**随处访问**"选项卡。  
  
6.  在“随处访问”选项卡上，执行以下操作：  
  
    1.  若要允许用户通过 VPN 连接到服务器，请选中“允许虚拟专用网络 (VPN)”复选框。  
  
    2.  若要允许用户通过远程 Web 访问连接到服务器，请选中“允许远程 Web 访问和对 Web 服务应用程序的访问”复选框。  
  
7.  单击“应用”，然后单击“确定”。  
  
##  <a name="BKMK_2"></a>设置路由器  
 当配置远程 Web 访问服务器时，“设置随处访问”向导会尝试设置路由器。 如果更改路由器或更改路由器设置，你必须返回设置路由器向导。 有关详细信息，请参阅下列主题：  
  
-   [设置路由器](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpRouter)  
  
-   [更换路由器](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ReplaceRouter)  
  
-   [定义的网络位置](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_NetworkLocation)  
  
-   [启用远程桌面服务 ActiveX 控件](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ActiveX)  
  
###  <a name="BKMK_SetUpRouter"></a>设置路由器  
 在此步骤中，Windows Server Essentials 会尝试使用 UPnP 命令自动配置路由器。 为此，你的路由器必须支持 UPnP 标准，而且也要启用 UPnP 设置。  
  
> [!NOTE]
>  你的网络配置应当满足 Windows Server Essentials 所需的网络要求。 网络中只应有一个路由器。  
  
 如果未通过“设置域名”向导设置路由器，则必须手动设置转发端口 443。 有关如何在路由器上设置端口转发的信息，请参阅 [路由器设置](https://social.technet.microsoft.com/wiki/contents/articles/windows-small-business-server-2011-essentials-router-setup.aspx)。  
  
###  <a name="BKMK_ReplaceRouter"></a>更换路由器  
 根据制造商的说明更换路由器, 然后运行 "设置路由器" 向导来配置新路由器。  
  
##### <a name="to-set-up-your-new-router"></a>设置新路由器  
  
1.  在 Windows Server Essentials 仪表板上单击“设置” 。  
  
2.  单击“随处访问”  选项卡，然后在“路由器”  部分中单击“设置” 。 “设置路由器”向导将会启动。  
  
3.  按照该向导中的说明完成新路由器的设置。  
  
###  <a name="BKMK_NetworkLocation"></a>定义的网络位置  
 网络位置是在连接到网络时 Windows 应用的网络设置的集合。 这些设置各不相同，并且可以根据使用的网络类型进行自定义。 网络位置的设置将确定某些功能（如文件和打印机共享、网络发现以及公用文件夹共享）是处于打开还是关闭的状态。 当需要连接到其他网络时，网络位置十分有用。  
  
 例如，你可能拥有一台便携式计算机，可以在家中和工作中使用。 当你在办公室时，你可以连接到办公室网络。 而当你回到家时，你可以使用便携式计算机访问并播放存储在家庭服务器上的视频和音乐。 在你连接到新网络并指定位置类型后，Windows 将分配为该位置类型预设的网络配置文件。 下次连接到该网络时，Windows 将识别该网络并自动分配正确的设置。 这增加了一层安全性，可帮助保护计算机中的信息，并且只有该位置所需的网络功能才处于启用状态。  
  
 有四种类型的网络位置：  
  
-   **家庭网络** 对于家庭网络或在你认识并信任网络上的个人和设备时，请选择此网络。 家庭网络中的计算机可以属于某个家庭组。 对于家庭网络，网络发现处于启用状态，它允许你查看网络上的其他计算机和设备并允许其他网络用户查看你的计算机。  
  
-   **工作网络** 对于小型办公室网络或其他工作区网络，请选择此网络。 默认情况下，网络发现处于启用状态，该功能允许你查看网络上的其他计算机和设备并允许其他网络用户查看你的计算机；但你无法创建或加入家庭组。  
  
-   **公用网络** 对于公共场所（如咖啡店或机场），请选择此网络。 此位置旨在使其他计算机不能查看你的计算机，并帮助保护你的计算机免受来自 Internet 的恶意软件的攻击。 家庭组在公用网络中不可用，并且网络发现也处于禁用状态。 如果你在没有使用路由器的情况下直接连接到 Internet，或者具有移动宽带连接，也应该选择此选项。  
  
-   **域** 请针对域（例如，企业工作区的域）选择此网络。 这种类型的网络位置由网络管理员控制，它无法进行选择或更改。  
  
###  <a name="BKMK_ActiveX"></a>启用远程桌面服务 ActiveX 控件  
 远程桌面服务 ActiveX 控件允许您通过 Internet 从其他计算机使用远程 Web 访问访问家庭或企业计算机。  
  
##### <a name="to-enable-remote-desktop-services-activex-controls"></a>启用远程桌面服务 ActiveX 控件  
  
1.  在 Internet Explorer 中，单击“工具” ，然后单击“Internet 选项” 。  
  
2.  在“安全”选项卡上，单击“自定义级别”。  
  
3.  在“ActiveX 控件和插件”  部分中，执行下列操作：  
  
    1.  在“下载已签名的 ActiveX 控件” 下，单击“提示” 。  
  
    2.  在“运行 ActiveX 控件和插件”下，单击“启用”。  
  
4.  单击“确定”两次以接受这些更改并关闭对话框。  
  
##  <a name="BKMK_3"></a>设置域名  
 在启用远程 Web 访问后，你可以为运行 Windows Server Essentials 的服务器设置域名。 如果计划从远程计算机使用远程 Web 访问功能，这将是一项必要步骤。 有关详细信息，请参阅下列主题：  
  
-   [域名概述](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_DNOverview)  
  
-   [了解 Microsoft 个性化域名](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_PersonalizedNames)  
  
-   [使用新的或现有的域名](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UseNewName)  
  
-   [设置域名](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpName)  
  
-   [选择域名服务提供商](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseProvider)  
  
-   [选择域名](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseDomainName)  
  
-   [选择域名前缀](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Prefixes)  
  
-   [选择域名扩展](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Extension)  
  
-   [更新或升级你的域名服务](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UpdateService)  
  
-   [在服务器上导出或导入证书](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ExportCert)  
  
-   [手动设置域名](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetNameManually)  
  
-   [查找域名服务提供商](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Find)  
  
###  <a name="BKMK_DNOverview"></a>域名概述  
 域名可在 Internet 上唯一地标识你的服务器。 域名至少包含以下两个部分：一级域名 (TLD) 和二级域名。 例如, 在 contoso.com 中, com 为 TLD, contoso 为二级域名。  
  
 当你离开办公室时，你可以使用你的域名来访问网络上服务器和计算机上的共享文件。 当你离开时，你还可以管理自己的服务器。 例如，为你的服务器注册 contoso.com。 当你离开办公室时，你可以在你的便携式计算机上打开 Web 浏览器，然后在地址文本框中键入 **contoso.com**，以连接到你在 Windows Server Essentials 上设置的远程 Web 访问的实例。  
  
###  <a name="BKMK_PersonalizedNames"></a>了解 Microsoft 个性化域名  
 Microsoft 个性化域名包括以下功能：  
  
- 远程 Web 访问的自定义域名 (*例如, remotewebaccess.com*)。 你的域名与公用 IP 地址相关联。  
  
- DNS 动态更新协议服务, 以便在公共 IP 地址更改时不会中断使用域名的远程 Web 访问。 通常, 用于组织的宽带连接的 Internet 服务提供商 (Isp) 提供可更改的动态公共 IP 地址。  
  
- 与域名相关联的受信任证书。  
  
  若要将 Microsoft 个性化域名与你的服务器集成，你需要一个 Microsoft 帐户（以前称为 Windows Live ID）。 如果没有 Microsoft 帐户，可以在 [Microsoft Hotmail](https://login.live.com/) 网站上注册一个帐户。  
  
> [!IMPORTANT]
>  Windows Live 允许在 Microsoft 帐户密码中使用服务器不支持的特殊字符。 若使用 Microsoft 个性化域，请确保 Microsoft 帐户密码仅包含服务器支持的字符。 服务器不支持使用字符 $、/、' 和 %。  
  
###  <a name="BKMK_UseNewName"></a>使用新的或现有的域名  
 若要在运行 Windows Server Essentials 的服务器上自动设置域名，则必须使用“设置域名”向导中列出的域名服务提供商。 你可以选择获取新域名或者使用现有域名。 执行下列操作之一：  
  
-   若要从该向导中列出的域名服务提供商处获取新域名，请单击“我想要设置新域名” 。  
  
-   如果你具有从某个受支持的域名服务提供商处购买的现有域名，则可以使用“设置域名”向导为服务器设置域名。 单击“我想要使用我已经拥有的域名”，然后在“设置你的域名”文本框中键入域名。 必须提供用于购买域名的用户名和密码。  
  
-   如果具有一个从 Windows Server Essentials 不支持的域名服务提供商处购买的现有域名，并且希望使用“设置域名”向导为服务器设置域名，则可以将该域名传输到该向导中列出的某个域名服务提供商。 单击 "**我想要使用我已经拥有的域名**", 在 "**域名**" 文本框中键入域名, 然后按照域名服务提供商的网站上的说明传输域名。  
  
###  <a name="BKMK_SetUpName"></a>设置域名  
 当启用远程 Web 访问时，你可以选择设置服务器的 Internet 域名。  
  
##### <a name="to-set-up-or-manage-an-internet-domain-name"></a>设置或管理 Internet 域名  
  
1.  打开“仪表板”。  
  
2.  单击“服务器设置”，然后单击“随处访问”选项卡。  
  
3.  在“域名”部分中，单击“设置”。  
  
4.  按照说明完成向导。 如果你尚未拥有域名和证书，则向导可帮助你找到域名提供商以购买域名和证书，或者你可以获取 Microsoft 个性化域名。  
  
###  <a name="BKMK_ChooseProvider"></a>选择域名服务提供商  
 你应该选择一个域名服务提供商，它支持要使用的域名扩展名。 "设置域名" 向导包括可以与每个提供商网站的链接一起使用的合格提供商列表。 单击每个提供商的名称旁的 "**详细信息**" 链接, 以获取有关提供商提供的服务和价格的信息。  
  
> [!NOTE]
>  一些域名服务提供商为广泛的国际市场提供服务，而其他域名服务提供商为较小的市场服务。 因此，某些提供商可能不会提供翻译为你偏好的语言的网站。  
  
 当购买域名时，你可能还需要考虑从域名服务提供商处购买域名系统 (DNS) 动态更新协议服务。 DNS 动态更新协议是一项服务，它使 Internet 上的任何人都可以在本地网络的 IP 地址不断更改时获得该网络上资源的访问权限。 或者，你可以从 Internet 服务提供商 (ISP) 那里购买静态 IP 地址，以确保你的 IP 地址不会更改。  
  
###  <a name="BKMK_ChooseDomainName"></a>选择域名  
 选择可唯一标识你公司服务器的名称。 例如，如果你的公司名称为 Contoso Ltd，则你可以选择 Contoso 来唯一标识 Internet 上的家庭服务器或公司服务器。 如果该域名不可用，则可以尝试该名称的另一种变体或完全不同的名称。  
  
 键入的名称可以包含以下内容：  
  
-   最多 63 个字符  
  
-   字母（英语或本地化的字符）数字或者连字符 (-)。 名称必须以字母或数字开始和结束。  
  
    > [!NOTE]
    >  域名不区分大小写。  
  
###  <a name="BKMK_Prefixes"></a>选择域名前缀  
 域名由分层标签组成。  
  
 **一级域名扩展名**为域名中最右边的标签。 例如, 在 www\.contoso.com 中, com 为顶级域名扩展。  
  
 **二级域名**是一级域名扩展名旁的标签。 通常根据公司名称、产品或服务来创建二级域名。 例如, 在 www\.contoso.com 中, contoso 是第二级域名, 并且是为公司名称 contoso 药品选择的。 二级域名有时称为主机名，它具有相关联的 IP 地址。  
  
 **域名前缀**可标识子域。 子域名可用于标识服务、设备或地区。 例如，Contoso Pharmaceuticals 希望远程用户可以登录到远程 Web 访问，但不希望向公众提供该网站，因此他们创建子域，该子域仅允许具有相应权限的用户访问该网站。 Contoso Pharmaceuticals 将 remote.contoso.com 设置为子域，并且“remote”是该域名的前缀。  
  
> [!TIP]
>  建议将默认的“Remote”用作你的域名前缀。  
  
###  <a name="BKMK_Extension"></a>选择域名扩展  
 在为你的 Internet 网站选择域名时，你还需要指定要使用的域名扩展名。 该扩展名由任意域名的最后一个点号后跟随的字母来进行标识。 (扩展的正式术语为顶级域或 TLD。)  
  
 可使用的域扩展名有两种主要类型：通用域扩展名和国家/地区代码。  
  
#### <a name="generic-top-level-domains"></a>通用一级域名  
 通用域扩展名的长度至少为三个字母，并且它们通常由某些类型的组织使用。  
  
 **一般顶级域的示例**  
  
|域扩展名|描述|  
|----------------------|-----------------|  
|.com|通常由商业组织使用，但它可供任何人使用。|  
|.net|为提供网络基础结构服务的企业设计。|  
|.org|最初由非赢利机构和不属于另一种通用一级域名类别的其他企业使用。 可供任何人使用。|  
|.edu|仅限教育机构使用。|  
  
#### <a name="country-code-top-level-domains"></a>国家/地区代码一级域名  
 这些域扩展名长度为两个字母。 它们旨在供与该代码相关联的国家或地区中的组织使用。 某些国家/地区代码一级域名仅限该国家或地区的公民使用。 而另一些国家/地区代码一级域名可供任何人使用。  
  
 **国家/地区代码的顶级域的示例**  
  
|域扩展名|描述|  
|----------------------|-----------------|  
|.ca|供加拿大的网站使用|  
|.cn|供中国的网站使用|  
|.de|供德国的网站使用|  
|.co.uk|供英国的网站使用|  
  
 若要查看一级域名的完整列表，请参阅 [Internet 编号分配管理机构网站](https://go.microsoft.com/fwlink/?LinkId=117438)。  
  
#### <a name="if-a-domain-extension-is-not-available-to-select-in-the-set-up-domain-name-wizard"></a>若某个域扩展名在“设置域名”向导中不可选择  
 当你运行“设置域名”向导时，该向导将查看你的系统信息以确定你所在的国家或地区。 然后，该向导仅显示你所在区域的参与提供商支持的域扩展名。 如果你希望使用的域扩展名未在列表中显示，你必须选择另一个域扩展名以继续操作。 从该向导返回的列表中选择域扩展名。  
  
###  <a name="BKMK_UpdateService"></a>更新或升级你的域名服务  
 如果你已购买域名，但未购买证书，则需要更新或升级你的域名服务。 你必须从你的域名服务提供商处获取用于域名的证书。  
  
> [!NOTE]
>  与你的域名服务提供商协作以确定所需的证书类型。 证书可以是提供的价格较低的证书之一。 但是，你应查看较高级别安全证书的文档和功能，以确定它们是否能更好地满足你的业务需求。  
  
###  <a name="BKMK_ExportCert"></a>在服务器上导出或导入证书  
 若要创建某个证书的备份副本或在其他服务器上使用它，则必须导出该证书。 有关导出证书的信息，请参阅 [导出证书](https://go.microsoft.com/fwlink/p/?LinkId=214362)。  
  
###  <a name="BKMK_SetNameManually"></a>手动设置域名  
 如果选择此选项，那么服务器不会监视或维护你的域名，也不会在出现配置问题时向你发出警报。 如果满足以下任一情况，也可以考虑使用此选项：  
  
- 你所在国家或地区的合作伙伴域名提供商未列出。  
  
- 列出的合作伙伴域名提供商不支持你的域名扩展名。  
  
- 你现有的域名的域名提供商目前不是合作伙伴，而且你不希望将该域名传输到支持 Windows Server Essentials 的域名提供商。  
  
- 此向导未列出你要使用的域名扩展名，但是该扩展名的域名提供商目前并非合作伙伴。  
  
  如果你选择手动设置域名, 请与你的域名服务提供商协作, 为你的域创建 A 记录。  
  
##### <a name="to-create-an-a-record"></a>创建 A 记录  
  
1.  确定主机名, 如远程。 此为域名前缀。 域名前缀加上域名可定义用于打开远程 Web 访问登录页面的 URL;例如, **http://remote.contoso.com** 。  
  
2.  在域名服务提供商配置仪表板中 (通常在其网页上), 为你在步骤1中确定的主机名创建 A 记录。 确保在 A 记录中指定的 IP 地址是路由器 WAN 端 (面向 Internet) 的 IP 地址。 请参阅你的路由器文档，查找关于 WAN IP 地址的相关信息。  
  
3.  建议联系你的 Internet 服务提供商 (ISP) 为你的网络购买一个静态 IP 地址。 这能确保 IP 地址不会发生变化，而且你的 DNS 项也不会过期。  
  
     如果你无法从 ISP 获取静态 IP 地址，也可以考虑从域名服务提供商或其他服务提供商处购买域名系统 (DNS) 动态更新协议服务。 DNS 动态更新协议是一项能够将网络的 WAN IP 地址保持为最新的服务，这样即使 IP 地址发生变化，也能够解析为你的域名。  
  
4.  收到向导提醒时，请导入一个受信任的证书。 如果没有受信任的证书，可以从向导中列出的某个受支持的域名提供商处获取一个，也可以选择一个受信任的提供商进行购买。 有关受信任证书的详细信息，请联系你的域名提供商。  
  
###  <a name="BKMK_Find"></a>查找域名服务提供商  
  
##### <a name="to-find-the-domain-name-service-provider-for-your-domain-name"></a>查找你的域名的域名服务提供商  
  
1. 打开 Web 浏览器，然后在地址栏键入 <strong>www.internic.com</strong> 以转至 Internic 主页。  
  
2. 在 Internic 主页上，单击“Whois”。  
  
3. 在“Whois”框中，键入你的域名（例如 contoso.com）。  
  
4. 单击“域”  选项，然后单击“提交” 。  
  
5. 在搜索结果中，域名称服务提供商的名称已在“注册机构”下列出。  
  
##  <a name="BKMK_4"></a>自定义远程 Web 访问  
 你可以通过添加个人徽标或背景图像自定义远程 Web 访问站点。 还可以在主页上添加链接，以便向你的所有用户提供此信息。 有关详细信息，请参阅下列主题：  
  
-   [自定义远程 Web 访问](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeRWA)  
  
-   [为背景和徽标自定义图像](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeImages)  
  
-   [修复远程 Web 访问](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_RepairRWA)  
  
###  <a name="BKMK_CustomizeRWA"></a>自定义远程 Web 访问  
 通过更改网站的标题、更改背景图像和徽标以及添加指向主页上的其他网站的链接，你可以自定义远程 Web 访问。  
  
##### <a name="to-customize-remote-web-access"></a>自定义远程 Web 访问  
  
1.  打开“仪表板”。  
  
2.  单击“设置”，然后单击“随处访问”选项卡。  
  
3.  在“网站设置”部分中，单击“自定义”。  
  
4.  完成自定义远程 Web 访问后，单击“确定” 。 测试远程 Web 访问上的更改。  
  
###  <a name="BKMK_CustomizeImages"></a>为背景和徽标自定义图像  
 本部分提供用于自定义远程 Web 访问的图像的相关信息。  
  
#### <a name="image-size"></a>映像大小  
 **徽标图像**  
  
 建议你使用 32x32 像素的徽标图像。 较大的图像将缩小到 32x32，而较小的图像将拉伸到 32x32，这可能会使图像失真。  
  
 **背景图像**  
  
 尽管背景图像没有大小限制，但为了达到最佳效果，建议使用大约 800x500 像素的图像。 背景图像应置于登录页的中心位置（水平和垂直）。 若要帮助登录页上的文本更易于阅读，则背景图像的中心位置应采用浅色。  
  
#### <a name="image-file-types"></a>图像文件类型  
 下列图像文件类型可用于替换默认背景和网站徽标：  
  
-   位图 (* .bmp, \*.dib, \*rle)  
  
-   GIF (*.gif)  
  
-   PNG (*.png)  
  
-   JPG (*.jpg)  
  
###  <a name="BKMK_RepairRWA"></a>修复远程 Web 访问  
 修复向导可以帮助检测并解决你的路由器或域名问题。 可使用两种方式来发现远程 Web 访问的问题：  
  
-   在仪表板的“服务器设置”中，在“随处访问”选项卡上，将显示带有红色 X 的图标以及问题描述。  
  
-   警报查看器中的警报。  
  
> [!NOTE]
>  在启用远程 Web 访问后，修复向导才可用。 有关打开远程 Web 访问的信息，请参阅 [Turn on Remote Web Access](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA)。  
  
##### <a name="to-repair-remote-web-access"></a>修复远程 Web 访问  
  
1.  登录到仪表板。  
  
2.  单击“设置”，然后单击“随处访问”选项卡。  
  
3.  单击“修复”。 “修复远程 Web 访问”  向导将启动。  
  
4.  单击“下一步” 。 该向导分析远程 Web 访问、标识问题并尝试修复问题。  
  
5.  如果你在向导完成后收到一个警报，你可以单击“重试”以尝试重新修复问题。 如果你仍收到警报，请检查该警报以获取有关问题和疑难解答步骤的其他信息。  
  
##  <a name="BKMK_5"></a>远程 Web 访问疑难解答  
  
-   [排查远程 Web 访问连接问题](../support/Troubleshoot-Remote-Web-Access-connectivity-in-Windows-Server-Essentials.md)  
  
-   [排查防火墙问题](../support/Troubleshoot-your-firewall-in-Windows-Server-Essentials.md)  
  
-   [随处访问疑难解答](../support/Troubleshoot-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
## <a name="see-also"></a>请参阅  
  
-   [远程桌面选项](Remote-desktop-options.md)  
  
-   [使用远程 Web 访问](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理随处访问](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
