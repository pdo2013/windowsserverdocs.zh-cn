---
title: 预安排群集计算机对象在 Active Directory 域服务
description: 如何预安排群集计算机对象在 Active Directory 域服务中。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/25/2018
ms.localizationpriority: medium
ms.openlocfilehash: 111969b074b33764dbbf72bfb24ad606f8314e41
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869718"
---
# <a name="prestage-cluster-computer-objects-in-active-directory-domain-services"></a>预安排群集计算机对象在 Active Directory 域服务

>适用于：Windows Server 2012 R2、 Windows Server 2012 中，Windows Server 2016

本主题介绍如何在 Active Directory 域服务 (AD DS) 中预安排群集计算机对象。 当用户或组没有在 AD DS 中创建计算机对象的权限时，你可以使用此过程，允许用户或组创建故障转移群集。

使用创建群集向导或 Windows PowerShell 创建故障转移群集时，必须指定群集的名称。 如果你在创建群集时具有足够的权限，则群集创建过程会自动在 AD DS 中创建一个与群集名称相匹配的计算机对象。 此对象称为“*群集名称对象*”或 CNO。 当你配置使用客户端访问点的群集角色时，可以通过 CNO 自动创建虚拟计算机对象 (VCO)。 例如，如果你创建高可用性文件服务器，它具有名为“ *FileServer1*”的客户端访问点，则 CNO 将在 AD DS 中创建相应的 VCO。

>[!NOTE]
>在 Windows Server 2012 R2 中，没有用于创建与 Active Directory 分离的群集，其中在 AD DS 中创建任何 CNO 或 Vco 的选项。 此选项适用于特定类型的群集部署。 有关详细信息，请参阅[部署与 Active Directory 分离的群集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v%3dws.11)>)。

若要自动创建 CNO，创建故障转移群集的用户必须具备“ **创建计算机对象** ”权限，这一权限针对组织单位 (OU) 或将形成群集的服务器所在的容器。 若要允许用户或组在不具备此权限的情况下创建群集，可以让具有 AD DS 中的适当权限的用户（通常为域管理员）在 AD DS 中预安排 CNO。 这也让域管理员能够更多地控制用于群集的命名约定，以及在其中创建群集对象的 OU。

## <a name="step-1-prestage-the-cno-in-ad-ds"></a>第 1 步：预安排 CNO 中 AD DS

开始之前，请确保你掌握了以下信息：

- 你想要为群集分配名称
- 用户帐户或组你想要授予权限来创建群集的名称

我们建议你为群集对象创建 OU，这是一种最佳做法。 如果你要使用的 OU 已经存在，则 **Account Operators** 组中的成员身份是完成此步骤的最低要求。 如果你需要为群集对象创建 OU，则 **Domain Admins** 组中的成员身份或等效身份是完成此步骤的最低要求。

>[!NOTE]
>如果你在默认的计算机容器中创建 CNO，而不是在 OU 中创建，则无需完成本主题的步骤 3。 在这种情况下，群集管理员可以创建最多 10 个 VCO，而无需任何附加配置。

### <a name="prestage-the-cno-in-ad-ds"></a>预安排 CNO 中 AD DS

1. 在从远程服务器管理工具安装了 AD DS 工具的计算机上，或在域控制器上，打开“Active Directory 用户和计算机” 。 若要执行此操作的服务器上，启动服务器管理器，然后在**工具**菜单中，选择**Active Directory 用户和计算机**。
2. 若要创建的 OU 为群集计算机对象，右键单击域名或现有 OU，指向**新建**，然后选择**组织单位**。 在中**名称**框中，输入 OU 的名称，然后选择**确定**。
3. 在控制台树中，右键单击你想要创建了 CNO 的 OU 指向**新建**，然后选择**计算机**。
4. 在中**计算机名**框中，输入名称，用于故障转移群集，然后选择**确定**。

  >[!NOTE]
  >创建群集的用户将在创建群集向导中的“用于管理群集的访问点”  页面上指定此群集名称，或将此群集名称用作 *New-Cluster* Windows PowerShell cmdlet 的 **–Name** 参数值。

5. 作为最佳实践中，右键单击刚创建的计算机帐户，选择**属性**，然后选择**对象**选项卡。上**对象**选项卡上，选择**防止对象被意外删除**复选框，并选择**确定**。
6. 右键单击您刚的计算机帐户创建，然后依次**禁用帐户**。 选择**是**进行确认，然后选择**确定**。

  >[!NOTE]
  >必须禁用帐户，这样在群集创建期间，群集创建过程就能确认帐户当前未被域中的现有计算机或群集使用。

![示例群集 OU 中的已禁用 CNO](media/prestage-cluster-adds/disabled-cno-in-the-example-clusters-ou.png)

**图 1。示例群集 OU 中的已禁用的 CNO**

## <a name="step-2-grant-the-user-permissions-to-create-the-cluster"></a>步骤 2：授予创建群集的用户权限

你必须配置权限，使得将用于创建故障转移群集的用户帐户具有对 CNO 的“完全控制”权限。

**Account Operators** 组中的成员身份是完成此步骤的最低要求。

下面介绍了如何授予用户权限来创建群集：

