---
title: 文件夹重定向和漫游用户配置文件部署主计算机
description: 了解如何启用主计算机支持，并指定用户的文件夹重定向和漫游用户配置文件的主计算机。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 39b790f39a2bf9c6334eb2176aa2e5f2e0196c0c
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475969"
---
# <a name="deploy-primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>文件夹重定向和漫游用户配置文件部署主计算机

>适用于：Windows 10 中，Windows 8、 Windows 8.1、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 中，Windows Server 2012 R2

本主题介绍如何启用主计算机支持和指定的用户的主计算机。 执行此操作，来控制哪些计算机使用文件夹重定向和漫游用户配置文件。

>[!IMPORTANT]
>在启用漫游用户配置文件的主计算机支持时，始终启用主计算机支持文件夹重定向以及。 这会使文档和其他用户文件超出了用户配置文件，可帮助配置文件保持较小，和登录时间保持快速。

## <a name="prerequisites"></a>先决条件

## <a name="software-requirements"></a>软件要求

主计算机支持具有以下要求：

- 必须更新 Active Directory 域服务 (AD DS) 架构，以包含 Windows Server 2012 架构新增内容 （安装 Windows Server 2012 域控制器会自动更新架构）。 有关更新 AD DS 架构的信息，请参阅[Adprep.exe 集成](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>)并[运行 Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>)。
- 客户端计算机必须运行 Windows 10、 Windows 8.1，Windows 8、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。

>[!TIP]
>虽然主计算机支持需要文件夹重定向和/或漫游用户配置文件中，如果你首次部署这些技术，但最好设置启用的 Gpo 的配置文件夹重定向之前的主计算机支持和漫游用户配置文件。 这会防止在启用主计算机支持之前将用户数据复制到非主计算机。 有关配置信息，请参阅[部署文件夹重定向](deploy-folder-redirection.md)并[部署漫游用户配置文件](deploy-roaming-user-profiles.md)。

## <a name="step-1-designate-primary-computers-for-users"></a>第 1 步：为用户指定主计算机

部署主计算机支持的第一步指定为每个用户的主计算机。 为此，请使用 Active Directory 管理中心来获取相关计算机的可分辨的名称，然后设置**msDs PrimaryComputer**属性。

>[!TIP]
>若要使用 Windows PowerShell 使用的主计算机，请参阅博客文章[深入发掘 Windows 8 的主要计算机](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>)。

下面介绍了如何指定用户的主计算机：

1. 在装有 Active Directory 管理工具的计算机上打开服务器管理器。
2. 上**工具**菜单中，选择**Active Directory 管理中心**。 此时将出现 Active Directory 管理中心。
3. 导航到**计算机**相应域中的容器。
4. 右键单击你想要将指定为主计算机，然后选择计算机**属性**。
5. 在导航窗格中，选择**扩展**。
6. 选择**属性编辑器**选项卡上，滚动到**distinguishedName**，选择**视图**，右键单击列出的值，选择**复制**，选择**确定**，然后选择**取消**。
7. 导航到**用户**容器中适当的域中，右键单击你想要将计算机分配，然后选择的用户**属性**。
8. 在导航窗格中，选择**扩展**。
9. 选择**属性编辑器**选项卡上，选择**msDs PrimaryComputer** ，然后选择**编辑**。 此时将出现“多值字符串编辑器”对话框。
10. 右键单击文本框中，选择**粘贴**，选择**添加**，选择**确定**，然后选择**确定**试。

## <a name="step-2-optionally-enable-primary-computers-for-folder-redirection-in-group-policy"></a>步骤 2：（可选） 为组策略中的文件夹重定向启用主计算机

下一步是根据需要配置组策略以启用文件夹重定向的主计算机支持。 这样做可以让用户的文件夹重定向为计算机指定为用户的主计算机上，但不是在任何其他计算机上。 可以在每台计算机或按用户文件夹重定向的控制主计算机。

下面介绍了如何为文件夹重定向启用主计算机：

1. 在组策略管理中，右键单击时执行的初始配置文件夹重定向和/或漫游用户配置文件创建的 GPO (例如，**文件夹重定向设置**或**漫游用户配置文件设置**)，然后选择**编辑**。
2. 若要启用主计算机支持使用基于计算机的组策略，请导航到**计算机配置**。 对于基于用户的组策略，导航到**用户配置**。
    - 基于计算机的组策略应用于 GPO 应用于，影响计算机的所有用户的所有计算机的主计算机处理。
    - 基于用户的组策略以将主计算机处理过程应用于 GPO 应用于，影响到的用户登录的所有计算机的所有用户帐户。
