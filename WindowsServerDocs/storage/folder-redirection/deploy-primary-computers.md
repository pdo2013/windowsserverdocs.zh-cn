---
title: 部署用于文件夹重定向和漫游用户配置文件的主计算机
description: 如何启用主计算机支持，并为具有文件夹重定向和漫游用户配置文件的用户指定主要计算机。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: be2b41cf32e2020422c32415e2d8f4273eb09859
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394437"
---
# <a name="deploy-primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>部署用于文件夹重定向和漫游用户配置文件的主计算机

>适用于：Windows 10、Windows 8、Windows 8.1、Windows Server 2019、Windows Server 2016、Windows Server 2012、Windows Server 2012 R2

本主题介绍如何启用主计算机支持，并为用户指定主要计算机。 这样，你就可以控制哪些计算机使用文件夹重定向和漫游用户配置文件。

> [!IMPORTANT]
> 为漫游用户配置文件启用主计算机支持时，始终为文件夹重定向启用主计算机支持。 这会使文档和其他用户文件不在用户配置文件中，这可帮助配置文件保持较小并使登录时间保持快速。

## <a name="prerequisites"></a>先决条件

## <a name="software-requirements"></a>软件要求

主计算机支持具有以下要求：

- 必须更新 Active Directory 域服务（AD DS）架构以包括 Windows Server 2012 架构添加功能（安装 Windows Server 2012 域控制器会自动更新架构）。 有关更新 AD DS 架构的信息，请参阅[adprep.log 集成](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>)和[运行 al.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>)。
- 客户端计算机必须运行 Windows 10、Windows 8.1、Windows 8、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。

> [!TIP]
> 虽然主计算机支持需要 "文件夹重定向" 和/或 "漫游用户配置文件"，但是，如果你是首次部署这些技术，最好设置主计算机支持，然后再启用用于配置文件夹重定向和漫游用户配置文件。 这会防止在启用主计算机支持之前将用户数据复制到非主计算机。 有关配置信息，请参阅[部署文件夹重定向](deploy-folder-redirection.md)和[部署漫游用户配置文件](deploy-roaming-user-profiles.md)。

## <a name="step-1-designate-primary-computers-for-users"></a>第 1 步：为用户指定主计算机

部署主计算机支持的第一步是为每个用户指定主要计算机。 为此，请使用 Active Directory 管理中心获取相关计算机的可分辨名称，然后设置**PrimaryComputer**属性。

> [!TIP]
> 若要使用 Windows PowerShell 来使用主计算机，请参阅博客文章[深入，深入了解 Windows 8 主计算机](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>)。

下面介绍了如何为用户指定主要计算机：

1. 在装有 Active Directory 管理工具的计算机上打开服务器管理器。
2. 在 "**工具**" 菜单上，选择 " **Active Directory 管理中心**"。 此时将出现 Active Directory 管理中心。
3. 导航到相应域中的 "**计算机**" 容器。
4. 右键单击要指定为主计算机的计算机，然后选择 "**属性**"。
5. 在导航窗格中，选择 "**扩展**"。
6. 选择 "**属性编辑器**" 选项卡，滚动到 " **distinguishedName**"，选择 "**查看**"，右键单击列出的值，选择 "**复制**"，然后选择 **"确定"** ，然后选择 "**取消**"。
7. 导航到相应域中的 "**用户**" 容器，右键单击要向其分配计算机的用户，然后选择 "**属性**"。
8. 在导航窗格中，选择 "**扩展**"。
9. 选择 "**属性编辑器**" 选项卡，选择 " **PrimaryComputer** "，然后选择 "**编辑**"。 此时将出现“多值字符串编辑器”对话框。
10. 右键单击该文本框，依次选择 "**粘贴**"、"**添加**"，选择 **"确定**"，然后再次选择 **"确定"** 。

## <a name="step-2-optionally-enable-primary-computers-for-folder-redirection-in-group-policy"></a>步骤 2：（可选）在组策略中为文件夹重定向启用主计算机

下一步是根据需要配置组策略以便为文件夹重定向启用主计算机支持。 这样做可以在指定为用户的主计算机的计算机上重定向用户的文件夹，但不能在任何其他计算机上重定向。 您可以基于每台计算机或每个用户控制文件夹重定向的主计算机。

以下是为文件夹重定向启用主计算机的方法：

