---
title: ASP.NET Core MVC のキャッシュ タグ ヘルパー
author: pkellner
description: キャッシュ タグ ヘルパーを使用する方法について説明します。
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 11754d2858d8f02c7eb9baac8feda9b50ddb3d79
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028156"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="af9e6-103">ASP.NET Core MVC のキャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="af9e6-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="af9e6-104">著者: [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="af9e6-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="af9e6-105">キャッシュ タグ ヘルパーは、ASP.NET Core アプリの内容を内部 ASP.NET Core キャッシュ プロバイダーにキャッシュすることによって、アプリのパフォーマンスを大幅に改善する機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="af9e6-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="af9e6-106">Razor ビュー エンジンは、既定の `expires-after` を 20 分に設定します。</span><span class="sxs-lookup"><span data-stu-id="af9e6-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="af9e6-107">次の Razor マークアップは、日付/時刻をキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="af9e6-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="af9e6-108">`CacheTagHelper` を含むページに対する最初の要求で、現在の日付/時刻が表示されます。</span><span class="sxs-lookup"><span data-stu-id="af9e6-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="af9e6-109">キャッシュの有効期限 (既定値は 20 分) が切れるか、メモリ不足により削除されるまで、それ以降の要求ではキャッシュされた値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="af9e6-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="af9e6-110">キャッシュ期間は次の属性で設定できます。</span><span class="sxs-lookup"><span data-stu-id="af9e6-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="af9e6-111">キャッシュ タグ ヘルパーの属性</span><span class="sxs-lookup"><span data-stu-id="af9e6-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="af9e6-112">enabled</span><span class="sxs-lookup"><span data-stu-id="af9e6-112">enabled</span></span>    


| <span data-ttu-id="af9e6-113">属性の型</span><span class="sxs-lookup"><span data-stu-id="af9e6-113">Attribute Type</span></span>    | <span data-ttu-id="af9e6-114">有効な値</span><span class="sxs-lookup"><span data-stu-id="af9e6-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="af9e6-115">boolean</span><span class="sxs-lookup"><span data-stu-id="af9e6-115">boolean</span></span>           | <span data-ttu-id="af9e6-116">"true" (既定値)</span><span class="sxs-lookup"><span data-stu-id="af9e6-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="af9e6-117">"false"</span><span class="sxs-lookup"><span data-stu-id="af9e6-117">"false"</span></span>   |


<span data-ttu-id="af9e6-118">キャッシュ タグ ヘルパーで囲まれた内容をキャッシュするかどうかを決定します。</span><span class="sxs-lookup"><span data-stu-id="af9e6-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="af9e6-119">既定値は、`true` です。</span><span class="sxs-lookup"><span data-stu-id="af9e6-119">The default is `true`.</span></span>  <span data-ttu-id="af9e6-120">`false` に設定すると、そのキャッシュ タグ ヘルパーは表示される出力に対してキャッシュ効果を持ちません。</span><span class="sxs-lookup"><span data-stu-id="af9e6-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="af9e6-121">例:</span><span class="sxs-lookup"><span data-stu-id="af9e6-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="af9e6-122">expires-on</span><span class="sxs-lookup"><span data-stu-id="af9e6-122">expires-on</span></span> 

| <span data-ttu-id="af9e6-123">属性の型</span><span class="sxs-lookup"><span data-stu-id="af9e6-123">Attribute Type</span></span> |           <span data-ttu-id="af9e6-124">値の例</span><span class="sxs-lookup"><span data-stu-id="af9e6-124">Example Value</span></span>            |
|----------------|------------------------------------|
| <span data-ttu-id="af9e6-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="af9e6-125">DateTimeOffset</span></span> | <span data-ttu-id="af9e6-126">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="af9e6-126">"@new DateTime(2025,1,29,17,02,0)"</span></span> |

<span data-ttu-id="af9e6-127">特定の日時で有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="af9e6-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="af9e6-128">次の例では、キャッシュ タグ ヘルパーの内容を 2025 年 1 月 29 日午後 5 時 2 分までキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="af9e6-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="af9e6-129">例:</span><span class="sxs-lookup"><span data-stu-id="af9e6-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="af9e6-130">expires-after</span><span class="sxs-lookup"><span data-stu-id="af9e6-130">expires-after</span></span>

| <span data-ttu-id="af9e6-131">属性の型</span><span class="sxs-lookup"><span data-stu-id="af9e6-131">Attribute Type</span></span> |        <span data-ttu-id="af9e6-132">値の例</span><span class="sxs-lookup"><span data-stu-id="af9e6-132">Example Value</span></span>         |
|----------------|------------------------------|
|    <span data-ttu-id="af9e6-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="af9e6-133">TimeSpan</span></span>    | <span data-ttu-id="af9e6-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="af9e6-134">"@TimeSpan.FromSeconds(120)"</span></span> |

<span data-ttu-id="af9e6-135">最初の要求があってから内容をキャッシュしている時間の長さを設定します。</span><span class="sxs-lookup"><span data-stu-id="af9e6-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="af9e6-136">例:</span><span class="sxs-lookup"><span data-stu-id="af9e6-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="af9e6-137">expires-sliding</span><span class="sxs-lookup"><span data-stu-id="af9e6-137">expires-sliding</span></span>

| <span data-ttu-id="af9e6-138">属性の型</span><span class="sxs-lookup"><span data-stu-id="af9e6-138">Attribute Type</span></span> |        <span data-ttu-id="af9e6-139">値の例</span><span class="sxs-lookup"><span data-stu-id="af9e6-139">Example Value</span></span>        |
|----------------|-----------------------------|
|    <span data-ttu-id="af9e6-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="af9e6-140">TimeSpan</span></span>    | <span data-ttu-id="af9e6-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="af9e6-141">"@TimeSpan.FromSeconds(60)"</span></span> |

<span data-ttu-id="af9e6-142">この属性で設定した時間だけアクセスがない場合、キャッシュ エントリを削除します。</span><span class="sxs-lookup"><span data-stu-id="af9e6-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="af9e6-143">例:</span><span class="sxs-lookup"><span data-stu-id="af9e6-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="af9e6-144">vary-by-header</span><span class="sxs-lookup"><span data-stu-id="af9e6-144">vary-by-header</span></span>

| <span data-ttu-id="af9e6-145">属性の型</span><span class="sxs-lookup"><span data-stu-id="af9e6-145">Attribute Type</span></span>    | <span data-ttu-id="af9e6-146">値の例</span><span class="sxs-lookup"><span data-stu-id="af9e6-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="af9e6-147">String</span><span class="sxs-lookup"><span data-stu-id="af9e6-147">String</span></span>            | <span data-ttu-id="af9e6-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="af9e6-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="af9e6-149">"User-Agent,content-encoding"</span><span class="sxs-lookup"><span data-stu-id="af9e6-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="af9e6-150">変化したときにキャッシュの更新をトリガーする 1 つのヘッダー値またはコンマで区切ったヘッダー値のリストを受け付けます。</span><span class="sxs-lookup"><span data-stu-id="af9e6-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="af9e6-151">次の例では、ヘッダー値 `User-Agent` を監視します。</span><span class="sxs-lookup"><span data-stu-id="af9e6-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="af9e6-152">この例は、Web サーバーに提示されるすべての異なる `User-Agent` の内容をキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="af9e6-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="af9e6-153">例:</span><span class="sxs-lookup"><span data-stu-id="af9e6-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="af9e6-154">vary-by-query</span><span class="sxs-lookup"><span data-stu-id="af9e6-154">vary-by-query</span></span>

| <span data-ttu-id="af9e6-155">属性の型</span><span class="sxs-lookup"><span data-stu-id="af9e6-155">Attribute Type</span></span>    | <span data-ttu-id="af9e6-156">値の例</span><span class="sxs-lookup"><span data-stu-id="af9e6-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="af9e6-157">String</span><span class="sxs-lookup"><span data-stu-id="af9e6-157">String</span></span>            | <span data-ttu-id="af9e6-158">"Make"</span><span class="sxs-lookup"><span data-stu-id="af9e6-158">"Make"</span></span>                |
|                   | <span data-ttu-id="af9e6-159">"Make,Model"</span><span class="sxs-lookup"><span data-stu-id="af9e6-159">"Make,Model"</span></span> |

<span data-ttu-id="af9e6-160">ヘッダーの値が変化したときにキャッシュの更新をトリガーする 1 つのヘッダー値またはコンマで区切ったヘッダー値のリストを受け付けます。</span><span class="sxs-lookup"><span data-stu-id="af9e6-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="af9e6-161">次の例では、`Make` と `Model` の値を調べます。</span><span class="sxs-lookup"><span data-stu-id="af9e6-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="af9e6-162">例:</span><span class="sxs-lookup"><span data-stu-id="af9e6-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="af9e6-163">vary-by-route</span><span class="sxs-lookup"><span data-stu-id="af9e6-163">vary-by-route</span></span>

| <span data-ttu-id="af9e6-164">属性の型</span><span class="sxs-lookup"><span data-stu-id="af9e6-164">Attribute Type</span></span>    | <span data-ttu-id="af9e6-165">値の例</span><span class="sxs-lookup"><span data-stu-id="af9e6-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="af9e6-166">String</span><span class="sxs-lookup"><span data-stu-id="af9e6-166">String</span></span>            | <span data-ttu-id="af9e6-167">"Make"</span><span class="sxs-lookup"><span data-stu-id="af9e6-167">"Make"</span></span>                |
|                   | <span data-ttu-id="af9e6-168">"Make,Model"</span><span class="sxs-lookup"><span data-stu-id="af9e6-168">"Make,Model"</span></span> |

<span data-ttu-id="af9e6-169">ルート データ パラメーターの値が変化したときにキャッシュの更新をトリガーする 1 つのヘッダー値またはコンマで区切ったヘッダー値のリストを受け付けます。</span><span class="sxs-lookup"><span data-stu-id="af9e6-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="af9e6-170">例:</span><span class="sxs-lookup"><span data-stu-id="af9e6-170">Example:</span></span>

<span data-ttu-id="af9e6-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="af9e6-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="af9e6-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="af9e6-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="af9e6-173">vary-by-cookie</span><span class="sxs-lookup"><span data-stu-id="af9e6-173">vary-by-cookie</span></span>

| <span data-ttu-id="af9e6-174">属性の型</span><span class="sxs-lookup"><span data-stu-id="af9e6-174">Attribute Type</span></span>    | <span data-ttu-id="af9e6-175">値の例</span><span class="sxs-lookup"><span data-stu-id="af9e6-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="af9e6-176">String</span><span class="sxs-lookup"><span data-stu-id="af9e6-176">String</span></span>            | <span data-ttu-id="af9e6-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="af9e6-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="af9e6-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="af9e6-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="af9e6-179">ヘッダーの値が変化したときにキャッシュの更新をトリガーする 1 つのヘッダー値またはコンマで区切ったヘッダー値のリストを受け付けます。</span><span class="sxs-lookup"><span data-stu-id="af9e6-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="af9e6-180">次の例では、ASP.NET Core ID に関連付けられている Cookie を調べます。</span><span class="sxs-lookup"><span data-stu-id="af9e6-180">The following example looks at the cookie associated with ASP.NET Core Identity.</span></span> <span data-ttu-id="af9e6-181">ユーザーが認証されると、キャッシュの更新をトリガーする要求 Cookie が設定されます。</span><span class="sxs-lookup"><span data-stu-id="af9e6-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="af9e6-182">例:</span><span class="sxs-lookup"><span data-stu-id="af9e6-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="af9e6-183">vary-by-user</span><span class="sxs-lookup"><span data-stu-id="af9e6-183">vary-by-user</span></span>

| <span data-ttu-id="af9e6-184">属性の型</span><span class="sxs-lookup"><span data-stu-id="af9e6-184">Attribute Type</span></span>    | <span data-ttu-id="af9e6-185">値の例</span><span class="sxs-lookup"><span data-stu-id="af9e6-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="af9e6-186">ブール型</span><span class="sxs-lookup"><span data-stu-id="af9e6-186">Boolean</span></span>             | <span data-ttu-id="af9e6-187">"true"</span><span class="sxs-lookup"><span data-stu-id="af9e6-187">"true"</span></span>                  |
|                     | <span data-ttu-id="af9e6-188">"false" (既定値)</span><span class="sxs-lookup"><span data-stu-id="af9e6-188">"false" (default)</span></span> |

<span data-ttu-id="af9e6-189">ログインしているユーザー (またはコンテキストのプリンシパル) が変化したときにキャッシュをリセットするかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="af9e6-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="af9e6-190">現在のユーザーは要求コンテキストのプリンシパルとも呼ばれ、`@User.Identity.Name` を参照することにより Razor ビューで表示できます。</span><span class="sxs-lookup"><span data-stu-id="af9e6-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="af9e6-191">次の例では、現在ログインしているユーザーを調べています。</span><span class="sxs-lookup"><span data-stu-id="af9e6-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="af9e6-192">例:</span><span class="sxs-lookup"><span data-stu-id="af9e6-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="af9e6-193">この属性を使って、ユーザーがログインしてからログアウトするまでキャッシュの内容を保持します。</span><span class="sxs-lookup"><span data-stu-id="af9e6-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="af9e6-194">`vary-by-user="true"` を使うと、ログイン アクションとログアウト アクションで、認証されたユーザーのキャッシュが無効になります。</span><span class="sxs-lookup"><span data-stu-id="af9e6-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="af9e6-195">ログイン時に新しい一意の Cookie 値が生成されるため、キャッシュは無効になります。</span><span class="sxs-lookup"><span data-stu-id="af9e6-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="af9e6-196">Cookie が存在しない場合、または有効期限が切れた場合は、キャッシュは匿名状態で保持されます。</span><span class="sxs-lookup"><span data-stu-id="af9e6-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="af9e6-197">つまり、ユーザーがログインしていない場合でも、キャッシュは維持されます。</span><span class="sxs-lookup"><span data-stu-id="af9e6-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="af9e6-198">vary-by</span><span class="sxs-lookup"><span data-stu-id="af9e6-198">vary-by</span></span>

| <span data-ttu-id="af9e6-199">属性の型</span><span class="sxs-lookup"><span data-stu-id="af9e6-199">Attribute Type</span></span> | <span data-ttu-id="af9e6-200">値の例</span><span class="sxs-lookup"><span data-stu-id="af9e6-200">Example Values</span></span> |
|----------------|----------------|
|     <span data-ttu-id="af9e6-201">String</span><span class="sxs-lookup"><span data-stu-id="af9e6-201">String</span></span>     |    <span data-ttu-id="af9e6-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="af9e6-202">"@Model"</span></span>    |

<span data-ttu-id="af9e6-203">キャッシュ対象のデータをカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="af9e6-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="af9e6-204">属性の文字列値によって参照されているオブジェクトが変化すると、キャッシュ タグ ヘルパーの内容が更新されます。</span><span class="sxs-lookup"><span data-stu-id="af9e6-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="af9e6-205">多くの場合、モデル値の文字列を連結したものがこの属性に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="af9e6-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="af9e6-206">そのため、連結されている値のいずれかが更新されると、キャッシュは無効になります。</span><span class="sxs-lookup"><span data-stu-id="af9e6-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="af9e6-207">次の例では、ビューをレンダリングするコントローラー メソッドは 2 つのルート パラメーター `myParam1` と `myParam2` の整数値を合計値し、1 つのモデル プロパティとしてそれを返すものとします。</span><span class="sxs-lookup"><span data-stu-id="af9e6-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="af9e6-208">この合計が変化すると、キャッシュ タグ ヘルパーの内容がレンダリングされて、再びキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="af9e6-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="af9e6-209">例:</span><span class="sxs-lookup"><span data-stu-id="af9e6-209">Example:</span></span>

<span data-ttu-id="af9e6-210">アクション:</span><span class="sxs-lookup"><span data-stu-id="af9e6-210">Action:</span></span>

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="af9e6-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="af9e6-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="af9e6-212">priority</span><span class="sxs-lookup"><span data-stu-id="af9e6-212">priority</span></span>

| <span data-ttu-id="af9e6-213">属性の型</span><span class="sxs-lookup"><span data-stu-id="af9e6-213">Attribute Type</span></span>    | <span data-ttu-id="af9e6-214">値の例</span><span class="sxs-lookup"><span data-stu-id="af9e6-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="af9e6-215">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="af9e6-215">CacheItemPriority</span></span>  | <span data-ttu-id="af9e6-216">"High"</span><span class="sxs-lookup"><span data-stu-id="af9e6-216">"High"</span></span>                   |
|                    | <span data-ttu-id="af9e6-217">"Low"</span><span class="sxs-lookup"><span data-stu-id="af9e6-217">"Low"</span></span> |
|                    | <span data-ttu-id="af9e6-218">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="af9e6-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="af9e6-219">"Normal"</span><span class="sxs-lookup"><span data-stu-id="af9e6-219">"Normal"</span></span> |

<span data-ttu-id="af9e6-220">組み込みのキャッシュ プロバイダーにキャッシュ削除ガイダンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="af9e6-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="af9e6-221">Web サーバーは、メモリ不足になると、`Low` キャッシュ エントリを最初に削除します。</span><span class="sxs-lookup"><span data-stu-id="af9e6-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="af9e6-222">例:</span><span class="sxs-lookup"><span data-stu-id="af9e6-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="af9e6-223">`priority` 属性によって、特定レベルのキャッシュ リテンション期間が保証されることはありません。</span><span class="sxs-lookup"><span data-stu-id="af9e6-223">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="af9e6-224">`CacheItemPriority` は提案するだけです。</span><span class="sxs-lookup"><span data-stu-id="af9e6-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="af9e6-225">この属性を `NeverRemove` に設定しても、キャッシュが常に保持されるという保証はありません。</span><span class="sxs-lookup"><span data-stu-id="af9e6-225">Setting this attribute to `NeverRemove` doesn't guarantee that the cache will always be retained.</span></span> <span data-ttu-id="af9e6-226">詳しくは、「[その他の技術情報](#additional-resources)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="af9e6-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="af9e6-227">キャッシュ タグ ヘルパーは、[メモリ キャッシュ サービス](xref:performance/caching/memory)に依存します。</span><span class="sxs-lookup"><span data-stu-id="af9e6-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="af9e6-228">サービスが追加されていない場合、キャッシュ タグ ヘルパーがサービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="af9e6-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="af9e6-229">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="af9e6-229">Additional resources</span></span>

* [<span data-ttu-id="af9e6-230">メモリ内キャッシュ</span><span class="sxs-lookup"><span data-stu-id="af9e6-230">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="af9e6-231">Identity の概要</span><span class="sxs-lookup"><span data-stu-id="af9e6-231">Introduction to Identity</span></span>](xref:security/authentication/identity)
