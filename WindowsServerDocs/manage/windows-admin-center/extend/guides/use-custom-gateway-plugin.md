---
title: 在工具扩展中使用自定义的网关插件
description: 开发工具扩展 Windows Admin Center SDK (Project Honolulu)-在工具扩展中使用自定义的网关插件
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4652616478b7b05bde97db48bf84648984b5a325
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296759"
---
# 在工具扩展中使用自定义的网关插件

>适用于：Windows Admin Center、Windows Admin Center 预览版

在本文中，我们将使用我们已使用 Windows Admin Center CLI 创建新的空工具扩展中的自定义的网关插件。

## 准备你的环境 ##

如果尚未启动，请按照[开发工具扩展](..\develop-tool.md)以准备环境，并创建新的空工具扩展中的说明操作。

## 将模块添加到你的项目 ##

如果你尚未到你的项目，我们将使用下一步中添加一个新的[空模块](add-module.md)。  

## 将集成添加到自定义的网关插件 ##

现在我们将在我们刚刚创建的新的空模块中使用自定义的网关插件。

### 创建 plugin.service.ts

更改为上面创建的新工具模块的目录 (```\src\app\{!Module-Name}```)，并创建一个新的文件```plugin.service.ts```。

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

将引用更改为```Sample Uno```和```Sample%20Uno```根据你功能名称。

[!WARNING]
> 它是建议的内置在```this.appContextService.node```用于调用你的自定义的网关插件中定义的任何 API。 这将确保，如果内它们都可以正确处理你网关插件所需的凭据。

### 修改 module.ts

打开```module.ts```之前创建的新模块文件 (即```{!Module-Name}.module.ts```):

添加以下导入语句：

``` ts
import { HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Http } from '@microsoft/windows-admin-center-sdk/core';
import { PluginService } from './plugin.service';
```

将下列提供程序添加 （之后声明）：

``` ts
  ,
  providers: [
    HttpService,
    PluginService,
    Http
  ]
```

### 修改 component.ts

打开```component.ts```之前创建的新模块文件 (即```{!Module-Name}.component.ts```):

添加以下导入语句：

``` ts
import { ActivatedRouteSnapshot } from '@angular/router';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Subscription } from 'rxjs';
import { Strings } from '../../generated/strings';
import { PluginService } from './plugin.service';
```

将以下变量添加：

``` ts
  private serviceSubscription: Subscription;
  private responseResult: string;
```

修改构造函数和修改/添加以下功能：

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

### 修改 component.html ###

打开```component.html```之前创建的新模块文件 (即```{!Module-Name}.component.html```):

向 html 文件中添加以下内容：
``` html
<button (click)="onClick()" >go</button>
{{ responseResult }}
```

## 构建并旁加载你的扩展

现在你准备[构建并旁加载](..\develop-tool.md#build-and-side-load-your-extension)你在 Windows Admin Center 的扩展。
