---
title: 管理虚拟桌面
description: 了解如何在 MultiPoint Services 中管理虚拟桌面（VDI）
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fa9ac0ed-47cb-4811-91ff-4fcb62d7858b
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 45bb3e98779bc27913c7e675a9c9db7e575d9d72
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389587"
---
# <a name="manage-virtual-desktops"></a>管理虚拟桌面
使用单机 VDI，你可以将每个*本地*MultiPoint services 工作站配置为连接到与工作站位于同一 MultiPoint 服务计算机上的 hyper-v 虚拟机（VM）中运行的 Windows 10 企业来宾操作系统。 可以使用无法安装在 Windows Server 版本中的应用程序自定义这些虚拟桌面工作站。  
  
## <a name="enable-the-virtual-desktop-feature"></a>启用虚拟桌面功能  
  
1.  打开 MultiPoint Manager，然后单击“虚拟桌面”选项卡。  
  
2.  在“VDI 任务”下，单击“创建虚拟桌面”，然后浏览你的 Windows 10 企业版 .iso 或 VHD。  
  
将重新启动系统，可能需要几分钟时间。  
  
## <a name="create-a-virtual-desktop-template"></a>创建虚拟桌面模板  
  
1.  打开 MultiPoint Manager，然后单击“虚拟桌面”选项卡。  
  
2.  在“VDI 任务”下，单击“创建虚拟桌面”，然后浏览到你的 Windows 10 企业版 .iso 或 VHD。  
  
    如果使用的是 DVD，程序将自动找到 Windows 10 企业版 .wim 文件。 否则，请单击“浏览”，然后导航到 Windows 10 企业版 .iso 或 VHD。  
  
    根据需要修改前缀。 默认为主计算机名。  
  
    > [!NOTE]  
    > 前缀用于为模板和虚拟桌面工作站命名。 模板被命名为前缀 \-t。 虚拟桌面工作站被命名为前缀 \-n，其中，n 是工作站 ID。  
  
4.  输入本地管理员账户的用户名和密码，并且使用该账户登录到所有从模板创建的虚拟工作站桌面，然后单击“确定”。  
  
    模板创建过程需要几分钟时间才能完成。  
      
    接下来，了解如何自定义虚拟桌面模板。  
      
    > [!NOTE]  
    > 如果 MultiPoint Server 已加入域，则对话框将填充一个附加域，以指示是否应将通过模板创建的虚拟机加入到域。   
  
## <a name="import-a-virtual-desktop-template"></a>导入虚拟桌面模板  
如果你在另一台 MultiPoint Server 上创建了一个虚拟桌面模板，则可以通过以下步骤导入该模板。  

1.  打开 MultiPoint Manager，然后单击“虚拟桌面”选项卡。  
  
2.  在“VDI 任务”下，单击“导入虚拟桌面模板”。  
  
3.  定位到该模板，并找到已导入的模板的路径和前缀。  
  
## <a name="customize-the-virtual-desktop-template"></a>自定义虚拟桌面模板  
创建虚拟桌面模板后，你可以使用应用程序和软件更新对其进行自定义，并配置系统设置。   

1. 打开 MultiPoint Manager，然后单击“虚拟桌面”选项卡。  
2. 选择虚拟桌面模板，然后单击“自定义虚拟桌面模板”。  
将在单独的窗口中打开模板并显示其他说明，其中突出显示了自定义虚拟模板的最重要步骤。 请仔细阅读这些说明。  
  
## <a name="create-virtual-desktop-stations"></a>创建虚拟桌面工作站  
  
1.  在工作站模式下打开 MultiPoint Manager，然后单击“虚拟桌面”选项卡。  
  
    > [!NOTE]  
    > 如果 MultiPoint Services 系统未以工作站模式运行，则重启系统，然后完成此过程。  
  
2.  选择左侧 @ no__t-0hand 窗格中的 "虚拟桌面" 模板。 其名称为 <prefix –t>。  
  
3.  在“模板任务”下，单击“创建虚拟桌面工作站”，然后单击“确定”。  
  
    虚拟桌面工作站创建过程需要几分钟时间才能完成。  
  
    > [!NOTE]  
    > 如果任何本地工作站当前已连接到会话 @ no__t-0based 虚拟桌面，则必须注销这些工作站，才能将其连接到新创建的虚拟桌面工作站之一。  
  
### <a name="validate-the-newly-created-customized-virtual-station-desktops"></a>验证新建的自定义虚拟工作站桌面  
  
你可以使用本地管理员帐户或域帐户登录到一个或多个虚拟桌面工作站，验证你的自定义虚拟工作站桌面，然后验证新的 VM @ no__t 0based 虚拟桌面是否正常工作。  
  
## <a name="disable-virtual-desktops"></a>禁用虚拟桌面  
  
禁用虚拟桌面后，将关闭 Hyper-V 功能。 所有用户都将被注销，并且将重启系统。 重启系统后，所有虚拟工作站都将被分配给 MultiPoint 本地会话。  

1. 在工作站模式下打开 MultiPoint Manager，然后单击“虚拟桌面”选项卡。  
  
2. 在“VDI 任务”下，单击“禁用虚拟桌面”。 