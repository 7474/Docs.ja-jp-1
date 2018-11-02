---
title: ASP.NET Core のアンカー タグ ヘルパー
author: pkellner
description: ASP.NET Core アンカー タグ ヘルパーの属性と、HTML アンカー タグの動作拡張時の各属性の役割を示します。
ms.author: scaddie
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 01c5833210b73dafb763602d363afcf9e7bc0122
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206277"
---
# <a name="anchor-tag-helper-in-aspnet-core"></a><span data-ttu-id="73b02-103">ASP.NET Core のアンカー タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="73b02-103">Anchor Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="73b02-104">作成者: [Peter Kellner](http://peterkellner.net)、[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="73b02-104">By [Peter Kellner](http://peterkellner.net) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="73b02-105">[アンカー タグ ヘルパー](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper)は、新しい属性を追加することで標準の HTML アンカー (`<a ... ></a>`) タグを拡張します。</span><span class="sxs-lookup"><span data-stu-id="73b02-105">The [Anchor Tag Helper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) enhances the standard HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="73b02-106">規則では、属性名の前に `asp-` が付きます。</span><span class="sxs-lookup"><span data-stu-id="73b02-106">By convention, the attribute names are prefixed with `asp-`.</span></span> <span data-ttu-id="73b02-107">レンダリングされたアンカー要素の `href` 属性値は、`asp-` 属性の値によって決まります。</span><span class="sxs-lookup"><span data-stu-id="73b02-107">The rendered anchor element's `href` attribute value is determined by the values of the `asp-` attributes.</span></span>

<span data-ttu-id="73b02-108">タグ ヘルパーの概要については、「<xref:mvc/views/tag-helpers/intro>」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="73b02-108">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="73b02-109">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="73b02-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="73b02-110">*SpeakerController* は、このドキュメント全体にわたってサンプルとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="73b02-110">*SpeakerController* is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

<span data-ttu-id="73b02-111">`asp-` 属性のインベントリが続きます。</span><span class="sxs-lookup"><span data-stu-id="73b02-111">An inventory of the `asp-` attributes follows.</span></span>

## <a name="asp-controller"></a><span data-ttu-id="73b02-112">asp-controller</span><span class="sxs-lookup"><span data-stu-id="73b02-112">asp-controller</span></span>

<span data-ttu-id="73b02-113">[asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) 属性は、URL の生成に使用するコント ローラーを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="73b02-113">The [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) attribute assigns the controller used for generating the URL.</span></span> <span data-ttu-id="73b02-114">次のマークアップでは、すべてのスピーカーが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="73b02-114">The following markup lists all speakers:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

<span data-ttu-id="73b02-115">生成される HTML:</span><span class="sxs-lookup"><span data-stu-id="73b02-115">The generated HTML:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="73b02-116">`asp-controller` 属性が指定されていて、`asp-action` が指定されていない場合は、既定値 `asp-action` が現在実行中のビューに関連付けられたコント ローラー アクションです。</span><span class="sxs-lookup"><span data-stu-id="73b02-116">If the `asp-controller` attribute is specified and `asp-action` isn't, the default `asp-action` value is the controller action associated with the currently executing view.</span></span> <span data-ttu-id="73b02-117">`asp-action` が前のマークアップから省略されていて、アンカー タグ ヘルパーが *HomeController* の *Index* ビュー (*/Home*) で使用されている場合、次の HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="73b02-117">If `asp-action` is omitted from the preceding markup, and the Anchor Tag Helper is used in *HomeController*'s *Index* view (*/Home*), the generated HTML is:</span></span>

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a><span data-ttu-id="73b02-118">asp-action</span><span class="sxs-lookup"><span data-stu-id="73b02-118">asp-action</span></span>

<span data-ttu-id="73b02-119">[asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) 属性値は、生成された `href` 属性に含まれるコントローラー アクション名を表します。</span><span class="sxs-lookup"><span data-stu-id="73b02-119">The [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) attribute value represents the controller action name included in the generated `href` attribute.</span></span> <span data-ttu-id="73b02-120">次のマークアップは、生成された `href` 属性値をスピーカー評価ページに設定します。</span><span class="sxs-lookup"><span data-stu-id="73b02-120">The following markup sets the generated `href` attribute value to the speaker evaluations page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

<span data-ttu-id="73b02-121">生成される HTML:</span><span class="sxs-lookup"><span data-stu-id="73b02-121">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="73b02-122">`asp-controller` 属性が指定されていない場合、現在のビューを実行しているビューを呼び出す既定のコントローラーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="73b02-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view is used.</span></span>

