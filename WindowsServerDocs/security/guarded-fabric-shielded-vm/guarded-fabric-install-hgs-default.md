---
title: 在新林中安装 HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7f04d9964f1a19e79335e50a1882f0326f15ddc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860028"
---
# <a name="install-hgs-in-a-new-forest"></a>在新林中安装 HGS 

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

## <a name="add-the-hgs-server-role"></a>添加 HGS 服务器角色

在提升的 PowerShell 会话以添加 HGS 服务器角色并安装 HGS 中运行以下命令。

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

## <a name="install-hgs"></a>安装 HGS 

[!INCLUDE [Install HGS by default](../../../includes/install-hgs-default.md)] 

## <a name="next-steps"></a>后续步骤

- 若要设置基于 TPM 的认证的下一个步骤，请参阅[初始化 HGS 群集在新的专用林 （默认值） 中使用 TPM 模式](guarded-fabric-initialize-hgs-tpm-mode-default.md)。
- 有关设置主机密钥证明的下一个步骤，请参阅[初始化 HGS 群集在新的专用林 （默认值） 中使用密钥模式](guarded-fabric-initialize-hgs-key-mode-default.md)。
- 在接下来的步骤来设置基于管理员的证明 （不推荐在 Windows Server 2019），请参阅[初始化 HGS 群集在新的专用林 （默认值） 中使用 AD 模式](guarded-fabric-initialize-hgs-ad-mode-default.md)。

## <a name="next-step"></a>下一步

>[!div class="nextstepaction"]
[初始化 HGS](guarded-fabric-initialize-hgs.md)


