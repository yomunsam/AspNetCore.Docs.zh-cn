---
title: ASP.NET Core 中的分布式缓存标记帮助程序
author: pkellner
description: 了解如何使用分布式缓存标记帮助程序。
ms.author: riande
ms.custom: mvc
ms.date: 01/24/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: df1daa68a3e18f7aad4507ce9526d76ff6a2114d
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/04/2020
ms.locfileid: "82773911"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>ASP.NET Core 中的分布式缓存标记帮助程序

作者：[Peter Kellner](https://peterkellner.net)

分布式缓存标记帮助程序将其内容缓存到分布式缓存源，从而大幅提高 ASP.NET Core 应用的性能。

有关标签帮助程序的概述，请参阅 <xref:mvc/views/tag-helpers/intro>。

分布式缓存标记帮助程序与缓存标记帮助程序继承自相同的基类。 分布式标记帮助程序可以使用所有[缓存标记帮助程序](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)属性。

分布式缓存标记帮助程序使用[构造函数注入](xref:fundamentals/dependency-injection#constructor-injection-behavior)。 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 接口将传递到分布式缓存标记帮助程序的构造函数中。 如果在 `Startup.ConfigureServices`(Startup.cs) 中未创建 `IDistributedCache` 的具体实现，则分布式缓存标记帮助程序会使用与[缓存标记帮助程序](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)用于存储缓存数据相同的内存中提供程序。**

## <a name="distributed-cache-tag-helper-attributes"></a>分布式缓存标记帮助程序属性

### <a name="attributes-shared-with-the-cache-tag-helper"></a>与缓存标记帮助程序共享的属性

* `enabled`
* `expires-on`
* `expires-after`
* `expires-sliding`
* `vary-by-header`
* `vary-by-query`
* `vary-by-route`
* `vary-by-cookie`
* `vary-by-user`
* `vary-by priority`

分布式缓存标记帮助程序继承自与缓存标记帮助程序相同的类。 有关这些属性的说明，请参阅[缓存标记帮助程序](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)。

### <a name="name"></a>name

| 属性类型 | 示例                               |
| -------------- | ------------------------------------- |
| String         | `my-distributed-cache-unique-key-101` |

需要 `name`。 `name` 属性用作每个存储的缓存实例的键。 与缓存标记帮助程序不同的是，基于Razor页面名称和Razor页面中的位置将缓存键分配给每个实例，分布式缓存标记帮助程序只将其密钥基于`name`属性。

示例：

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>分布式缓存标记帮助程序 IDistributedCache 实现

ASP.NET Core 中内置了 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 的两个实现。 一个是基于 SQL Server，另一个是基于 Redis。 还提供第三方实现，如 [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)。 有关这些实现的详细信息，请参阅 <xref:performance/caching/distributed>。 这两种实现都需要在 `Startup` 中设置 `IDistributedCache` 的实例。

没有专门与使用 `IDistributedCache` 的任何特定实现相关的标记属性。

## <a name="additional-resources"></a>其他资源

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
