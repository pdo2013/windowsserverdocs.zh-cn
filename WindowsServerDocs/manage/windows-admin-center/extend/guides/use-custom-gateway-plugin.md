---
title: 在工具扩展中使用自定义的网关插件
description: 开发工具扩展 Windows 管理中心 SDK （Project Honolulu）-在工具扩展中使用自定义网关插件
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 829cbf6df8cc2738bf4066b36210b860595774ed
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385226"
---
# <a name="use-a-custom-gateway-plugin-in-your-tool-extension"></a>在工具扩展中使用自定义的网关插件

>适用于：Windows Admin Center、Windows Admin Center 预览版

在本文中，我们将使用 Windows 管理中心 CLI 创建的一个新的空工具扩展中的自定义网关插件。

## <a name="prepare-your-environment"></a>准备你的环境 ##

如果尚未这样做，请按照[开发工具扩展](../develop-tool.md)中的说明来准备环境，并创建新的空工具扩展。

## <a name="add-a-module-to-your-project"></a>向项目添加模块 ##

如果你尚未这样做，请向你的项目添加一个新的[空模块](add-module.md)，我们将在下一步中使用该模块。  

## <a name="add-integration-to-custom-gateway-plugin"></a>将集成添加到自定义网关插件 ##

现在，我们将使用刚刚创建的新的空模块中的自定义网关插件。

### <a name="create-pluginservicets"></a>创建插件。

更改到上面创建的新工具模块的目录（```\src\app\{!Module-Name}```），并创建一个新文件 ```plugin.service.ts```。

将以下代码添加到刚创建的文件中：
``` ts
import { Injectable } from '@angular/core';
import { AppContextService, HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Cim, Http, PowerShell, PowerShellSession } from '@microsoft/windows-admin-center-sdk/core';
import { AjaxResponse, Observable } from 'rxjs';

@Injectable()
export class PluginService {
    constructor(private appContextService: AppContextService, private http: Http) {
    }
    
    public getGatewayRestResponse(): Observable<any> {
        let callUrl = this.appContextService.activeConnection.nodeName;

        return this.appContextService.node.get(callUrl, 'features/Sample%20Uno').map(
            (response: any) => {
                return response;
            }
        )
    }
}
```

根据需要将对的引用更改为 ```Sample Uno``` 并将 @no__t 为-1。

[!WARNING]
> 建议使用内置 ```this.appContextService.node``` 来调用在自定义网关插件中定义的任何 API。 这将确保在网关插件中需要凭据，以使其正确处理。

### <a name="modify-modulets"></a>修改 module

打开前面创建的新模块的 @no__t 0 文件（即 ```{!Module-Name}.module.ts```）：

添加以下 import 语句：

``` ts
import { HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Http } from '@microsoft/windows-admin-center-sdk/core';
import { PluginService } from './plugin.service';
```

添加以下提供程序（声明后）：

``` ts
  ,
  providers: [
    HttpService,
    PluginService,
    Http
  ]
```

### <a name="modify-componentts"></a>修改组件。 ts

打开前面创建的新模块的 @no__t 0 文件（即 ```{!Module-Name}.component.ts```）：

添加以下 import 语句：

``` ts
import { ActivatedRouteSnapshot } from '@angular/router';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Subscription } from 'rxjs';
import { Strings } from '../../generated/strings';
import { PluginService } from './plugin.service';
```

添加以下变量：

``` ts
  private serviceSubscription: Subscription;
  private responseResult: string;
```

修改构造函数并修改/添加以下函数：

``` ts
  constructor(private appContextService: AppContextService, private plugin: PluginService) {
    //
  }

  public ngOnInit() {
    this.responseResult = 'click go to do something';
  }

  public onClick() {
    this.serviceSubscription = this.plugin.getGatewayRestResponse().subscribe(
      (response: any) => {
        this.responseResult = 'response: ' + response.message;
      },
      (error) => {
        console.log(error);
      }
    );
  }
```

### <a name="modify-componenthtml"></a>修改组件 .html ###

打开前面创建的新模块的 @no__t 0 文件（即 ```{!Module-Name}.component.html```）：

将以下内容添加到 html 文件：
``` html
<button (click)="onClick()" >go</button>
{{ responseResult }}
```

## <a name="build-and-side-load-your-extension"></a>生成和加载扩展

现在，你已准备好在 Windows 管理中心中[构建和端加载](../develop-tool.md#build-and-side-load-your-extension)扩展。