1. 在“Active Directory 用户和计算机”中，在“查看”菜单上，确认“高级功能”处于选中状态。
2. 找到并右键单击 CNO，然后选择**属性**。
3. 上**安全**选项卡上，选择**添加**。
4. 在中**选择用户、 计算机或组**对话框框中，指定用户帐户或组，你想要向其授予权限，然后选择**确定**。
5. 选中刚才添加的用户帐户或组，然后选中“完全控制” 旁边的“允许”  复选框。
  
  ![向将要创建群集的用户或组授予“完全控制”权限](media/prestage-cluster-adds/granting-full-control-to-the-user-create-the-cluster.png)
  
  **图 2。授予完全控制权限的用户或组，将创建群集**
6. 选择“确定”。

完成此步骤后，你向其授予权限的用户将能够创建故障转移群集。 但是，如果 CNO 位于 OU 中，则在完成步骤 3 之前，用户将无法创建需要客户端访问点的群集角色。

>[!NOTE]
>如果 CNO 位于默认的计算机容器中，则群集管理员可以创建最多 10 个 VCO，而无需任何附加配置。 要添加 10 个以上的 VCO，你必须明确授予对计算机容器的 CNO 的“创建计算机对象”权限。

## <a name="step-3-grant-the-cno-permissions-to-the-ou-or-prestage-vcos-for-clustered-roles"></a>步骤 3:CNO 权限授予 OU 或的群集角色预安排 Vco

当你创建具有客户端访问点的群集角色时，群集会在 CNO 所在的同一个 OU 中创建 VCO。 若要自动执行此操作，CNO 必须具备在 OU 中创建计算机对象的权限。

如果你已在 AD DS 中预安排 CNO，则可以通过执行以下某一项操作来创建 VCO：

- 选项 1：[CNO 权限授予 OU](#grant-the-cno-permissions-to-the-ou)。 如果使用此选项，则群集可以自动在 AD DS 中创建 VCO。 因此，故障转移群集的管理员可以创建群集角色，而无需请求你在 AD DS 中预安排 VCO。

>[!NOTE]
>**Domain Admins** 组中的成员身份或等效身份是完成此选项步骤所需的最低要求。

- 选项 2：[预安排群集角色 VCO](#prestage-a-vco-for-the-clustered-role)。 如果由于组织的要求，必须为群集角色预安排帐户，请使用此选项。 例如，你可能需要控制命名约定，或者控制创建哪些群集角色。

>[!NOTE]
>**Account Operators** 组中的成员身份是完成此选项步骤所需的最低要求。

### <a name="grant-the-cno-permissions-to-the-ou"></a>CNO 权限授予 OU

1. 在“Active Directory 用户和计算机”中，在“查看”菜单上，确认“高级功能”处于选中状态。
2. 右键单击你在其中创建在 CNO 的 OU[步骤 1:预安排 CNO 中 AD DS](#step-1:-prestage-the-CNO-in-ad-ds)，然后选择**属性**。
3. 上**安全**选项卡上，选择**高级**。
4. 在中**高级安全设置**对话框中，选择**添加**。
5. 下一步**主体**，选择**选择主体**。
6. 在中**选择用户、 计算机、 服务帐户或组**对话框中，选择**对象类型**，选择**计算机**复选框，并选择**确定**.
7. 下**输入要选择的对象名称**，输入 CNO 的名称，选择**检查名称**，然后选择**确定**。 在警告消息，提示您是否要添加已禁用的对象的响应，选择**确定**。
8. 在“权限条目”对话框中，确保“类型”列表设置为“允许”, 并且“应用于”列表设置为“这个对象及全部后代”。
9. 在“权限”下，选中“创建计算机对象”复选框。

  ![向 CNO 授予创建计算机对象的权限](media/prestage-cluster-adds/granting-create-computer-objects-permission-to-the-cno.png)

  **图 3。向 CNO 授予创建计算机对象的权限**
10. 选择**确定**直到返回到 Active Directory 用户和计算机管理单元中。

故障转移群集管理员现在能够创建具有客户端访问点的群集角色，并让资源联机。

### <a name="prestage-a-vco-for-a-clustered-role"></a>预安排 VCO 的群集角色

1. 开始之前，请确保你知道群集的名称，以及群集角色将要使用的名称。
2. 在“Active Directory 用户和计算机”中，在“查看”菜单上，确认“高级功能”处于选中状态。
3. 在 Active Directory 用户和计算机中，右键单击 OU CNO 所在的群集，指向**新建**，然后选择**计算机**。
4. 在中**计算机名**框中，输入名称，将使用为群集角色，然后选择**确定**。
5. 作为最佳实践中，右键单击刚创建的计算机帐户，选择**属性**，然后选择**对象**选项卡。上**对象**选项卡上，选择**防止对象被意外删除**复选框，并选择**确定**。
6. 右键单击您刚的计算机帐户创建，然后依次**属性**。
7. 上**安全**选项卡上，选择**添加**。
8. 在中**选择用户、 计算机、 服务帐户或组**对话框中，选择**对象类型**，选择**计算机**复选框，并选择**确定**.
9. 下**输入要选择的对象名称**，输入 CNO 的名称，选择**检查名称**，然后选择**确定**。 如果收到警告消息，提示您是否要添加已禁用的对象，请选择**确定**。
10. 确保 CNO 已选中，然后选中“完全控制”旁边的“允许”复选框。
11. 选择“确定”。

故障转移群集管理员现在能够创建其客户端访问点与预安排 VCO 名称相匹配的群集角色，并让资源联机。

## <a name="more-information"></a>详细信息

- [故障转移群集](failover-clustering.md)