3. 下**计算机配置**或**用户配置**，导航到**策略**，然后**管理模板**，则**系统**，然后**文件夹重定向**。
4. 右键单击**将仅在主计算机上的文件夹重定向**，然后选择**编辑**。
5. 选择**已启用**，然后选择**确定**。

## <a name="step-3-optionally-enable-primary-computers-for-roaming-user-profiles-in-group-policy"></a>步骤 3:（可选） 为组策略中漫游用户配置文件启用主计算机

下一步是根据需要配置组策略以启用漫游用户配置文件的主计算机支持。 这样做可以让用户的配置文件漫游指定为用户的主计算机的计算机上，但不是在任何其他计算机上。

下面介绍了如何为漫游用户配置文件启用主计算机：

1. 如果尚未，启用文件夹重定向的主计算机支持。
    * 这会使文档和其他用户文件超出了用户配置文件，可帮助配置文件保持较小，和登录时间保持快速。
2. 在组策略管理，右键单击你创建的 GPO (例如，**文件夹重定向和漫游用户配置文件设置**)，然后选择**编辑**。
3. 导航到**计算机配置**，然后**策略**，然后**管理模板**，然后**系统**，，然后**用户配置文件**。
4. 右键单击**下载仅，在主计算机上漫游配置文件**，然后选择**编辑**。
5. 选择**已启用**，然后选择**确定**。

## <a name="step-4-enable-the-gpo"></a>步骤 4：启用 GPO

一次完成配置文件夹重定向和漫游用户配置文件，如果尚未启用该 GPO。 这样一来它要应用于受影响的用户和计算机。

下面介绍了如何启用文件夹重定向和/或漫游用户配置文件 Gpo:

1. 打开组策略管理
2. 右键单击 Gpo 创建，并选择**已启用链接**。 菜单项旁边应显示一个复选框。

## <a name="step-5-test-primary-computer-function"></a>步骤 5：测试主计算机函数

若要测试主计算机支持，请登录到主计算机，确认的文件夹和配置文件将被重定向，然后登录到非主计算机并确认的文件夹和配置文件不重定向。

下面介绍了如何测试主要计算机功能：

1. 登录到已为其启用文件夹重定向和/或漫游用户配置文件的用户帐户的指定主计算机。
2. 如果用户帐户登录到计算机之前，打开 Windows PowerShell 会话或以管理员身份的命令提示符窗口、 键入以下命令，然后注销，以确保最新的组策略设置应用于出现提示时客户端计算机：
    ```PowerShell
    Gpupdate /force
    ```
3. 打开文件资源管理器。
4. 重定向的文件夹 （例如，在文档库中的 My Documents 文件夹），右键单击，然后选择**属性**。
5. 选择**位置**选项卡，并确认该路径显示，而不是本地路径指定的文件共享。 若要确认漫游用户配置文件，请打开**Control Panel**，选择**系统和安全**，选择**系统**，选择**高级系统设置**，选择**设置**部分中用户配置文件，然后查找**漫游**中**类型**列。
6. 使用相同用户帐户登录到未指定为该用户的主计算机的计算机。
7. 重复步骤 2-5，而不查看本地路径和一个**本地**配置文件类型。

>[!NOTE]
>如果文件夹已重定向的计算机上，启用主计算机支持之前，文件夹将保留重定向，除非每个文件夹的文件夹重定向策略设置中配置以下设置：**策略被删除时，将文件夹重定向回本地用户配置文件位置**。 同样，将显示以前在特定计算机漫游的配置文件**漫游**中**类型**列; 但是，**状态**列将显示**本地**。

## <a name="more-information"></a>详细信息

- [部署使用脱机文件的文件夹重定向](deploy-folder-redirection.md)
- [部署漫游用户配置文件](deploy-roaming-user-profiles.md)
- [文件夹重定向、 脱机文件和漫游用户配置文件概述](folder-redirection-rup-overview.md)
- [深入探讨稍微深入讨论 Windows 8 的主要计算机](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)