<span data-ttu-id="73b02-123">属性値 `asp-action` が `Index` で、URL にアクションが何も追加されていない場合、既定の `Index` の操作が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="73b02-123">If the `asp-action` attribute value is `Index`, then no action is appended to the URL, leading to the invocation of the default `Index` action.</span></span> <span data-ttu-id="73b02-124">指定された (または既定の) 操作が、`asp-controller` で参照されるコントローラーに存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="73b02-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

## <a name="asp-route-value"></a><span data-ttu-id="73b02-125">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="73b02-125">asp-route-{value}</span></span>

<span data-ttu-id="73b02-126">[asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) 属性は、ワイルドカード ルート プレフィックスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="73b02-126">The [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute enables a wildcard route prefix.</span></span> <span data-ttu-id="73b02-127">`{value}` プレースホルダーに入っている値はすべて、潜在的なルートのパラメーターとして解釈されます。</span><span class="sxs-lookup"><span data-stu-id="73b02-127">Any value occupying the `{value}` placeholder is interpreted as a potential route parameter.</span></span> <span data-ttu-id="73b02-128">既定のルートが見つからない場合は、このルート プレフィックスが生成された `href` に要求パラメーターおよび値として追加されます。</span><span class="sxs-lookup"><span data-stu-id="73b02-128">If a default route isn't found, this route prefix is appended to the generated `href` attribute as a request parameter and value.</span></span> <span data-ttu-id="73b02-129">見つかった場合は、そのルートがルート テンプレートで置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="73b02-129">Otherwise, it's substituted in the route template.</span></span>

<span data-ttu-id="73b02-130">次のコントローラー アクションがあるとします。</span><span class="sxs-lookup"><span data-stu-id="73b02-130">Consider the following controller action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

<span data-ttu-id="73b02-131">既定のルート テンプレートは *Startup.Configure* に定義されています。</span><span class="sxs-lookup"><span data-stu-id="73b02-131">With a default route template defined in *Startup.Configure*:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

<span data-ttu-id="73b02-132">MVC ビューは、次のように、アクションによって提供されるモデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="73b02-132">The MVC view uses the model, provided by the action, as follows:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

<span data-ttu-id="73b02-133">既定のルートの `{id?}` プレースホルダーが一致しました。</span><span class="sxs-lookup"><span data-stu-id="73b02-133">The default route's `{id?}` placeholder was matched.</span></span> <span data-ttu-id="73b02-134">生成される HTML:</span><span class="sxs-lookup"><span data-stu-id="73b02-134">The generated HTML:</span></span>

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

<span data-ttu-id="73b02-135">次の MVC ビューのように、ルート プレフィックスが一致するルーティング テンプレートに含まれないものとします。</span><span class="sxs-lookup"><span data-stu-id="73b02-135">Assume the route prefix isn't part of the matching routing template, as with the following MVC view:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker" 
       asp-action="Detail" 
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

<span data-ttu-id="73b02-136">`speakerid` が一致するルートに見つからなかったため、次の HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="73b02-136">The following HTML is generated because `speakerid` wasn't found in the matching route:</span></span>

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

<span data-ttu-id="73b02-137">`asp-controller` または `asp-action` のどちらかが指定されていない場合は、`asp-route` 属性の場合と同じ既定の処理に従います。</span><span class="sxs-lookup"><span data-stu-id="73b02-137">If either `asp-controller` or `asp-action` aren't specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

## <a name="asp-route"></a><span data-ttu-id="73b02-138">asp-route</span><span class="sxs-lookup"><span data-stu-id="73b02-138">asp-route</span></span>

<span data-ttu-id="73b02-139">[asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) 属性は、名前付きのルートに直接 URL リンクを作成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="73b02-139">The [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) attribute is used for creating a URL linking directly to a named route.</span></span> <span data-ttu-id="73b02-140">[ルーティング属性](xref:mvc/controllers/routing#attribute-routing)を使用すると、`SpeakerController` に示されているようにルートに名前を付け、その `Evaluations` アクションで使用することができます。</span><span class="sxs-lookup"><span data-stu-id="73b02-140">Using [routing attributes](xref:mvc/controllers/routing#attribute-routing), a route can be named as shown in the `SpeakerController` and used in its `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

<span data-ttu-id="73b02-141">次のマークアップでは、`asp-route` 属性が名前付きのルートを参照します。</span><span class="sxs-lookup"><span data-stu-id="73b02-141">In the following markup, the `asp-route` attribute references the named route:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

<span data-ttu-id="73b02-142">アンカー タグ ヘルパーは URL */Speaker/Evaluations* を使用してそのコントローラー アクションに直接ルートを生成します。</span><span class="sxs-lookup"><span data-stu-id="73b02-142">The Anchor Tag Helper generates a route directly to that controller action using the URL */Speaker/Evaluations*.</span></span> <span data-ttu-id="73b02-143">生成される HTML:</span><span class="sxs-lookup"><span data-stu-id="73b02-143">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="73b02-144">`asp-controller` または `asp-action` が `asp-route` の他に指定されていると、想定と異なるルートが生成される場合があります。</span><span class="sxs-lookup"><span data-stu-id="73b02-144">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="73b02-145">ルートの競合を防ぐには、`asp-route` を属性 `asp-controller` および `asp-action` とともに使用してはいけません。</span><span class="sxs-lookup"><span data-stu-id="73b02-145">To avoid a route conflict, `asp-route` shouldn't be used with the `asp-controller` and `asp-action` attributes.</span></span>

## <a name="asp-all-route-data"></a><span data-ttu-id="73b02-146">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="73b02-146">asp-all-route-data</span></span>

<span data-ttu-id="73b02-147">[asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) 属性は、キー/値ペアのディクショナリの作成をサポートします。</span><span class="sxs-lookup"><span data-stu-id="73b02-147">The [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute supports the creation of a dictionary of key-value pairs.</span></span> <span data-ttu-id="73b02-148">キーはパラメーターの名前で、値はパラメーターの値です。</span><span class="sxs-lookup"><span data-stu-id="73b02-148">The key is the parameter name, and the value is the parameter value.</span></span>

<span data-ttu-id="73b02-149">次の例では、ディクショナリが初期化され、Razor ビューに渡されます。</span><span class="sxs-lookup"><span data-stu-id="73b02-149">In the following example, a dictionary is initialized and passed to a Razor view.</span></span> <span data-ttu-id="73b02-150">データがモデルによって渡される場合もあります。</span><span class="sxs-lookup"><span data-stu-id="73b02-150">Alternatively, the data could be passed in with your model.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

<span data-ttu-id="73b02-151">上のコードを実行すると、次の HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="73b02-151">The preceding code generates the following HTML:</span></span>

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

<span data-ttu-id="73b02-152">`asp-all-route-data` ディクショナリは、オーバー ロードされた `Evaluations` アクションの要件を満たすクエリ文字列を生成するためにフラット化されます。</span><span class="sxs-lookup"><span data-stu-id="73b02-152">The `asp-all-route-data` dictionary is flattened to produce a querystring meeting the requirements of the overloaded `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

<span data-ttu-id="73b02-153">ディクショナリ内のいずれかのキーがルート パラメーターと一致する場合は、その値がルートで適宜置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="73b02-153">If any keys in the dictionary match route parameters, those values are substituted in the route as appropriate.</span></span> <span data-ttu-id="73b02-154">その他の一致しない値は要求パラメーターとして生成されます。</span><span class="sxs-lookup"><span data-stu-id="73b02-154">The other non-matching values are generated as request parameters.</span></span>

## <a name="asp-fragment"></a><span data-ttu-id="73b02-155">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="73b02-155">asp-fragment</span></span>

<span data-ttu-id="73b02-156">[asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) 属性では URL に追加される URL フラグメントが定義されます。</span><span class="sxs-lookup"><span data-stu-id="73b02-156">The [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) attribute defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="73b02-157">アンカー タグ ヘルパーにより、ハッシュ文字 (#) が追加されます。</span><span class="sxs-lookup"><span data-stu-id="73b02-157">The Anchor Tag Helper adds the hash character (#).</span></span> <span data-ttu-id="73b02-158">次のマークアップがあるとします。</span><span class="sxs-lookup"><span data-stu-id="73b02-158">Consider the following markup:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

<span data-ttu-id="73b02-159">生成される HTML:</span><span class="sxs-lookup"><span data-stu-id="73b02-159">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

<span data-ttu-id="73b02-160">ハッシュ タグはクライアント側アプリを構築するときに便利です。</span><span class="sxs-lookup"><span data-stu-id="73b02-160">Hash tags are useful when building client-side apps.</span></span> <span data-ttu-id="73b02-161">たとえば、JavaScript でのマーク付けや検索を簡単にするために使用できます。</span><span class="sxs-lookup"><span data-stu-id="73b02-161">They can be used for easy marking and searching in JavaScript, for example.</span></span>

## <a name="asp-area"></a><span data-ttu-id="73b02-162">asp-area</span><span class="sxs-lookup"><span data-stu-id="73b02-162">asp-area</span></span>

<span data-ttu-id="73b02-163">[asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) 属性は、適切なルートの設定に使用する領域名を設定します。</span><span class="sxs-lookup"><span data-stu-id="73b02-163">The [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) attribute sets the area name used to set the appropriate route.</span></span> <span data-ttu-id="73b02-164">領域の属性によってどのようにルートの再マップが行われるかの例を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="73b02-164">The following example depicts how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="73b02-165">`asp-area` を [ブログ] に設定すると、このアンカー タグの関連付けられているコントローラーとビューのルートに、ディレクトリ *Areas/Blogs* のプレフィックスが付けられます。</span><span class="sxs-lookup"><span data-stu-id="73b02-165">Setting `asp-area` to "Blogs" prefixes the directory *Areas/Blogs* to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="73b02-166">**{プロジェクト名}**</span><span class="sxs-lookup"><span data-stu-id="73b02-166">**{Project name}**</span></span>
  * <span data-ttu-id="73b02-167">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="73b02-167">**wwwroot**</span></span>
  * <span data-ttu-id="73b02-168">**領域**</span><span class="sxs-lookup"><span data-stu-id="73b02-168">**Areas**</span></span>
    * <span data-ttu-id="73b02-169">**ブログ**</span><span class="sxs-lookup"><span data-stu-id="73b02-169">**Blogs**</span></span>
      * <span data-ttu-id="73b02-170">**コントローラー**</span><span class="sxs-lookup"><span data-stu-id="73b02-170">**Controllers**</span></span>
        * <span data-ttu-id="73b02-171">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="73b02-171">*HomeController.cs*</span></span>
      * <span data-ttu-id="73b02-172">**ビュー**</span><span class="sxs-lookup"><span data-stu-id="73b02-172">**Views**</span></span>
        * <span data-ttu-id="73b02-173">**Home**</span><span class="sxs-lookup"><span data-stu-id="73b02-173">**Home**</span></span>
          * <span data-ttu-id="73b02-174">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="73b02-174">*AboutBlog.cshtml*</span></span>
          * <span data-ttu-id="73b02-175">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="73b02-175">*Index.cshtml*</span></span>
        * <span data-ttu-id="73b02-176">*\_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="73b02-176">*\_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="73b02-177">**コントローラー**</span><span class="sxs-lookup"><span data-stu-id="73b02-177">**Controllers**</span></span>

<span data-ttu-id="73b02-178">上記のディレクトリ階層の場合、*AboutBlog.cshtml* ファイルを参照するマークアップは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="73b02-178">Given the preceding directory hierarchy, the markup to reference the *AboutBlog.cshtml* file is:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

<span data-ttu-id="73b02-179">生成される HTML:</span><span class="sxs-lookup"><span data-stu-id="73b02-179">The generated HTML:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> <span data-ttu-id="73b02-180">領域が MVC アプリで動作するには、ルート テンプレートに領域への参照 (存在する場合) が含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="73b02-180">For areas to work in an MVC app, the route template must include a reference to the area, if it exists.</span></span> <span data-ttu-id="73b02-181">そのテンプレートは、*Startup.Configure* の `routes.MapRoute` メソッド呼び出しの 2 番目のパラメーターで表されます</span><span class="sxs-lookup"><span data-stu-id="73b02-181">That template is represented by the second parameter of the `routes.MapRoute` method call in *Startup.Configure*:</span></span>
>
> [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a><span data-ttu-id="73b02-182">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="73b02-182">asp-protocol</span></span>

<span data-ttu-id="73b02-183">[asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) 属性は URL に (`https` などの) プロトコルを指定するためにあります。</span><span class="sxs-lookup"><span data-stu-id="73b02-183">The [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) attribute is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="73b02-184">例:</span><span class="sxs-lookup"><span data-stu-id="73b02-184">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

<span data-ttu-id="73b02-185">生成される HTML:</span><span class="sxs-lookup"><span data-stu-id="73b02-185">The generated HTML:</span></span>

```html
<a href="https://localhost/Home/About">About</a>
```

<span data-ttu-id="73b02-186">例ではホスト名が localhost ですが、アンカー タグ ヘルパーは URL を生成するときに Web サイトのパブリック ドメインを使用します。</span><span class="sxs-lookup"><span data-stu-id="73b02-186">The host name in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="asp-host"></a><span data-ttu-id="73b02-187">asp-host</span><span class="sxs-lookup"><span data-stu-id="73b02-187">asp-host</span></span>

<span data-ttu-id="73b02-188">[asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) 属性は URL のホスト名を指定するためにあります。</span><span class="sxs-lookup"><span data-stu-id="73b02-188">The [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) attribute is for specifying a host name in your URL.</span></span> <span data-ttu-id="73b02-189">例:</span><span class="sxs-lookup"><span data-stu-id="73b02-189">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

<span data-ttu-id="73b02-190">生成される HTML:</span><span class="sxs-lookup"><span data-stu-id="73b02-190">The generated HTML:</span></span>

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a><span data-ttu-id="73b02-191">asp-page</span><span class="sxs-lookup"><span data-stu-id="73b02-191">asp-page</span></span>

<span data-ttu-id="73b02-192">[asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) 属性は Razor ページで使用されます。</span><span class="sxs-lookup"><span data-stu-id="73b02-192">The [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) attribute is used with Razor Pages.</span></span> <span data-ttu-id="73b02-193">アンカー タグの `href` 属性の値を特定のページに設定するために使用します。</span><span class="sxs-lookup"><span data-stu-id="73b02-193">Use it to set an anchor tag's `href` attribute value to a specific page.</span></span> <span data-ttu-id="73b02-194">ページ名の前にスラッシュ "/" を付けると URL が作成されます。</span><span class="sxs-lookup"><span data-stu-id="73b02-194">Prefixing the page name with a forward slash ("/") creates the URL.</span></span>

<span data-ttu-id="73b02-195">次の例は、出席者の Razor ページを示しています。</span><span class="sxs-lookup"><span data-stu-id="73b02-195">The following sample points to the attendee Razor Page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

<span data-ttu-id="73b02-196">生成される HTML:</span><span class="sxs-lookup"><span data-stu-id="73b02-196">The generated HTML:</span></span>

```html
<a href="/Attendee">All Attendees</a>
```

<span data-ttu-id="73b02-197">`asp-page` 属性は `asp-route`、`asp-controller`、`asp-action` の各属性と相互に排他的です。</span><span class="sxs-lookup"><span data-stu-id="73b02-197">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="73b02-198">ただし、`asp-page` は、次のマークアップのサンプルで示されているように、`asp-route-{value}` とともに使用してルーティングを制御できます。</span><span class="sxs-lookup"><span data-stu-id="73b02-198">However, `asp-page` can be used with `asp-route-{value}` to control routing, as the following markup demonstrates:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

<span data-ttu-id="73b02-199">生成される HTML:</span><span class="sxs-lookup"><span data-stu-id="73b02-199">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a><span data-ttu-id="73b02-200">asp-page-handler</span><span class="sxs-lookup"><span data-stu-id="73b02-200">asp-page-handler</span></span>

<span data-ttu-id="73b02-201">[asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) 属性は Razor ページで使用されます。</span><span class="sxs-lookup"><span data-stu-id="73b02-201">The [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) attribute is used with Razor Pages.</span></span> <span data-ttu-id="73b02-202">特定のページ ハンドラーへのリンクが目的です。</span><span class="sxs-lookup"><span data-stu-id="73b02-202">It's intended for linking to specific page handlers.</span></span>

<span data-ttu-id="73b02-203">次のページ ハンドラーがあるとします。</span><span class="sxs-lookup"><span data-stu-id="73b02-203">Consider the following page handler:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

<span data-ttu-id="73b02-204">ページ モデルの関連するマークアップが `OnGetProfile` ページ ハンドラーにリンクしています。</span><span class="sxs-lookup"><span data-stu-id="73b02-204">The page model's associated markup links to the `OnGetProfile` page handler.</span></span> <span data-ttu-id="73b02-205">ページ ハンドラーのメソッド名の `On<Verb>` プレフィックスは `asp-page-handler` 属性値では省略されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="73b02-205">Note that the `On<Verb>` prefix of the page handler method name is omitted in the `asp-page-handler` attribute value.</span></span> <span data-ttu-id="73b02-206">非同期メソッドの場合、`Async` サフィックスも省略されます。</span><span class="sxs-lookup"><span data-stu-id="73b02-206">If this were an asynchronous method, the `Async` suffix would be omitted too.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

<span data-ttu-id="73b02-207">生成される HTML:</span><span class="sxs-lookup"><span data-stu-id="73b02-207">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a><span data-ttu-id="73b02-208">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="73b02-208">Additional resources</span></span>

* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
