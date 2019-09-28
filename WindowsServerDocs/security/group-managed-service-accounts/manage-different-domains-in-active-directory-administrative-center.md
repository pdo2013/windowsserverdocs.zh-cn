---
title: 管理 Active Directory 管理中心中的不同域
ms.prod: windows-server
description: Windows Server 安全
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: 166351c3-4076-48be-aa8f-797adf1e9d68
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 71edf6bb38cc665fe5c780ce986d0c0b8807d6ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386928"
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>管理 Active Directory 管理中心中的不同域

>适用于：Windows Server（半年频道）、Windows Server 2016

  当你打开 Active Directory 管理 "时，你当前登录到此计算机上的域 @no__t-Active Directory 管理中心导航窗格中显示" 导航窗格 @no__t 2the 左窗格 @ no__t。 根据当前登录凭据集的权限，你可以查看或管理本地域中的 Active Directory 对象。

 你还可以使用相同的一组登录凭据和 Active Directory 管理中心的同一个实例来查看或管理同一林中的任何其他域中的 Active Directory 对象，或者使用与本地域名. 同时支持一个 @ no__t-0way 信任和两个 @ no__t-1way 信任。

> [!NOTE]
>  如果域 a 和域 B 之间有一个 @ no__t-0way 信任，域 A 中的用户可以访问域 B 中的资源，但是域 B 中的用户无法访问域 A 中的资源，但如果你在计算机上运行 Active Directory 管理中心域 A 是本地域，你可以使用当前的一组登录凭据和 Active Directory 管理中心的同一实例连接到域 B。 但如果在域 B 是本地域的计算机上运行 Active Directory 管理中心，则无法使用同一 Active Directory 管理中心实例中的相同凭据集连接到域 A。

 不需要任何最低组成员身份即可完成此过程。

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012：使用当前的一组登录凭据管理 Active Directory 管理中心所选实例中的外部域

1.  若要打开 Active Directory 管理中心，请在**服务器管理器**中单击 "**工具**"，然后单击 " **Active Directory 管理中心**"。

    > [!NOTE]
    >  打开 Active Directory 管理中心的另一种方法是单击 "**开始**"，然后键入**dsac.exe**。

2.  若要打开 "**添加导航节点**"，请单击 "**管理**"，然后单击 "**添加导航节点**"，如下图所示。

     ![显示 * * 添加导航节点的屏幕截图 * * UI](media/ADDS_ADACAddNavNode.gif)

3.  在 "**添加导航节点**" 中，单击 "**连接到其他域**"，如下图所示。

     ![显示 * * 添加导航节点的屏幕截图 * * UI](media/ADDS_ADACConnectToDomain.gif)

4.  在 "**连接到**" 中，键入要管理的外部域的名称 \(for example， **contoso.com**\)，然后单击 **"确定"** 。

5.  成功连接到外部域后，浏览 "**添加导航节点**" 窗口中的列，选择要添加到 Active Directory 管理中心导航窗格中的容器，然后单击 **"确定"** .

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2：使用当前的一组登录凭据管理 Active Directory 管理中心所选实例中的外部域

1. 若要打开 Active Directory 管理中心，请单击 "**开始**"，单击 "**管理工具**"，然后单击 " **Active Directory 管理中心**"。

   > [!NOTE]
   >  打开 Active Directory 管理中心的另一种方法是单击 "**开始**"，再单击 "**运行**"，然后键入**dsac.exe**。

2. 若要打开 "**添加导航节点**"，在 "Active Directory 管理中心" 窗口顶部附近，单击 "**添加导航节点**"，如下图所示。

    ![显示 * * 添加导航节点的屏幕截图 * * UI](media/click_add_nav_nodes.gif)

   > [!NOTE]
   >  打开 "**添加导航节点**" 的另一种方法是在 Active Directory 管理中心导航窗格的空白区域中的任意位置右 @ no__t-1click，然后单击 "**添加导航节点**"。

3. 在 "**添加导航节点**" 中，单击 "**连接到其他域**"，如下图所示。

    ![显示 * * 添加导航节点的屏幕截图 * * * 连接到其他域 * * UI](media/add_nav_nodes.gif)

4. 在 "**连接到**" 中，键入要管理的外部域的名称 \(for example， **contoso.com**\)，然后单击 **"确定"** 。

5. 成功连接到外部域后，浏览 "**添加导航节点**" 窗口中的列，选择要添加到 Active Directory 管理中心导航窗格中的容器，然后单击 **"确定"** .

   有关自定义 Active Directory 管理中心导航窗格的详细信息，请参阅[自定义 Active Directory 管理中心导航窗格](customize-the-active-directory-administrative-center-navigation-pane.md)。

   你还可以通过使用一组不同于当前登录凭据集的登录凭据来打开 Active Directory 管理中心。 如果你使用普通用户凭据登录到运行 Active Directory 管理中心的计算机，但你想要在此计算机上使用 Active Directory 管理中心来管理你的作为管理员的本地域。 如果你想要使用 Active Directory 管理中心以远程方式管理与你的本地域不同的外域，并使用一组与当前登录凭据不同的凭据，则0This 命令也会很有用。 @no__t 但是，外部域必须与本地域建立了信任关系。 \)

   不需要任何最低组成员身份即可完成此过程。

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>使用与当前登录凭据集不同的登录凭据管理域的步骤

1.  若要打开 Active Directory 管理中心，请在命令提示符下键入以下命令，然后按 ENTER：

     `runas /user:<domain\user> dsac`

     其中 `<domain\user>` 是要用来打开 Active Directory 管理中心的一组凭据，`dsac` 是 Active Directory 管理中心可执行文件名称 \(Dsac @ no__t。

     例如，键入以下命令，然后按 Enter：

     `runas /user:contoso\administrator dsac`

2.  打开 Active Directory 管理中心后，浏览导航窗格来查看或管理 Active Directory 域。

  

