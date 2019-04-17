---
title: "管理 Windows Server Essentials 中的 Web 远程访问"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.openlocfilehash: de01b2fd2395377b6e7b3349b9862eb0e51a59b8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="manage-remote-web-access-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的 Web 远程访问

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
 
 远程 Web 访问在 Windows Server Essentials 或 Windows Server 2012 R2 与 Windows Server Essentials 体验角色安装，提供了更流畅、触摸体验更加友好的浏览器体验，用于访问应用和从任何地方的数据需要 Internet 连接和使用几乎所有设备。 若要使用远程网站访问的功能，必须使用设置任何地方访问向导，先将其打开，然后设置你的路由器并域名。  
  
## <a name="in-this-topic"></a>在本主题  
  
-   [打开并配置远程访问 Web](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [设置你的路由器](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [设置你的域名](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [自定义远程 Web 访问](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [解决了远程 Web 访问](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_5)  
  
##  <a name="BKMK_1"></a>打开并配置远程访问 Web  
 以下主题将帮助你打开并配置远程访问 Web:  
  
-   [远程访问 Web 概述](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [打开 Web 远程访问](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA)  
  
-   [更改地区](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Region)  
  
-   [管理远程网站的访问权限](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManagePerms)  
  
-   [安全的 Web 远程访问](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SecureRWA)  
  
-   [管理用户远程网站访问和 VPN](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManageRWAVPN)  
  
###  <a name="BKMK_Overview"></a>远程访问 Web 概述  
 当你离开办公室，可以打开 web 浏览器并访问 Internet 的任何位置访问远程 Web 访问。 在远程 Web 访问，你可以：  
  
-   访问共享的文件和文件夹的服务器上。  
  
-   访问你的服务器和网络上的计算机。 这意味着你可以访问网络计算机的桌面上，如同在办公室内坐。  
  
  远程 Web 访问不处于打开状态，默认情况。 安装任何地方访问向导运行时，该向导将尝试设置你的路由器并 Internet 连接。 远程访问 Web 处于打开状态后，你可以设置服务器域名并自定义远程 Web 访问。 你还可以设置路由器再次如果更改你的路由器。  
  
 若要访问远程 Web 访问不自动授予权限当你添加新的用户帐户。 当你添加用户帐户时，你可以选择允许访问共享的文件夹，在媒体库、计算机、主页链接和服务器仪表板。 你还可以指定，用户不允许使用远程 Web 访问。  
  
 每个用户帐户在上显示远程 Web 访问设置**用户**Windows Server Essentials 仪表板中的选项卡。 若要更改远程 Web 访问设置，请右键单击用户帐户时，，然后单击**查看帐户属性**。  
  
###  <a name="BKMK_TurnOnRWA"></a>打开 Web 远程访问  
 你可以打开远程访问 Web 通过运行服务器仪表板上安装任何地方访问向导。  
  
##### <a name="to-turn-on-remote-web-access"></a>若要打开远程访问 Web  
  
1.  打开的面板。  
  
2.  单击**设置**，然后单击**任何地方访问**选项卡。  
  
3.  单击**配置**。 显示设置任何地方访问向导。  
  
4.  在**选择任何地方访问功能供**页上，选择**远程访问 Web**复选框。  
  
5.  按照说明来完成向导。  
  
###  <a name="BKMK_Region"></a>更改地区  
 你必须要更改地区设置，在 Windows Server Essentials 网络管理员联系。  
  
##### <a name="to-change-the-region-setting"></a>若要更改地区设置  
  
1.  在连接到 Windows Server Essentials 计算机上，打开仪表板。  
  
2.  单击**设置**。  
  
3.  在**常规**选项卡上，单击的下拉列表中**服务器的国家/地区位置**部分。  
  
4.  从下拉列表中，选择新的区域，然后单击**应用**接受的新的区域设置。  
  
###  <a name="BKMK_ManagePerms"></a>管理远程网站的访问权限  
 当你在 Windows Server Essentials 添加用户帐户时，新的用户允许默认情况下要使用远程网站访问。 如果您选择不允许远程访问的网站，对于用户帐户，，然后查找用户需要使用远程访问 Web，你可以更新用户帐户的 s 属性。  
  
##### <a name="to-manage-remote-web-access-permissions-for-a-user-account"></a>若要管理远程 Web 访问权限的用户帐户  
  
1.  登录到仪表板上，然后单击**用户**。  
  
2.  单击你想要管理，然后单击的用户帐户**查看帐户属性**中**任务**窗格。  
  
3.  在**属性**对话框中，单击**任何地方访问**选项卡。  
  
4.  在**任何地方访问**选项卡上，选择**允许远程网站访问和 web 服务的应用程序访问**复选框，以便用户可以使用连接到服务器远程 Web 访问。  
  
5.  单击**应用**，然后单击**确定**。  
  
 有关详细信息，请参阅[管理用户帐户](Manage-User-Accounts-in-Windows-Server-Essentials.md)。  
  
###  <a name="BKMK_SecureRWA"></a>安全的 Web 远程访问  
 Windows Server 软件包使用安全证书来帮助保护互换该软件和 web 浏览器的信息。 在你的计算机上安装接头软件时，Windows Server Essentials 的安全证书将添加到你的计算机上的受信任的证书列表。 用户访问远程 Web 访问你的 office 离开时的最佳方法是使用已安装在其上的接头软件笔记本电脑。  
  
> [!WARNING]
>  使用远程 Web 访问从公共场所或不受信任的其他计算机的用户都应确保他们注销网站计算机无人照看前或被完成与他们的会话时。  
  
###  <a name="BKMK_ManageRWAVPN"></a>管理用户远程网站访问和 VPN  
 你可以使用 VPN 可连接到 Windows Server Essentials 访问存储在服务器的你所有资源。 这是非常有用，如果你有可用于连接到 VPN 连接通过托管的 Windows Server Essentials 服务器的网络帐户与设置 client 计算机。 托管 Windows Server Essentials 服务器上的所有新创建的用户帐户必须使用 VPN 可首次登录到 client 计算机。  
  
##### <a name="to-set-vpn-and-remote-web-access-permissions-for-network-users"></a>若要设置为网络用户的 VPN 和远程 Web 访问权限  
  
1.  打开的面板。  
  
2.  在导航栏中，单击**用户**。  
  
3.  在用户帐户列表中，选择你想要授予权限才能远程访问桌面的用户帐户。  
  
4.  在**< 用户 Account\ > 任务**窗格中，单击**属性**。  
  
5.  在**< 用户 Account\ > 属性**，单击**任何地方访问**选项卡。  
  
6.  在**任何地方访问**选项卡上，请执行以下操作：  
  
    1.  若要使用户可以使用 VPN 连接到服务器，请选择**允许虚拟专用网络 (VPN)**复选框。  
  
    2.  若要使用户可以通过使用远程连接到服务器，请选择**允许远程网站访问和 web 服务的应用程序访问**复选框。  
  
7.  单击**应用**，然后单击**确定**。  
  
##  <a name="BKMK_2"></a>设置你的路由器  
 远程网站访问配置服务器时，安装任何地方访问向导将尝试路由器设置。 如果你更改路由器或更改在路由器上的设置，你必须重新运行设置你的路由器向导。 有关详细信息，请参阅以下主题：  
  
-   [设置你的路由器](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpRouter)  
  
-   [替换路由器](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ReplaceRouter)  
  
-   [定义的网络位置](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_NetworkLocation)  
  
-   [允许远程桌面服务 ActiveX 控件](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ActiveX)  
  
###  <a name="BKMK_SetUpRouter"></a>设置你的路由器  
 在此步骤中，Windows Server Essentials 尝试自动通过 UPnP 命令来配置你的路由器。 若要执行此操作，你的路由器必须支持 UPnP 标准，并且必须在你的路由器中启用 UPnP 设置。  
  
> [!NOTE]
>  Windows Server Essentials 支持的网络要求，应按照你的网络配置。 在你的网络应只有一个路由器。  
  
 如果未通过设置你域名称向导设置路由器，你必须手动转发 443 端口。 有关如何设置端口转发路由器上的信息，请参阅[路由器设置](https://social.technet.microsoft.com/wiki/contents/articles/windows-small-business-server-2011-essentials-router-setup.aspx)。  
  
###  <a name="BKMK_ReplaceRouter"></a>替换路由器  
 更换路由器根据制造商 s 的说明进行操作，并运行设置你的路由器向导配置新路由器。  
  
##### <a name="to-set-up-your-new-router"></a>若要将新路由器设置为  
  
1.  在 Windows Server Essentials 仪表板中，单击**设置**。  
  
2.  单击**任何地方访问**选项卡，然后在**路由器**部分中，单击**设置**。 设置你的路由器向导启动。  
  
3.  按照该向导中的说明完成新路由器设置。  
  
###  <a name="BKMK_NetworkLocation"></a>定义的网络位置  
 网络位置是当你连接到网络时，适用于 Windows 的网络设置集合。 设置可以基于你使用的网络类型自定义和而异。 网络位置设置确定（如文件和打印机共享、网络发现和公共文件夹共享）的某些功能会打开或关闭。 当你需要连接到不同的网络，网络位置非常有用。  
  
 例如，你可能拥有家庭和工作使用笔记本电脑。 当你在办公室，请连接到 office 网络。 但是，推出家庭，你使用你的笔记本电脑访问和游玩视频和存储在家庭的服务器的音乐。 当您连接到新的网络，并指定位置类型时，Windows 将分配网络配置文件，该位置的类型预设。 下次你连接到该网络，Windows 可识别的网络，并自动将分配了正确的设置。 这添加一层安全性，以帮助保护你的计算机上的信息，并打开你需要针对该位置的网络功能。  
  
 有四种类型的网络位置：  
  
-   **家庭网络**选择该网络中的家庭网络或你了解并信任的人和网络上的设备。 家庭网络上的计算机可属于家庭组。 网络发现它允许你查看网络上的其他计算机和设备，并允许其他网络用户，若要查看你的计算机的家庭网络中处于打开状态。  
  
-   **工作网络**选择小型办公室该网络或其他工作区的网络。 网络发现，它允许你查看网络上的其他计算机和设备，并允许其他网络用户，若要查看你的计算机，默认情况下，已打开，但无法创建或加入家庭组。  
  
-   **公共网络**选择公共场所（如咖啡厅或机场）该网络。 此位置被设计，以使你的计算机不可见到其他计算机并帮助保护你的计算机免受恶意软件从 Internet。 家庭组适用于公共网络并不网络发现处于关闭状态。 如果你已连接到 Internet 直接而无需使用路由器，或者如果你有移动宽带连接，你还应该选择此选项。  
  
-   **域**选择此域如企业工作区的网络。 这种类型的网络位置控制你的网络管理员，并且无法选择或更改。  
  
###  <a name="BKMK_ActiveX"></a>允许远程桌面服务 ActiveX 控件  
 远程桌面服务 ActiveX 控件可以访问你的家庭或企业计算机，通过 Internet，从另一台计算机使用远程 Web 访问。  
  
##### <a name="to-enable-remote-desktop-services-activex-controls"></a>若要使远程桌面服务 ActiveX 控件  
  
1.  在 Internet Explorer 中，单击**工具**，然后单击**Internet 选项**。  
  
2.  在**安全**选项卡上，单击**自定义级别**。  
  
3.  在**ActiveX 控件和插件**部分中，请执行以下操作：  
  
    1.  下**下载已签名的 ActiveX 控件**，单击**提示**。  
  
    2.  下**运行 ActiveX 控件和插件**，单击**启用**。  
  
4.  单击**确定**两次，接受所做的更改并关闭对话框。  
  
##  <a name="BKMK_3"></a>设置你的域名  
 远程访问 Web 处于打开状态后，你可以设置适用于运行的 Windows Server Essentials 你 server 域名。 如果你打算从远程计算机使用远程访问 Web，这是必需的步骤。 有关详细信息，请参阅以下主题：  
  
-   [域名称概述](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_DNOverview)  
  
-   [了解 Microsoft 个性化域名](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_PersonalizedNames)  
  
-   [使用新的或现有域名](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UseNewName)  
  
-   [设置一个域名](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpName)  
  
-   [选择域名称服务提供商](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseProvider)  
  
-   [选择一个域名](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseDomainName)  
  
-   [选择域名前缀](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Prefixes)  
  
-   [选择域名称扩展](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Extension)  
  
-   [更新或升级你域名服务](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UpdateService)  
  
-   [导出或服务器上的证书导入](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ExportCert)  
  
-   [手动设置域名称](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetNameManually)  
  
-   [查找你的域名称服务提供商](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Find)  
  
###  <a name="BKMK_DNOverview"></a>域名称概述  
 域名唯一标识你在 Internet 上的服务器。 域名以至少两部分组成：顶部级别域名 (TLD) 和第二个级别域名。 例如，在 contoso.com，com TLD 并且 contoso 第二个级别的域名。  
  
 虽然离开办公室中，你可以使用你的域名服务器上的共享的文件或网络上的计算机的访问。 在离开时，还可以管理您的服务器。 例如，你注册 contoso.com 服务器。 当你离开办公室，你可以打开 web 浏览器类型和笔记本电脑上的**contoso.com**中地址文本框中连接到你在 Windows Server Essentials 设置的远程网站访问的实例。  
  
###  <a name="BKMK_PersonalizedNames"></a>了解 Microsoft 个性化域名  
 Microsoft 个性化的域名包含以下功能：  
  
-   远程网站访问的自定义域名称 (例如，*yourhostname*。remotewebaccess.com)。 你的域名是你公共的 IP 地址相关联。  
  
-   动态 DNS 更新服务协议，以便如果更改了你公共的 IP 地址，使用你的域名远程访问 Web 将不会中断。 通常，Internet 服务提供商 (Isp) 的你的组织 s 宽带连接提供可以更改的动态公共的 IP 地址。  
  
-   受信任的证书相关联的域名称。  
  
 若要将 Microsoft 个性化的域名集成与服务器，你需要（以前称为 Windows Live ID）的 Microsoft 帐户。 如果你没有 Microsoft 帐户，您可以注册一个在[Microsoft Hotmail](https://login.live.com/)网站。  
  
> [!IMPORTANT]
>  Windows Live 允许特殊字符在你服务器不支持的 Microsoft 帐户密码。 如果你使用 Microsoft 个性化的域，请确保你的 Microsoft 帐户密码包含服务器支持的字符。 服务器不支持使用 $ 字符 /，，和 %。  
  
###  <a name="BKMK_UseNewName"></a>使用新的或现有域名  
 若要自动设置你的域名运行 Windows Server Essentials 服务器上，必须使用已设置你域名称向导中列出的域名称服务提供商。 你可以选择获取新的域名或使用现有的域名。 请执行以下任一操作：  
  
-   如果你想要获取新的域名从某个域名称服务提供商列出向导中，单击**我想要设置新的域名**。  
  
-   如果你有现有域名你从受支持的域名称服务提供商处购买，可用于安装你域名称向导设置服务器的域名。 单击**我想要使用的域名我已经拥有**，然后键入中的域名**设置为你的域名**文本框。 你必须提供的用户名和密码，你用于购买的域名。  
  
-   如果你有现有域名域名称服务提供商不支持的 Windows Server Essentials 从你购买了，并且你想要设置你域名称向导用于设置适用于你 server 域名，您可传输到某个域名称服务提供商向导中列出的域名。 单击**我想要使用的域名我已经拥有**，键入中的域名**域名**文本框中，，然后按照传输域名域名称服务提供商的网站上的说明进行操作。  
  
###  <a name="BKMK_SetUpName"></a>设置一个域名  
 当你打开远程访问 Web 时，可以选择要设置 Internet 域名服务器。  
  
##### <a name="to-set-up-or-manage-an-internet-domain-name"></a>若要设置或管理 Internet 域名  
  
1.  打开的面板。  
  
2.  单击**服务器设置**，然后单击**任何地方访问**选项卡。  
  
3.  在**域名**部分中，单击**设置**。  
  
4.  按照说明来完成向导。 如果你已经拥有一个域名和证书，该向导，可帮助你查找要购买域名和证书的域名称提供程序，或者你可以个性化的 Microsoft 域名。  
  
###  <a name="BKMK_ChooseProvider"></a>选择域名称服务提供商  
 你应该选择支持你想要使用的域扩展名域名称服务提供商。 设置你域名称向导包括合格的提供商，你可以使用指向每个提供商网站的链接的列表。 单击**更信息**获取信息的服务和提供商提供的价格每个提供商的名称旁边的链接。  
  
> [!NOTE]
>  某些域名称服务提供商提供的大量国际地区和其他提供较小的市场。 出于此原因，某些提供商可能不提供翻译为你的偏好随意选择的语言的网站。  
  
 当你购买了你的域名时，你还可以考虑购买域名系统 (DNS) 的动态更新协议服务域名称服务提供商。 DNS 的动态更新协议是一项服务，可让任何人都在 Internet 上时，该网络的 IP 地址会不断可以访问与本地网络上的资源。 你可以购买或的静态 IP 地址从 Internet 服务提供商 (ISP) 以确保不会更改你的 IP 地址。  
  
###  <a name="BKMK_ChooseDomainName"></a>选择一个域名  
 选择一个唯一标识你的企业服务器的名称。 例如，如果你的公司名称是 Contoso Ltd，你可以选择 Contoso 唯一标识你在 Internet 上的家庭或企业服务器。 如果域名不可用，请尝试另一个该名称，或或许内容完全不同的变体。  
  
 键入你的姓名可以包含以下信息：  
  
-   最大的 63 字符  
  
-   （英语或你的本地化的字符）字母、数字或连字符（-）。 名称必须开始和结束以字母或编号。  
  
    > [!NOTE]
    >  域名都是不区分大小写。  
  
###  <a name="BKMK_Prefixes"></a>选择域名前缀  
 域名包括分层标签。  
  
 **顶级域扩展**是在域名称的最右端标签。 例如，在 www.contoso.com，com 是顶级域扩展名。  
  
 **第二层域名**是旁边的顶级域扩展名标签。 第二层域名通常根据公司名称、产品或服务进行创建。 例如，在 www.contoso.com，contoso 二级域名和所选的公司名称 Contoso 医药。 第二层域有时称为主机，它有与之关联的 IP 地址。  
  
 **域名前缀**标识子域。 子域名可用来识别服务、设备或地区。 例如，Contoso 医药想要允许远程用户在登录到远程 Web 访问权限，但又不希望可向公众，以便他们创建只允许用户相应的权限来访问该网站与子域网站。 Contoso 医药设置 remote.contoso.com 作为子域，并且远程域名前缀。  
  
> [!TIP]
>  建议你使用的默认**远程**作为前缀你的域名。  
  
###  <a name="BKMK_Extension"></a>选择域名称扩展  
 当你选择域名 Internet 网站时，你还需要指定你希望使用域扩展名。 按照任何域名最终期间字母以标识扩展。 （扩展正式字词是顶级域或 TLD。）  
  
 有两种主域，则可以使用的扩展：通用和国家/地区代码。  
  
#### <a name="generic-top-level-domains"></a>一般顶级域  
 一般域扩展是三个或多个字母长度，并且通常由组织的某些类型。  
  
 **一般顶级的域的示例**  
  
|域扩展|描述|  
|----------------------|-----------------|  
|.com|通常由商业组织中，但人都可以使用。|  
|.net|专为提供网络基础结构服务的企业。|  
|.org|最初由非营利机关和其他不属于另一种通用顶级域类别的业务。 人都可以使用。|  
|.edu|限制用于教育组织。|  
  
#### <a name="country-code-top-level-domains"></a>国家/地区代码顶级域  
 这些域扩展有两个字母的长度。 它们旨在由组织中的国家或地区关联使用该代码。 某些国家/地区代码顶级域是准公民该国家或地区的使用。 其他类型是可供使用的任何人。  
  
 **国家/地区代码顶级域的示例**  
  
|域扩展|描述|  
|----------------------|-----------------|  
|.ca|以供加拿大中的网站|  
|.cn|以供中国中的网站|  
|.de|以供德国中的网站|  
|。co.uk|以供英国中的网站|  
  
 若要查看的完整列表的风险，请参阅[Internet 分配数字颁发机构网站](https://go.microsoft.com/fwlink/?LinkId=117438)。  
  
#### <a name="if-a-domain-extension-is-not-available-to-select-in-the-set-up-domain-name-wizard"></a>如果域扩展不可用，以选择设置域名称向导中的  
 设置域名称向导运行时，该向导查看你的系统信息，以确定您所在的国家或地区。 该向导然后显示你所在地区参与提供商支持那些域扩展。 如果在列表中未显示你想要域扩展，你必须选择不同的域扩展以继续。 从该向导返回列表中选择扩展。  
  
###  <a name="BKMK_UpdateService"></a>更新或升级你域名服务  
 你可能需要更新，或者如果你购买域名，但未购买证书升级你域名服务。 你必须证书针对你的域名域名称服务提供商。  
  
> [!NOTE]
>  适用于域名称服务提供商以确定你需要的证书类型。 该证书可提供便宜证书之一。 但是，您应查看文档和更高版本的安全级别证书，以确定是否他们更好地满足你的需求的功能。  
  
###  <a name="BKMK_ExportCert"></a>导出或服务器上的证书导入  
 如果你想要创建的备份副本证书或使用它在另一台服务器上，你必须导出证书。 有关导出证书的信息，请参阅[导出证书](https://go.microsoft.com/fwlink/p/?LinkId=214362)。  
  
###  <a name="BKMK_SetNameManually"></a>手动设置域名称  
 如果您选择此选项，服务器不监视器也维护你的域名，并且配置问题是否，它不会不提醒你。 你还可以在以下任一是否考虑此选项：  
  
-   你所在的国家或地区中列出了没有合作伙伴域名称提供商。  
  
-   列出合作伙伴域提供商不支持你域扩展名。  
  
-   你有现有域当前未合作伙伴的域名称提供商的名称，你不希望将该域名传输到 Windows Server Essentials 支持的域名称提供商。  
  
-   该向导将不会列出你想要使用时，域扩展名，但当前未合作伙伴域名称商提供扩展。  
  
 如果你选择要设置你的域名手动，是处理创建你的域记录你域名称服务提供商。  
  
##### <a name="to-create-an-a-record"></a>若要创建记录  
  
1.  在主机名称，如远程决定。 这是域名前缀。 域名前缀以及你的域名将定义的 URL，以打开远程访问 Web 登录页面。例如，**http://remote.contoso.com**。  
  
2.  在你域名称服务提供商配置仪表板（通常在其网页上），创建一个记录你决定在第 1 步中的主名称。 确保您的记录中指定的 IP 地址 WAN 侧 (Internet 面对侧) 路由器的 IP 地址。 请参阅路由器的文档，找到您 WAN IP 地址。  
  
3.  建议你联系 Internet 服务提供商 (ISP) 购买你的网络的静态 IP 地址。 这将确保的 IP 地址不会更改，并且你 DNS 条目不变为过期。  
  
     如果没有获取从 ISP 的静态 IP 地址的选项，你还可以考虑从你的域名称服务提供商或服务提供购买域名系统 (DNS) 的动态更新协议服务。 DNS 的动态更新协议是一项服务，以便可以解析为你的域名 IP 地址，即使 IP 地址发生了变化，你的网络的 WAN IP 地址保持最新。  
  
4.  该向导将提示您时，将导入的受信任的证书。 如果你没有受信任的证书，可以从一个向导中列出的受支持的域名称提供商获取一个或从你选择的受信任的提供商购买一个。 有关受信任的证书的详细信息，请联系你的域名称提供商。  
  
###  <a name="BKMK_Find"></a>查找你的域名称服务提供商  
  
##### <a name="to-find-the-domain-name-service-provider-for-your-domain-name"></a>若要查找你的域名域名称服务提供商  
  
1.  打开 web 浏览器，然后键入**www.internic.com**以转到主页 Internic 页面的地址栏中。  
  
2.  在 Internic 主页上，单击**Whois**。  
  
3.  在**Whois**框中，键入你的域名 (例如 contoso.com)。  
  
4.  单击**域**选项，然后依次单击**提交**。  
  
5.  在搜索结果中，您域名称服务提供商的名称下列出**注册**。  
  
##  <a name="BKMK_4"></a>自定义远程 Web 访问  
 你可以通过添加个人徽标或背景图像自定义您远程网站访问的站点。 你还可以在家庭页面添加链接，以便此信息提供给您的所有用户。 有关详细信息，请参阅以下主题：  
  
-   [自定义远程 Web 访问](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeRWA)  
  
-   [自定义背景和徽标的图像](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeImages)  
  
-   [修复 Web 远程访问](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_RepairRWA)  
  
###  <a name="BKMK_CustomizeRWA"></a>自定义远程 Web 访问  
 通过更改该网站的标题、更改背景图像和徽标，并添加链接到其他网站在主页上，你可以自定义远程 Web 访问。  
  
##### <a name="to-customize-remote-web-access"></a>若要自定义远程访问 Web  
  
1.  打开的面板。  
  
2.  单击**设置**，然后单击**任何地方访问**选项卡。  
  
3.  在**网站设置**部分中，单击**自定义**。  
  
4.  当你完成自定义远程 Web 访问时，请单击**确定**。 更改远程访问 Web 上的进行测试。  
  
###  <a name="BKMK_CustomizeImages"></a>自定义背景和徽标的图像  
 此部分中提供了你可以使用自定义远程 Web 访问图像的相关信息。  
  
#### <a name="image-size"></a>图像大小  
 **徽标的图像**  
  
 建议你使用的是 32 x 32 像素徽标图像。 变得更大图像收缩到 32 x 32 和较小的图像 32 x 32，无法扭曲图像到拉伸。  
  
 **背景图像**  
  
 尽管没有大小限制对于背景图像，获取最佳结果，建议你使用的是约 800 x 500 像素的图像。 背景图像放置在中心（水平和垂直）的登录页面。 若要帮助使登录页面上的文本更易于阅读，背景图像中心应该颜色内的光。  
  
#### <a name="image-file-types"></a>图像文件类型  
 以下图像文件类型可替换默认的背景和网站徽标：  
  
-   位图 *.bmp、\*.dib（\*.rle）  
  
-   GIF (*.gif)  
  
-   PNG (*.png)  
  
-   JPG (*.jpg)  
  
###  <a name="BKMK_RepairRWA"></a>修复 Web 远程访问  
 修复向导帮助检测和解决你的路由器或域名的问题。 有两种方法来使用远程访问 Web 发现问题：  
  
-   服务器仪表板上，在任何地方访问选项卡上的设置中的图标将显示红色 X 以及的问题描述。  
  
-   在查看器中通知的警报。  
  
> [!NOTE]
>  修复向导不可用，直到你打开远程访问 Web。 有关打开远程网站访问的信息，请参阅[打开远程访问 Web](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA)。  
  
##### <a name="to-repair-remote-web-access"></a>若要修复远程访问 Web  
  
1.  登录到仪表板。  
  
2.  单击**设置**，然后单击**任何地方访问**选项卡。  
  
3.  单击**修复**。 **修复远程访问 Web**向导启动。  
  
4.  单击**下一步**。 该向导分析远程 Web 访问、识别问题，然后尝试修复该问题。  
  
5.  如果你收到警报完成向导后，你可以单击**重试**尝试再次修复该问题。 如果你继续收到通知，查看有关的问题以及疑难解答步骤的其他信息的通知。  
  
##  <a name="BKMK_5"></a>解决了远程 Web 访问  
  
-   [解决了远程访问 Web 连接](../support/Troubleshoot-Remote-Web-Access-connectivity-in-Windows-Server-Essentials.md)  
  
-   [解决你的防火墙](../support/Troubleshoot-your-firewall-in-Windows-Server-Essentials.md)  
  
-   [疑难解答任意位置的访问权限](../support/Troubleshoot-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
## <a name="see-also"></a>请参阅  
  
-   [远程桌面选项](Remote-desktop-options.md)  
  
-   [使用远程 Web 访问](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理任意位置的访问权限](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
