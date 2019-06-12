---
title: 管理 Active Directory 管理中心内的不同域
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bd5650724272422d09e87b7eecf10f825b00fabf
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447039"
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>管理 Active Directory 管理中心内的不同域

>适用于：Windows 服务器 （半年频道），Windows Server 2016

  打开 Active Directory 管理，你当前在此计算机登录到域时\(本地域\)Active Directory 管理中心导航窗格中会显示\(的左的窗格\). 具体取决于你当前登录凭据集的权限，可以查看或管理此本地域中的 Active Directory 对象。

 此外可以使用相同的登录凭据和 Active Directory 管理中心内的同一实例集以查看或管理在同一个林中的任何其他域或具有与本地建立信任关系的另一个林的域中的 Active Directory 对象域。 这两个之一\-双向信任关系和两个\-支持双向信任关系。

> [!NOTE]
>  如果没有一个\-双向信任域 A 和域 B 通过其域 A 中的用户可以访问域 B 中的资源，但如果您正在运行的计算机上的 Active Directory 管理中心内，域 B 中的用户无法访问域 A 中的资源之间其中一个是本地域，您可以连接到域 B 与当前设置的登录凭据和 Active Directory 管理中心内的同一实例中。 但如果在计算机上运行 Active Directory 管理中心内，其中域 B 本地域，则无法连接到域 A 具有相同的 Active Directory 管理中心内的同一实例中的凭据集。

 不需要任何最低组成员身份即可完成此过程。

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012:若要管理外部域中使用当前登录凭据集的 Active Directory 管理中心的所选实例

1.  若要打开 Active Directory 管理中心内，在**服务器管理器**，单击**工具**，然后单击**Active Directory 管理中心**。

    > [!NOTE]
    >  若要打开 Active Directory 管理中心内的另一种方法是单击**启动**，然后键入**dsac.exe**。

2.  若要打开**添加导航节点**，单击**管理**，然后单击**添加导航节点**，如下图中所示。

     ![屏幕截图显示 * * 添加导航节点 * * UI](media/ADDS_ADACAddNavNode.gif)

3.  在中**添加导航节点**，单击**连接到其他域**，如下图中所示。

     ![屏幕截图显示 * * 添加导航节点 * * UI](media/ADDS_ADACConnectToDomain.gif)

4.  在中**连接到**，键入你想要管理的外部域的名称\(等**contoso.com**\)，然后单击**确定**。

5.  已成功连接到外部域，浏览在中的列**添加导航节点**窗口中，选择或多个容器添加到 Active Directory 管理中心导航窗格，并然后单击**确定**。

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2：若要管理外部域中使用当前登录凭据集的 Active Directory 管理中心的所选实例

1. 若要打开 Active Directory 管理中心内，单击**启动**，单击**管理工具**，然后单击**Active Directory 管理中心**。

   > [!NOTE]
   >  若要打开 Active Directory 管理中心内的另一种方法是单击**启动**，单击**运行**，然后键入**dsac.exe**。

2. 若要打开**添加导航节点**，附近的 Active Directory 管理中心窗口顶部，单击**添加导航节点**，如下图中所示。

    ![屏幕截图显示 * * 添加导航节点 * * UI](media/click_add_nav_nodes.gif)

   > [!NOTE]
   >  另一种方法打开**添加导航节点**是右侧\-在 Active Directory 管理中心导航窗格中的空格中任意位置单击，然后单击**添加导航节点**.

3. 在中**添加导航节点**，单击**连接到其他域**，如下图中所示。

    ![屏幕截图显示 * * 添加导航节点 * * * * 连接到其他域 * * UI](media/add_nav_nodes.gif)

4. 在中**连接到**，键入你想要管理的外部域的名称\(等**contoso.com**\)，然后单击**确定**。

5. 已成功连接到外部域，浏览在中的列**添加导航节点**窗口中，选择或多个容器添加到 Active Directory 管理中心导航窗格，并然后单击**确定**。

   有关自定义 Active Directory 管理中心导航窗格的详细信息，请参阅[自定义 Active Directory 管理中心导航窗格](customize-the-active-directory-administrative-center-navigation-pane.md)。

   此外可以使用一组登录凭据不同于你当前登录凭据集打开 Active Directory 管理中心。 以下过程中的该命令可以是登录到使用普通用户凭据运行 Active Directory 管理中心的计算机的情况下很有用，但你想要在此计算机上使用 Active Directory 管理中心来管理应用以管理员身份的本地域。 \(此命令也可以是你想要使用 Active Directory 管理中心远程管理不同于本地域不同于你当前登录凭据集的一组凭据与外部域的情况下很有用。 但是，外部域必须具有与本地域建立信任关系。\)

   不需要任何最低组成员身份即可完成此过程。

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>使用与当前登录凭据集不同的登录凭据管理域的步骤

1.  若要打开 Active Directory 管理中心内，在命令提示符下，键入以下命令，然后按 ENTER:

     `runas /user:<domain\user> dsac`

     其中`<domain\user>`是你想要打开 Active Directory 管理中心内使用的凭据集并`dsac`是 Active Directory 管理中心可执行文件名\(Dsac.exe\)。

     例如，键入以下命令，然后按 Enter：

     `runas /user:contoso\administrator dsac`

2.  打开 Active Directory 管理中心时，即可浏览导航窗格中，若要查看或管理你的 Active Directory 域。

  

