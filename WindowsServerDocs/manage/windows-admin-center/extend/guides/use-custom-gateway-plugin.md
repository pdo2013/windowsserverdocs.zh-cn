---
title: 在工具扩展中使用自定义的网关插件
description: 开发工具扩展 Windows Admin Center SDK (项目 Honolulu)-在您的工具扩展中使用自定义网关插件
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: c9b2e9201d58472286b42a9c89a36423f40d143d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834508"
---
# <a name="use-a-custom-gateway-plugin-in-your-tool-extension"></a>在工具扩展中使用自定义的网关插件

>适用于：Windows Admin Center，Windows Admin Center 预览版

在本文中，我们将使用我们已使用 Windows Admin Center CLI 创建一个新的空工具扩展中的自定义网关插件。

## <a name="prepare-your-environment"></a>准备你的环境 ##

如果尚未安装，请按照中的说明[开发的工具扩展](..\develop-tool.md)准备您的环境，创建了一个新的空工具扩展。

## <a name="add-a-module-to-your-project"></a>将模块添加到你的项目 ##

如果尚未安装，请添加一个新[空模块](add-module.md)到项目中，我们将在下一步中使用。  

## <a name="add-integration-to-custom-gateway-plugin"></a>将集成添加到自定义网关插件 ##

现在我们将我们刚刚创建的新的空模块中使用自定义网关插件。

### <a name="create-pluginservicets"></a>创建 plugin.service.ts

将更改为上面创建的新工具模块的目录 (```\src\app\{!Module-Name}```)，并创建新文件```plugin.service.ts```。

将以下代码添加到刚创建的文件：
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

将引用更改为```Sample Uno```和```Sample%20Uno```到根据你功能的名称。

### <a name="modify-modulets"></a>修改 module.ts

打开```module.ts```之前创建的新模块文件 (即```{!Module-Name}.module.ts```):

添加以下 import 语句：

``` ts
import { HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Http } from '@microsoft/windows-admin-center-sdk/core';
import { PluginService } from './plugin.service';
```

将以下提供程序添加 （之后声明）：

``` ts
  ,
  providers: [
    HttpService,
    PluginService,
    Http
  ]
```

### <a name="modify-componentts"></a>修改 component.ts

打开```component.ts```之前创建的新模块文件 (即```{!Module-Name}.component.ts```):

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

修改构造函数，并修改/添加以下函数：

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

### <a name="modify-componenthtml"></a>修改 component.html ###

打开```component.html```之前创建的新模块文件 (即```{!Module-Name}.component.html```):

将以下内容添加到 html 文件：
``` html
<button (click)="onClick()" >go</button>
{{ responseResult }}
```

## <a name="build-and-side-load-your-extension"></a>生成和端加载您的扩展插件

现在，你就已准备好[生成并端负载](..\develop-tool.md#build-and-side-load-your-extension)你在 Windows Admin Center 中的扩展。