1. 在组策略管理 "中，右键单击在执行" 文件夹重定向 "和/或" 漫游用户配置文件 "（例如"**文件夹重定向设置**"或"**漫游用户配置文件设置**"）的初始配置时创建的 GPO，然后选择 "**编辑**"。
2. 若要使用基于计算机的组策略启用主计算机支持，请导航到 "**计算机配置**"。 对于基于用户的组策略，请导航到 "**用户配置**"。
    - 基于计算机的组策略将主计算机处理应用到 GPO 应用到的所有计算机，从而影响计算机的所有用户。
    - 基于用户的组策略用于将主计算机处理应用到 GPO 应用到的所有用户帐户，从而影响用户登录的所有计算机。
3. 在 "**计算机配置**" 或 "**用户配置**" 下，导航到 "**策略**"，然后依次**管理模板**、"**系统**"、"**文件夹重定向**"
4. 右键单击 "**仅在主计算机上重定向文件夹**"，然后选择 "**编辑**"。
5. 选择 "**已启用**"，然后选择 **"确定"** 。

## <a name="step-3-optionally-enable-primary-computers-for-roaming-user-profiles-in-group-policy"></a>步骤 3:（可选）在组策略中启用漫游用户配置文件的主计算机

下一步是根据需要配置组策略，以便为漫游用户配置文件启用主计算机支持。 这样做可使用户的配置文件漫游到指定为用户的主计算机的计算机上，而不是在任何其他计算机上漫游。

下面介绍了如何为漫游用户配置文件启用主计算机：

1. 为文件夹重定向启用主计算机支持（如果尚未这样做）。<br>这会使文档和其他用户文件不在用户配置文件中，这可帮助配置文件保持较小并使登录时间保持快速。
2. 在组策略管理 "中，右键单击你创建的 GPO （例如，**文件夹重定向和漫游用户配置文件设置**），然后选择"**编辑**"。
3. 依次导航到 "**计算机配置**"、"**策略**"、"**管理模板**"、"**系统**"、"**用户配置文件**"。
4. 右键单击 "**仅在主计算机上下载漫游配置文件"，** 然后选择 "**编辑**"。
5. 选择 "**已启用**"，然后选择 **"确定"** 。

## <a name="step-4-enable-the-gpo"></a>步骤 4：启用 GPO

完成配置文件夹重定向和漫游用户配置文件后，请启用 GPO （如果尚未这样做）。 这样做可以将其应用于受影响的用户和计算机。

下面介绍了如何启用文件夹重定向和/或漫游用户配置文件 Gpo：

1. 打开组策略管理
2. 右键单击你创建的 Gpo，然后选择 "**已启用链接**"。 菜单项旁边应会出现一个复选框。

## <a name="step-5-test-primary-computer-function"></a>步骤 5：测试主计算机函数

若要测试主计算机支持，请登录到一台主计算机，确认已重定向文件夹和配置文件，然后登录到非主计算机并确认未重定向文件夹和配置文件。

下面介绍了如何测试主要计算机功能：

1. 使用已启用文件夹重定向和/或漫游用户配置文件的用户帐户登录到指定的主计算机。
2. 如果用户帐户以前登录过计算机，请以管理员身份打开 Windows PowerShell 会话或命令提示符窗口，键入以下命令，然后在系统提示时注销，以确保将最新组策略设置应用到客户端计算机：

    ```PowerShell
    Gpupdate /force
    ```

3. 打开文件资源管理器。
1. 右键单击重定向的文件夹（例如，文档库中的 "我的文档" 文件夹），然后选择 "**属性**"。
1. 选择 "**位置**" 选项卡，并确认路径显示指定的文件共享，而不是本地路径。 若要确认用户配置文件是否为漫游，请打开 "控制面板"，选择 **"** **系统和安全**"，选择 "**系统**"，选择 "**高级系统设置**"，在 "用户配置文件" 部分中选择 "**设置**"，然后查找在 "**类型**" 列中漫游。
1. 使用同一个用户帐户登录到未指定为用户的主计算机的计算机。
1. 重复步骤 2-5，而不是查找本地路径和**本地**配置文件类型。

> [!NOTE]
> 如果在启用主计算机支持之前已在计算机上重定向文件夹，则这些文件夹将保持重定向，除非在每个文件夹的文件夹重定向策略设置中配置了以下设置：**删除策略后，将该文件夹重定向回本地 userprofile 位置**。 同样，先前在特定计算机上漫游的配置文件将在**类型**列中显示**漫游**;但是，"**状态**" 列将显示 "**本地**"。

## <a name="more-information"></a>详细信息

- [部署文件夹重定向与脱机文件](deploy-folder-redirection.md)
- [部署漫游用户配置文件](deploy-roaming-user-profiles.md)
- [文件夹重定向、脱机文件和漫游用户配置文件概述](folder-redirection-rup-overview.md)
- [更深入地深入 Windows 8 主计算机](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)