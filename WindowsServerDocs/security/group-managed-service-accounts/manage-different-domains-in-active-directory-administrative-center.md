---
title: "管理不同的域的 Active Directory 管理中心"
ms.prod: windows-server-threshold
description: "Windows Server 安全"
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: 166351c3-4076-48be-aa8f-797adf1e9d68
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 5f253bd4952d8a347e97eafdb38d86fa98024b8d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>管理不同的域的 Active Directory 管理中心

>适用于：Windows Server（半年通道），Windows Server 2016

  当你打开 Active Directory 管理，你将当前登录到此计算机的域 \ (本地 domain\) 中显示 Active Directory 管理中心导航窗格 \(the left pane\)。 根据你的登录凭据的当前设置的权利，你可以查看或管理中此本地域的 Active Directory 对象。

 你还可以使用登录凭据和 Active Directory 管理中心的同一个实例的同一的组查看或管理 Active Directory 相同的树林中的任何其他域或另一台已与本地域的已建立的信任的森林中的某个域中的对象。 One\ 方式信任和 two\ 方式信任一起受支持。

> [!NOTE]
>  如果之间域 A 和域 B one\ 方式信任哪些中的用户域 A 可以通过访问 B 域中的资源，但 B 域中的用户无法访问资源域，如果你运行的 Active Directory 管理中心在其中域 A 是你地域计算机上，你可以连接到域 B 与当前的登录凭据，并在同一个实例 Active Directory 管理中心。 如果你计算机上运行 Active Directory 管理中心域 B 其中你的本地域，你无法连接到域 A 用的同一组中的同一个实例的凭据，但 < DICT__Active Directory >active Directory </DICT__Active Directory>管理中心。

 完成此过程需要没有最低组成员。

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012：若要管理外域中使用的当前套 Active Directory 管理中心所选实例登录凭据

1.  若要打开 Active Directory 管理中心，**服务器管理器**，单击**工具**，然后单击**Active Directory 管理中心**。

    > [!NOTE]
    >  若要打开 Active Directory 管理中心的另一个方法是单击**开始**，然后键入**dsac.exe**。

2.  若要打开**添加导航节点**，单击**管理**，然后单击**添加导航节点**下图所示。

     ![屏幕截图显示 * * 添加导航节点 * * UI](media/ADDS_ADACAddNavNode.gif)

3.  在**添加导航节点**，单击**连接到其他域**下图所示。

     ![屏幕截图显示 * * 添加导航节点 * * UI](media/ADDS_ADACConnectToDomain.gif)

4.  在**连接到**，键入你想要管理外的域名 \ (例如，**contoso.com**\)，然后单击**确定**。

5.  当你已成功连接到外部域时，浏览的列**添加导航节点**窗口中，选择或多个要添加到你 Active Directory 管理中心导航窗格中，然后单击**确定**。

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2：若要管理外域的 Active Directory 管理中心使用套当前登录所选实例凭据

1.  若要打开 Active Directory 管理中心，请单击**开始**，单击**管理工具**，然后单击**Active Directory 管理中心**。

    > [!NOTE]
    >  若要打开 Active Directory 管理中心的另一个方法是单击**开始**，单击**运行**，然后键入**dsac.exe**。

2.  若要打开**添加导航节点**，附近的 Active Directory 管理中心窗口顶部，单击**添加导航节点**下图所示。

     ![屏幕截图显示 * * 添加导航节点 * * UI](media/click_add_nav_nodes.gif)

    > [!NOTE]
    >  打开另一个方法**添加导航节点**是 right\ 键单击在 Active Directory 管理中心中的导航窗格中的空白区域中的任何位置，然后单击**添加导航节点**。

3.  在**添加导航节点**，单击**连接到其他域**下图所示。

     ![屏幕截图显示 * * 添加导航节点 * * * * 连接到其他域 * * UI](media/add_nav_nodes.gif)

4.  在**连接到**，键入你想要管理外的域名 \ (例如，**contoso.com**\)，然后单击**确定**。

5.  当你已成功连接到外部域时，浏览的列**添加导航节点**窗口中，选择或多个要添加到你 Active Directory 管理中心导航窗格中，然后单击**确定**。

 自定义 Active Directory 管理中心导航窗格中的详细信息，请参阅[自定义 Active Directory 管理中心导航窗格](customize-the-active-directory-administrative-center-navigation-pane.md)。

 你还可以通过使用一组的登录凭据不同于你的登录凭据的当前设置打开 Active Directory 管理中心。 下面的过程中的命令可能很有用，如果你登录到的计算机上运行的 Active Directory 管理中心使用普通用户凭据，但您想要使用 Active Directory 管理中心在此计算机上管理你地域以管理员身份登录。 \（此命令还可以有用，如果你想要使用 Active Directory 管理中心远程管理外域不同于你地域与是一组的凭据不同于你的登录凭据的当前设置。 但是外, 域必须与本地域的已建立的信任。\)

 完成此过程需要没有最低组成员。

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>若要管理使用不同的登录凭据当前集的登录凭据域

1.  若要打开 Active Directory 管理中心在命令提示符下，键入以下命令，然后按 ENTER:

     `runas /user:<domain\user> dsac`

     其中`<domain\user>`是你想要打开 Active Directory 管理中心的凭据套和`dsac`是 Active Directory 管理中心可执行文件名称 \(Dsac.exe\)。

     例如，键入以下命令，并按 enter 键：

     `runas /user:contoso\administrator dsac`

2.  当 Active Directory 管理中心处于打开状态，浏览导航窗格中查看或管理您的 Active Directory 的域。

  

