---
title: 将 Windows Server 2008/2008 R2 特殊化映像上传至 Azure
description: Windows Server 2008 和 2008 R2 的服务即将结束。 了解如何通过在云中托管 Windows Server 来提升和迁移至 Azure。
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: mikeblodge
ms.author: mikeblodge
ms.date: 07/11/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.localizationpriority: high
ms.openlocfilehash: 9bd2b17296872d4b94de5a7468178fbb2ba39709
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868399"
---
# <a name="upload-a-windows-server-20082008-r2-specialized-image-to-azure"></a>将 Windows Server 2008/2008 R2 特殊化映像上传至 Azure 

![介绍 WS08 映像主题的横幅](media/WS08-image-banner-large.png)

现在，可在云中使用 Azure 来运行 Windows Server 2008/2008 R2 虚拟机。 

## <a name="prep-the-windows-server-20082008-r2-specialized-image"></a>准备 Windows Server 2008/2008 R2 专用映像
需要先进行以下更改，然后才能上传映像：

- 如果映像中尚未安装 Windows Server 2008 Service Pack 2 (SP2)，请下载并安装。

- 配置远程桌面 (RDP) 设置。
  1. 转到“控制面板”   >   “系统设置”。   
  2. 在左侧菜单中，选择“远程设置”  。

     ![系统设置的屏幕截图，突出显示“远程设置”。](media/1a_remote_settings.png)

  3. 在“系统属性”中选择“远程”  选项卡。   

     ![“系统属性”中“远程”选项卡的屏幕截图。](media/2c_sysprops.png)

  4. 选择“允许运行任意版本远程桌面的计算机连接(较不安全)”。   
  5. 单击“应用”  和“确定”  。
- 配置 Windows 防火墙设置。   
   1. 在管理员模式下的命令提示符处，输入“wf.msc”以搜索 Windows 防火墙和高级安全设置。    
   2. 按**端口**对结果排序，选择**端口 3389**。   
     ![Windows 防火墙设置的入站规则的屏幕截图。](media/3b_inboundrules.png)   
   3. 为配置文件启用远程桌面 (TCP IN)：**域**、**专用**和**公用**（如上所示）。

- 保存所有设置并关闭该映像。   
- 如果使用 Hyper-V，请确保将子 AVHD 合并到父 VHD 中，以持久保留更改。

当前已知的 Bug 导致已上传映像中的管理员密码在 24 小时内到期。 请执行下列步骤，以避免此问题： 

1. 转到“开始”   >   “运行”
2. 键入 **lusrmgr.msc**
3. 在“本地用户和组”下选择“用户” 
4. 右键单击“管理员”  并选择“属性” 
5. 选择“密码永不过期”  ，然后选择“确定”  
![管理员属性的屏幕截图。](media/6_adminprops.png)

## <a name="uploading-the-image-vhd"></a>上传映像 VHD
可以使用下面的脚本来上传 VHD。 执行此操作之前，需要发布 Azure 帐户的设置文件。 获取 [Azure 文件设置](https://azure.microsoft.com/downloads/)。

下面是该脚本：

```powershell
Get-AzurePublishSettingsFile 

Login-AzureRmAccount
 
      # Import publishsettings
      Import-AzurePublishSettingsFile -PublishSettingsFile <LocationOfPublishingFile>
      $subscriptionId = 'xxxx-xxxx-xxxx-xxxx-xxxxx'
 
      # Set NodeFlight subscription as default subscription
      Select-AzureRmSubscription -SubscriptionId $subscriptionId
      Set-AzureRmContext -SubscriptionId $subscriptionId
      $rgName = "<resourcegroupname>"
    
      $urlOfUploadedImageVhd = "<BlobUrl>/<NameForVHD>.vhd"
      Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd -LocalFilePath "<FilePath>"  
```
## <a name="deploy-the-image-in-azure"></a>在 Azure 中部署映像
在本部分，将在 Azure 中部署映像 VHD。 

> [!IMPORTANT]
> 不要使用 Azure 中预定义的用户映像。

1.  创建一个新的[资源组](https://docs.microsoft.com/rest/api/resources/resourcegroups/createorupdate)。 
2.  在该资源组内创建新的[存储 Blob](https://docs.microsoft.com/rest/api/storageservices/put-blob)。
3.  在存储 Blob 内创建[容器](https://docs.microsoft.com/rest/api/storageservices/create-container)。
4.  从属性中复制 Blob 存储的 URL。
5.  使用上面提供的脚本将映像上传至新的存储 Blob。
6.  为 VHD 创建[磁盘](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image)。   
     a. 转到“磁盘”，单击“添加”  。  
     b. 输入磁盘的名称。 选择要使用的订阅，设置区域，然后选择帐户类型。   
     c. 对于“源类型”，请选择“存储”。 浏览到使用脚本创建的 Blob VHD 的位置。  
     d. 选择操作系统类型 Windows 和大小（默认值：1023）。   
     e. 单击“创建”  。   

7.  转到“已创建的磁盘”，单击“创建 VM”  。   
     a. 将 VM 命名。   
     b. 选择在步骤 5 创建的现有组，在该组中，你已经上传了磁盘。   
     c. 为 VM 选取大小和 SKU 计划。   
     d. 在设置页上选择一个网络接口。 确保该网络接口已指定以下规则：
 
        PORT:3389 Protocol: TCP Action: Allow Priority: 1000 Name: ‘RDP-Rule'.   
     e. 单击“创建”  。




