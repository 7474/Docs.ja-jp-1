---
title: ASP.NET Core の URL リライト ミドルウェア
author: guardrex
description: ASP.NET Core アプリケーションの URL リライト ミドルウェアを使用した URL の書き換えとリダイレクトについて説明します。
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: d9f33f34f75fe7bf534146c5a426335e74635018
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326070"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="6236d-103">ASP.NET Core の URL リライト ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="6236d-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="6236d-104">作成者: [Luke Latham](https://github.com/guardrex) および [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="6236d-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="6236d-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="6236d-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6236d-106">URL の書き換えは、1 つまたは複数の事前定義された規則に基づいて URL 要求を変更する操作です。</span><span class="sxs-lookup"><span data-stu-id="6236d-106">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="6236d-107">URL 書き換えは、リソースの場所とそれらのアドレスの間の抽象化を作成し、場所とアドレスが緊密にリンクされていないようにします。</span><span class="sxs-lookup"><span data-stu-id="6236d-107">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="6236d-108">URL 書き換えが役に立ついくつかのシナリオがあります。</span><span class="sxs-lookup"><span data-stu-id="6236d-108">There are several scenarios where URL rewriting is valuable:</span></span>

* <span data-ttu-id="6236d-109">サーバー リソースの安定したロケーターを維持しながらそれらのリソースを一時的または永続的に移動するか置き換える。</span><span class="sxs-lookup"><span data-stu-id="6236d-109">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources.</span></span>
* <span data-ttu-id="6236d-110">要求処理を異なる複数のアプリまたは 1 つのアプリの複数の区分にわたって分割する。</span><span class="sxs-lookup"><span data-stu-id="6236d-110">Splitting request processing across different apps or across areas of one app.</span></span>
* <span data-ttu-id="6236d-111">受信した要求の URL セグメントを削除、追加、または再編成する。</span><span class="sxs-lookup"><span data-stu-id="6236d-111">Removing, adding, or reorganizing URL segments on incoming requests.</span></span>
* <span data-ttu-id="6236d-112">検索エンジン最適化 (SEO) のためにパブリック URL を最適化する。</span><span class="sxs-lookup"><span data-stu-id="6236d-112">Optimizing public URLs for Search Engine Optimization (SEO).</span></span>
* <span data-ttu-id="6236d-113">リンクをたどることで見つかるコンテンツをユーザーが予測しやすくするフレンドリなパブリック URL の使用を許可する。</span><span class="sxs-lookup"><span data-stu-id="6236d-113">Permitting the use of friendly public URLs to help people predict the content they will find by following a link.</span></span>
* <span data-ttu-id="6236d-114">セキュリティで保護されていない要求をセキュリティで保護されたエンドポイントにリダイレクトする。</span><span class="sxs-lookup"><span data-stu-id="6236d-114">Redirecting insecure requests to secure endpoints.</span></span>
* <span data-ttu-id="6236d-115">イメージのホットリンクを防止する。</span><span class="sxs-lookup"><span data-stu-id="6236d-115">Preventing image hotlinking.</span></span>

<span data-ttu-id="6236d-116">正規表現、Apache mod_rewrite モジュール ルール、IIS Rewrite Module ルール、カスタム ルール ロジックの使用など、いくつかの方法で URL を変更するルールを定義することができます。</span><span class="sxs-lookup"><span data-stu-id="6236d-116">You can define rules for changing the URL in several ways, including Regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="6236d-117">このドキュメントでは、ASP.NET Core アプリの URL リライト ミドルウェアを使用して URL を書き換える手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="6236d-117">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="6236d-118">URL の書き換えによってアプリのパフォーマンスが低下することがあります。</span><span class="sxs-lookup"><span data-stu-id="6236d-118">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="6236d-119">可能であれば、ルールの複雑さと数を制限する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6236d-119">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="6236d-120">URL リダイレクトと URL 書き換え</span><span class="sxs-lookup"><span data-stu-id="6236d-120">URL redirect and URL rewrite</span></span>

<span data-ttu-id="6236d-121">*URL リダイレクト*と *URL 書き換え*の表現の違いは、最初はわずかに見えるかもしれませんが、クライアントへのリソースの提供に関しては重要な意味を持っています。</span><span class="sxs-lookup"><span data-stu-id="6236d-121">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="6236d-122">ASP.NET Core の URL リライト ミドルウェアは、両方のニーズを満たすことができます。</span><span class="sxs-lookup"><span data-stu-id="6236d-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="6236d-123">*URL リダイレクト*は、クライアントに別のアドレスにあるリソースにアクセスするよう指示するクライアント側の操作です。</span><span class="sxs-lookup"><span data-stu-id="6236d-123">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="6236d-124">これによりサーバーへのラウンドトリップが必要になります。</span><span class="sxs-lookup"><span data-stu-id="6236d-124">This requires a round-trip to the server.</span></span> <span data-ttu-id="6236d-125">クライアントが、リソースの新しい要求を実行するときに、クライアントに返されたリダイレクト URL がブラウザーのアドレス バーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="6236d-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> 

<span data-ttu-id="6236d-126">`/resource` が `/different-resource` に*リダイレクト*される場合、クライアントは `/resource` を要求します。</span><span class="sxs-lookup"><span data-stu-id="6236d-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`.</span></span> <span data-ttu-id="6236d-127">サーバーは、リダイレクトが一時的または永続的のどちらかを示すステータスコードと共に応答し、`/different-resource` にあるリソースをクライアントが取得する必要があることを示します。</span><span class="sxs-lookup"><span data-stu-id="6236d-127">The server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="6236d-128">クライアントは、リダイレクト URL にあるリソースに対する新しい要求を実行します。</span><span class="sxs-lookup"><span data-stu-id="6236d-128">The client executes a new request for the resource at the redirect URL.</span></span>

![WebAPI サービス エンドポイントは、サーバー上でバージョン 1 (v1) からバージョン 2 (v2) に一時的に変更されました。](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="6236d-134">要求を別の URL にリダイレクトする場合は、リダイレクトが永続的または一時的のどちらかを示します。</span><span class="sxs-lookup"><span data-stu-id="6236d-134">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="6236d-135">301 (永続的な移動) 状態コードは、リソースに新しい永続的な URL があり、リソースに対する将来のすべての要求で新しい URL を使用する必要があることをクライアントに指示したい場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="6236d-135">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="6236d-136">*301 状態コードを受信すると、クライアントは応答をキャッシュに入れる場合があります。*</span><span class="sxs-lookup"><span data-stu-id="6236d-136">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="6236d-137">302 (検出) 状態コードは、リダイレクトが一時的または一般的に変更される場合に使用され、クライアントがリダイレクト URL を保存して再利用しないようにします。</span><span class="sxs-lookup"><span data-stu-id="6236d-137">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="6236d-138">詳細については、「[RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)」(RFC 2616: ステータスコードの定義) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6236d-138">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="6236d-139">*URL 書き換え*は、別のリソースのアドレスからのリソースを提供するサーバー側の操作です。</span><span class="sxs-lookup"><span data-stu-id="6236d-139">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="6236d-140">URL の書き換えには、サーバーへのラウンドトリップは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="6236d-140">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="6236d-141">書き換えられた URL は、クライアントに返されず、ブラウザーのアドレス バーに表示されません。</span><span class="sxs-lookup"><span data-stu-id="6236d-141">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="6236d-142">`/resource` が `/different-resource` に*書き換えられた*場合、クライアントは `/resource` を要求し、サーバーは `/different-resource` にあるリソースを*内部的に*取得します。</span><span class="sxs-lookup"><span data-stu-id="6236d-142">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="6236d-143">クライアントは、書き換えられた URL にあるリソースを取得できる場合がありますが、クライアントは、その要求を実行して応答を受信したときに、書き換えられた URL にリソースが存在することを通知されません。</span><span class="sxs-lookup"><span data-stu-id="6236d-143">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![WebAPI サービス エンドポイントは、サーバー上でバージョン 1 (v1) からバージョン 2 (v2) に変更されました。](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="6236d-148">URL リライト サンプル アプリ</span><span class="sxs-lookup"><span data-stu-id="6236d-148">URL rewriting sample app</span></span>

<span data-ttu-id="6236d-149">[URL リライト サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/)を使用して、URL リライト ミドルウェアの機能を調べることができます。</span><span class="sxs-lookup"><span data-stu-id="6236d-149">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span></span> <span data-ttu-id="6236d-150">アプリが書き換えとリダイレクトのルールを適用し、書き換えおよびリダイレクトが実行された URL を表示します。</span><span class="sxs-lookup"><span data-stu-id="6236d-150">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="6236d-151">URL リライト ミドルウェアを使用する状況</span><span class="sxs-lookup"><span data-stu-id="6236d-151">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="6236d-152">URL リライト ミドルウェアは、Windows サーバー上の IIS の [URL リライト モジュール](https://www.iis.net/downloads/microsoft/url-rewrite)と、Apache サーバー上の [Apache mod_rewrite モジュール](https://httpd.apache.org/docs/2.4/rewrite/)、[Nginx 上の URL 書き換え](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)を使用できない場合、またはアプリが [HTTP.sys サーバー](xref:fundamentals/servers/httpsys) (旧称 [WebListener](xref:fundamentals/servers/weblistener)) 上でホストされている場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="6236d-152">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="6236d-153">IIS、Apache、Nginx でサーバー ベースの URL 書き換テクノロジを使用する主な理由は、ミドルウェアがこれらのモジュールの完全な機能をサポートせず、ミドルウェアのパフォーマンスがモジュールのパフォーマンスと一致しないことです。</span><span class="sxs-lookup"><span data-stu-id="6236d-153">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="6236d-154">ただし、IIS リライト モジュールの `IsFile` 制約や `IsDirectory` 制約など、ASP.NET Core プロジェクトと連携しないサーバー モジュールのいくつかの機能があります。</span><span class="sxs-lookup"><span data-stu-id="6236d-154">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="6236d-155">これらのシナリオでは、代わりにミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="6236d-155">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="6236d-156">Package</span><span class="sxs-lookup"><span data-stu-id="6236d-156">Package</span></span>

<span data-ttu-id="6236d-157">ミドルウェアをプロジェクトに含めるには、[ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/)パッケージへの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="6236d-157">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="6236d-158">この機能は、ASP.NET Core 1.1.0 以上をターゲットとするアプリで使用できます。</span><span class="sxs-lookup"><span data-stu-id="6236d-158">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="6236d-159">拡張機能とオプション</span><span class="sxs-lookup"><span data-stu-id="6236d-159">Extension and options</span></span>

<span data-ttu-id="6236d-160">各ルールの拡張メソッドを使用して、[RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) クラスのインスタンスを作成することによって、URL 書き換えとリダイレクトを確立します。</span><span class="sxs-lookup"><span data-stu-id="6236d-160">Establish your URL rewrite and redirect rules by creating an instance of the [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) class with extension methods for each of your rules.</span></span> <span data-ttu-id="6236d-161">処理したい順序で複数のルールを連結します。</span><span class="sxs-lookup"><span data-stu-id="6236d-161">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="6236d-162">`RewriteOptions` は、`app.UseRewriter(options);` を使用して要求パイプラインに追加されたときに、URL リライト ミドルウェアに渡されます。</span><span class="sxs-lookup"><span data-stu-id="6236d-162">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1")
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true)
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")
        .Add(RedirectXMLRequests)
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="redirect-non-www-to-www"></a><span data-ttu-id="6236d-163">非 WWW を WWW にリダイレクトする</span><span class="sxs-lookup"><span data-stu-id="6236d-163">Redirect non-www to www</span></span>

<span data-ttu-id="6236d-164">アプリで非 `www` の要求を `www` にリダイレクトできる 3 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="6236d-164">Three options permit the app to redirect non-`www` requests to `www`:</span></span>

* <span data-ttu-id="6236d-165">[AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; 要求が非 `www` の場合、要求を `www` サブドメインに永続的にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="6236d-165">[AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; Permanently redirect the request to the `www` subdomain if the request is non-`www`.</span></span> <span data-ttu-id="6236d-166">[Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect) 状態コードでリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="6236d-166">Redirects with a [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect) status code.</span></span>
* <span data-ttu-id="6236d-167">[AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; 受信した要求が非 `www` の場合、要求を `www` サブドメインにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="6236d-167">[AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="6236d-168">[Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) 状態コードでリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="6236d-168">Redirects with a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) status code.</span></span>
* <span data-ttu-id="6236d-169">[AddRedirectToWww(RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; 受信した要求が非 `www` の場合、要求を `www` サブドメインにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="6236d-169">[AddRedirectToWww(RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="6236d-170">応答に対する状態コードを提供することができます。</span><span class="sxs-lookup"><span data-stu-id="6236d-170">Allows you to provide the status code for the response.</span></span> <span data-ttu-id="6236d-171">`AddRedirectToWww` への割り当てには [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) クラスのフィールドを使用します。</span><span class="sxs-lookup"><span data-stu-id="6236d-171">Use the fields of the [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) class for assignments to `AddRedirectToWww`.</span></span>

::: moniker-end

### <a name="url-redirect"></a><span data-ttu-id="6236d-172">URL リダイレクト</span><span class="sxs-lookup"><span data-stu-id="6236d-172">URL redirect</span></span>

<span data-ttu-id="6236d-173">要求をリダイレクトするには、`AddRedirect` を使用します。</span><span class="sxs-lookup"><span data-stu-id="6236d-173">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="6236d-174">最初のパラメーターには、着信 URL のパスに一致する正規表現が含まれています。</span><span class="sxs-lookup"><span data-stu-id="6236d-174">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="6236d-175">2 番目のパラメーターは、置換文字列です。</span><span class="sxs-lookup"><span data-stu-id="6236d-175">The second parameter is the replacement string.</span></span> <span data-ttu-id="6236d-176">3 番目のパラメーター (存在する場合) は、状態コードを指定します。</span><span class="sxs-lookup"><span data-stu-id="6236d-176">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="6236d-177">状態コードを指定しない場合、既定値である 302 (検出) に設定されます。これは、リソースが一時的に移動されるか置き換えられることを示します。</span><span class="sxs-lookup"><span data-stu-id="6236d-177">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="6236d-178">開発者ツールが有効になっているブラウザーで、パス `/redirect-rule/1234/5678` を指定してサンプル アプリに対する要求を実行します。</span><span class="sxs-lookup"><span data-stu-id="6236d-178">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="6236d-179">正規表現が `redirect-rule/(.*)` の要求のパスに一致し、パスが `/redirected/1234/5678` に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="6236d-179">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="6236d-180">リダイレクト URL が、302 (検出) の状態コードでクライアントに返されます。</span><span class="sxs-lookup"><span data-stu-id="6236d-180">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="6236d-181">ブラウザーは、ブラウザーのアドレス バーに表示されるリダイレクト URL に新しい要求を実行します</span><span class="sxs-lookup"><span data-stu-id="6236d-181">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="6236d-182">サンプル アプリにはリダイレクト URL に一致するルールがないので、2 番目の要求は、アプリから 200 (OK) 応答を受信し、応答の本文にリダイレクト URL が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6236d-182">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="6236d-183">URL が*リダイレクト*されるときにサーバーへのラウンドトリップが実行されます。</span><span class="sxs-lookup"><span data-stu-id="6236d-183">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="6236d-184">リダイレクト ルールを確立するときには注意してください。</span><span class="sxs-lookup"><span data-stu-id="6236d-184">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="6236d-185">リダイレクト ルールは、リダイレクト後を含めて、アプリへの各要求で評価されます。</span><span class="sxs-lookup"><span data-stu-id="6236d-185">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="6236d-186">無限リダイレクトのループを誤って作成することがよくあります。</span><span class="sxs-lookup"><span data-stu-id="6236d-186">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="6236d-187">元の要求: `/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="6236d-187">Original Request: `/redirect-rule/1234/5678`</span></span>

![要求および応答を追跡する開発者ツールを備えたブラウザー ウィンドウ](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="6236d-189">かっこ内に含まれる式の一部は、*キャプチャ グループ*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="6236d-189">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="6236d-190">式のドット (`.`) は、*どの文字とも一致する*ことを意味します。</span><span class="sxs-lookup"><span data-stu-id="6236d-190">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="6236d-191">アスタリスク (`*`) は、*直前の文字に 0 回以上一致する*ことを示します。</span><span class="sxs-lookup"><span data-stu-id="6236d-191">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="6236d-192">そのため、URL の最後の 2 つのパス セグメント `1234/5678` は、キャプチャ グループ `(.*)` によってキャプチャされます。</span><span class="sxs-lookup"><span data-stu-id="6236d-192">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="6236d-193">要求 URL で `redirect-rule/` の後に指定した任意の値が、この単一のキャプチャ グループによってキャプチャされます。</span><span class="sxs-lookup"><span data-stu-id="6236d-193">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="6236d-194">置換文字列では、キャプチャされたグループがドル記号 (`$`) を使用して文字列に挿入され、その後にキャプチャのシーケンス番号が続きます。</span><span class="sxs-lookup"><span data-stu-id="6236d-194">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="6236d-195">最初のキャプチャ グループ値は、`$1` で取得され、2 番目は `$2` で取得され、これらは正規表現内のキャプチャ グループの順序で続行されます。</span><span class="sxs-lookup"><span data-stu-id="6236d-195">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="6236d-196">サンプル アプリでは、リダイレクト ルールの正規表現内に 1 つのキャプチャ グループだけがあるので、置換文字列に 1 つの挿入されたグループ (`$1`) だけがあります。</span><span class="sxs-lookup"><span data-stu-id="6236d-196">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="6236d-197">ルールが適用されるときに、URL `/redirected/1234/5678` になります。</span><span class="sxs-lookup"><span data-stu-id="6236d-197">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="6236d-198">セキュリティで保護されたエンドポイントへの URL リダイレクト</span><span class="sxs-lookup"><span data-stu-id="6236d-198">URL redirect to a secure endpoint</span></span>

<span data-ttu-id="6236d-199">`AddRedirectToHttps` を使用して HTTP 要求を、HTTPS (`https://`) を使用する同じホストとパスにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="6236d-199">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="6236d-200">状態コードが指定されていない場合、ミドルウェアは既定で 302 (検出) に設定します。</span><span class="sxs-lookup"><span data-stu-id="6236d-200">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="6236d-201">ポートが指定されていない場合は、ミドルウェアは既定で `null` に設定します。つまり、プロトコルを `https://` に変更し、クライアントはポート 443 上でリソースにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="6236d-201">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="6236d-202">この例では、状態コードを 301 (完全な移動) に設定し、ポートを 5001 に変更する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="6236d-202">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="6236d-203">`AddRedirectToHttpsPermanent` を使用してセキュリティで保護されていない要求を、セキュリティで保護された HTTPS プロトコル (ポート 443 上の `https://`) を使用する同じホストとパスにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="6236d-203">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="6236d-204">ミドルウェアは状態コードを 301 (完全な移動) に設定します。</span><span class="sxs-lookup"><span data-stu-id="6236d-204">The middleware sets the status code to 301 (Moved Permanently).</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> <span data-ttu-id="6236d-205">リダイレクト規則を追加せずに HTTPS にリダイレクトする場合、HTTPS Redirection Middleware を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="6236d-205">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware.</span></span> <span data-ttu-id="6236d-206">詳細については、「[HTTPS の適用](xref:security/enforcing-ssl#require-https)」のトピックをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="6236d-206">For more information, see the [Enforce HTTPS](xref:security/enforcing-ssl#require-https) topic.</span></span>

<span data-ttu-id="6236d-207">サンプル アプリは、`AddRedirectToHttps` または `AddRedirectToHttpsPermanent` の使用方法の例を示すことができます。</span><span class="sxs-lookup"><span data-stu-id="6236d-207">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="6236d-208">拡張メソッドを `RewriteOptions` に追加します。</span><span class="sxs-lookup"><span data-stu-id="6236d-208">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="6236d-209">任意の URL でセキュリティ保護されていない要求をアプリに対して行います。</span><span class="sxs-lookup"><span data-stu-id="6236d-209">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="6236d-210">自己署名証明書が信頼できないことを示すブラウザーのセキュリティ警告を消去するか、証明書を信頼する例外を作成します。</span><span class="sxs-lookup"><span data-stu-id="6236d-210">Dismiss the browser security warning that the self-signed certificate is untrusted or create an exception to trust the certificate.</span></span>

<span data-ttu-id="6236d-211">`AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure` を使用する元の要求</span><span class="sxs-lookup"><span data-stu-id="6236d-211">Original Request using `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span></span>

![要求および応答を追跡する開発者ツールを備えたブラウザー ウィンドウ](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="6236d-213">`AddRedirectToHttpsPermanent`: `http://localhost:5000/secure` を使用する元の要求</span><span class="sxs-lookup"><span data-stu-id="6236d-213">Original Request using `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span></span>

![要求および応答を追跡する開発者ツールを備えたブラウザー ウィンドウ](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="6236d-215">URL 書き換え</span><span class="sxs-lookup"><span data-stu-id="6236d-215">URL rewrite</span></span>

<span data-ttu-id="6236d-216">URL 書き換えのルールを作成するには、`AddRewrite` を使用します。</span><span class="sxs-lookup"><span data-stu-id="6236d-216">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="6236d-217">最初のパラメーターには、着信 URL のパスに一致する正規表現が含まれています。</span><span class="sxs-lookup"><span data-stu-id="6236d-217">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="6236d-218">2 番目のパラメーターは、置換文字列です。</span><span class="sxs-lookup"><span data-stu-id="6236d-218">The second parameter is the replacement string.</span></span> <span data-ttu-id="6236d-219">3 番目のパラメーター `skipRemainingRules: {true|false}` は、現在のルールが適用される場合に追加の書き換えルールをスキップするかどうかをミドルウェアに示します。</span><span class="sxs-lookup"><span data-stu-id="6236d-219">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="6236d-220">元の要求: `/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="6236d-220">Original Request: `/rewrite-rule/1234/5678`</span></span>

![要求および応答を追跡する開発者ツールを備えたブラウザー ウィンドウ](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="6236d-222">正規表現で最初に注目するのは、式の先頭にあるキャレット (`^`) です。</span><span class="sxs-lookup"><span data-stu-id="6236d-222">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="6236d-223">これは、URL パスの先頭からマッチングが開始されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="6236d-223">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="6236d-224">リダイレクト ルール `redirect-rule/(.*)` を使用する先ほどの例では、正規表現の先頭のキャレットはありません。したがって、パス内で `redirect-rule/` の前にある任意の文字が一致します。</span><span class="sxs-lookup"><span data-stu-id="6236d-224">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="6236d-225">パス</span><span class="sxs-lookup"><span data-stu-id="6236d-225">Path</span></span>                               | <span data-ttu-id="6236d-226">一致したもの</span><span class="sxs-lookup"><span data-stu-id="6236d-226">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="6236d-227">[はい]</span><span class="sxs-lookup"><span data-stu-id="6236d-227">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="6236d-228">[はい]</span><span class="sxs-lookup"><span data-stu-id="6236d-228">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="6236d-229">[はい]</span><span class="sxs-lookup"><span data-stu-id="6236d-229">Yes</span></span>   |

<span data-ttu-id="6236d-230">書き換えルール `^rewrite-rule/(\d+)/(\d+)` は、`rewrite-rule/` で始まる場合のみパスと一致します。</span><span class="sxs-lookup"><span data-stu-id="6236d-230">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="6236d-231">下の書き換えルールの上のリダイレクト ルール間の一致の違いに注意してください。</span><span class="sxs-lookup"><span data-stu-id="6236d-231">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="6236d-232">パス</span><span class="sxs-lookup"><span data-stu-id="6236d-232">Path</span></span>                              | <span data-ttu-id="6236d-233">一致したもの</span><span class="sxs-lookup"><span data-stu-id="6236d-233">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="6236d-234">[はい]</span><span class="sxs-lookup"><span data-stu-id="6236d-234">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="6236d-235">×</span><span class="sxs-lookup"><span data-stu-id="6236d-235">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="6236d-236">×</span><span class="sxs-lookup"><span data-stu-id="6236d-236">No</span></span>    |

<span data-ttu-id="6236d-237">式の `^rewrite-rule/` に続く部分に、2 つのキャプチャ グループ `(\d+)/(\d+)` があります。</span><span class="sxs-lookup"><span data-stu-id="6236d-237">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="6236d-238">`\d` は、*1 桁 (数字) に一致する*ことを示します。</span><span class="sxs-lookup"><span data-stu-id="6236d-238">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="6236d-239">プラス記号 (`+`) は、*直前の 1 つ以上の文字に一致する*ことを意味します。</span><span class="sxs-lookup"><span data-stu-id="6236d-239">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="6236d-240">そのため、URL は、数字とその後に続くスラッシュ、さらにその後に続く別の数字を含んでいる必要があります。</span><span class="sxs-lookup"><span data-stu-id="6236d-240">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="6236d-241">これらのキャプチャ グループが `$1` および `$2` として書き換えられた URL に挿入されます。</span><span class="sxs-lookup"><span data-stu-id="6236d-241">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="6236d-242">書き換えルールの置換文字列は、クエリ文字列にキャプチャされたグループを配置します。</span><span class="sxs-lookup"><span data-stu-id="6236d-242">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="6236d-243">`/rewrite-rule/1234/5678` の要求されたパスは、`/rewritten?var1=1234&var2=5678` でリソースを取得するように書き換えられます。</span><span class="sxs-lookup"><span data-stu-id="6236d-243">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="6236d-244">クエリ文字列が元の要求に存在する場合、URL が書き換えられるときに保持されます。</span><span class="sxs-lookup"><span data-stu-id="6236d-244">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="6236d-245">リソースを取得するためのサーバーへのラウンドトリップはありません。</span><span class="sxs-lookup"><span data-stu-id="6236d-245">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="6236d-246">リソースが存在する場合は、取得され、200 (OK) 状態コードと共にクライアントに返されます。</span><span class="sxs-lookup"><span data-stu-id="6236d-246">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="6236d-247">クライアントはリダイレクトされていないため、ブラウザーのアドレス バーの URL は変更されません。</span><span class="sxs-lookup"><span data-stu-id="6236d-247">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="6236d-248">クライアントに関しては、URL 書き換え操作は発生しません。</span><span class="sxs-lookup"><span data-stu-id="6236d-248">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="6236d-249">式内の照合ルールは、高コストな処理であり、アプリケーションの応答時間を短縮するので、可能な限り `skipRemainingRules: true` を使用します。</span><span class="sxs-lookup"><span data-stu-id="6236d-249">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="6236d-250">最も高速なアプリの応答を実現するには以下のようにします。</span><span class="sxs-lookup"><span data-stu-id="6236d-250">For the fastest app response:</span></span>
> * <span data-ttu-id="6236d-251">一致する頻度が最も多いルールから一致頻度が最も少ないルールへの順番で書き換えルールを並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="6236d-251">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="6236d-252">一致が発生し、追加の処理が必要ないときに残りのルールの処理をスキップします。</span><span class="sxs-lookup"><span data-stu-id="6236d-252">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="6236d-253">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="6236d-253">Apache mod_rewrite</span></span>

<span data-ttu-id="6236d-254">`AddApacheModRewrite` を使用して Apache mod_rewrite ルールを適用します。</span><span class="sxs-lookup"><span data-stu-id="6236d-254">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="6236d-255">ルール ファイルがアプリと共に展開されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6236d-255">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="6236d-256">mod_rewrite ルールの情報と例については、「[Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6236d-256">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6236d-257">*ApacheModRewrite.txt* ルール ファイルからルールを読み取るには `StreamReader` を使用します。</span><span class="sxs-lookup"><span data-stu-id="6236d-257">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6236d-258">最初のパラメーターは、[依存関係の挿入](dependency-injection.md)によって提供される `IFileProvider` を取得します。</span><span class="sxs-lookup"><span data-stu-id="6236d-258">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="6236d-259">`IHostingEnvironment` が挿入され、`ContentRootFileProvider` を提供します。</span><span class="sxs-lookup"><span data-stu-id="6236d-259">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="6236d-260">2 番目のパラメーターは、ルール ファイルへのパスであり、これはサンプル アプリでは *ApacheModRewrite.txt* です。</span><span class="sxs-lookup"><span data-stu-id="6236d-260">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="6236d-261">サンプル アプリは、要求を `/apache-mod-rules-redirect/(.\*)` から `/redirected?id=$1` にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="6236d-261">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="6236d-262">応答の状態コードは 302 (検出) です。</span><span class="sxs-lookup"><span data-stu-id="6236d-262">The response status code is 302 (Found).</span></span>

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

<span data-ttu-id="6236d-263">元の要求: `/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="6236d-263">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![要求および応答を追跡する開発者ツールを備えたブラウザー ウィンドウ](url-rewriting/_static/add_apache_mod_redirect.png)

<span data-ttu-id="6236d-265">ミドルウェアは、次の Apache mod_rewrite サーバー変数をサポートします。</span><span class="sxs-lookup"><span data-stu-id="6236d-265">The middleware supports the following Apache mod_rewrite server variables:</span></span>

* <span data-ttu-id="6236d-266">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="6236d-266">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="6236d-267">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="6236d-267">HTTP_ACCEPT</span></span>
* <span data-ttu-id="6236d-268">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="6236d-268">HTTP_CONNECTION</span></span>
* <span data-ttu-id="6236d-269">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="6236d-269">HTTP_COOKIE</span></span>
* <span data-ttu-id="6236d-270">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="6236d-270">HTTP_FORWARDED</span></span>
* <span data-ttu-id="6236d-271">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="6236d-271">HTTP_HOST</span></span>
* <span data-ttu-id="6236d-272">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="6236d-272">HTTP_REFERER</span></span>
* <span data-ttu-id="6236d-273">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="6236d-273">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="6236d-274">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6236d-274">HTTPS</span></span>
* <span data-ttu-id="6236d-275">IPV6</span><span class="sxs-lookup"><span data-stu-id="6236d-275">IPV6</span></span>
* <span data-ttu-id="6236d-276">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="6236d-276">QUERY_STRING</span></span>
* <span data-ttu-id="6236d-277">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="6236d-277">REMOTE_ADDR</span></span>
* <span data-ttu-id="6236d-278">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="6236d-278">REMOTE_PORT</span></span>
* <span data-ttu-id="6236d-279">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="6236d-279">REQUEST_FILENAME</span></span>
* <span data-ttu-id="6236d-280">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="6236d-280">REQUEST_METHOD</span></span>
* <span data-ttu-id="6236d-281">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="6236d-281">REQUEST_SCHEME</span></span>
* <span data-ttu-id="6236d-282">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="6236d-282">REQUEST_URI</span></span>
* <span data-ttu-id="6236d-283">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="6236d-283">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="6236d-284">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="6236d-284">SERVER_ADDR</span></span>
* <span data-ttu-id="6236d-285">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="6236d-285">SERVER_PORT</span></span>
* <span data-ttu-id="6236d-286">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="6236d-286">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="6236d-287">TIME</span><span class="sxs-lookup"><span data-stu-id="6236d-287">TIME</span></span>
* <span data-ttu-id="6236d-288">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="6236d-288">TIME_DAY</span></span>
* <span data-ttu-id="6236d-289">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="6236d-289">TIME_HOUR</span></span>
* <span data-ttu-id="6236d-290">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="6236d-290">TIME_MIN</span></span>
* <span data-ttu-id="6236d-291">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="6236d-291">TIME_MON</span></span>
* <span data-ttu-id="6236d-292">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="6236d-292">TIME_SEC</span></span>
* <span data-ttu-id="6236d-293">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="6236d-293">TIME_WDAY</span></span>
* <span data-ttu-id="6236d-294">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="6236d-294">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="6236d-295">IIS URL リライト モジュールのルール</span><span class="sxs-lookup"><span data-stu-id="6236d-295">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="6236d-296">IIS URL リライト モジュールに適用されるルールを使用するには、`AddIISUrlRewrite` を使用します。</span><span class="sxs-lookup"><span data-stu-id="6236d-296">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="6236d-297">ルール ファイルがアプリと共に展開されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6236d-297">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="6236d-298">Windows Server IIS で実行されているときにミドルウェアで *web.config* ファイルを使用するように指定しないでください。</span><span class="sxs-lookup"><span data-stu-id="6236d-298">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="6236d-299">IIS を使用する場合、IIS リライト モジュールとの競合を避けるために、これらのルールを *web.config* の外部に保存する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6236d-299">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="6236d-300">IIS URL リライト モジュールのルールの詳細と例については、「[Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)」(URL リライト モジュール 2.0 の使用 ) と「[URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)」(URL リライト モジュール構成リファレンス) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6236d-300">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6236d-301">*IISUrlRewrite.xml* ルール ファイルからルールを読み取るには `StreamReader` を使用します。</span><span class="sxs-lookup"><span data-stu-id="6236d-301">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6236d-302">最初のパラメーターは `IFileProvider` を取得します。2 番目のパラメーターは XML ルール ファイルのパスであり、このサンプル アプリでは *IISUrlRewrite.xml* です。</span><span class="sxs-lookup"><span data-stu-id="6236d-302">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="6236d-303">サンプル アプリは、要求を `/iis-rules-rewrite/(.*)` から `/rewritten?id=$1` に書き換えます。</span><span class="sxs-lookup"><span data-stu-id="6236d-303">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="6236d-304">応答は、200 (OK) 状態コードでクライアントに送信されます。</span><span class="sxs-lookup"><span data-stu-id="6236d-304">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

<span data-ttu-id="6236d-305">元の要求: `/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="6236d-305">Original Request: `/iis-rules-rewrite/1234`</span></span>

![要求および応答を追跡する開発者ツールを備えたブラウザー ウィンドウ](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="6236d-307">望ましくない方法でアプリに影響するように構成されたサーバー レベル ルールを使用するアクティブな IIS リライト モジュールがある場合は、アプリに対して IIS リライト モジュールを無効にできます。</span><span class="sxs-lookup"><span data-stu-id="6236d-307">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="6236d-308">詳細については、「[Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules)」 (IIS モジュールの無効化) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6236d-308">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="6236d-309">サポートされていない機能</span><span class="sxs-lookup"><span data-stu-id="6236d-309">Unsupported features</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6236d-310">ASP.NET Core 2.x でリリースされたミドルウェアは、次の IIS URL リライト モジュールの機能をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="6236d-310">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="6236d-311">送信ルール</span><span class="sxs-lookup"><span data-stu-id="6236d-311">Outbound Rules</span></span>
* <span data-ttu-id="6236d-312">カスタム サーバー変数</span><span class="sxs-lookup"><span data-stu-id="6236d-312">Custom Server Variables</span></span>
* <span data-ttu-id="6236d-313">ワイルドカード</span><span class="sxs-lookup"><span data-stu-id="6236d-313">Wildcards</span></span>
* <span data-ttu-id="6236d-314">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="6236d-314">LogRewrittenUrl</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6236d-315">ASP.NET Core 1.x でリリースされたミドルウェアは、次の IIS URL リライト モジュールの機能をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="6236d-315">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="6236d-316">グローバル ルール</span><span class="sxs-lookup"><span data-stu-id="6236d-316">Global Rules</span></span>
* <span data-ttu-id="6236d-317">送信ルール</span><span class="sxs-lookup"><span data-stu-id="6236d-317">Outbound Rules</span></span>
* <span data-ttu-id="6236d-318">マップの書き直し</span><span class="sxs-lookup"><span data-stu-id="6236d-318">Rewrite Maps</span></span>
* <span data-ttu-id="6236d-319">CustomResponse アクション</span><span class="sxs-lookup"><span data-stu-id="6236d-319">CustomResponse Action</span></span>
* <span data-ttu-id="6236d-320">カスタム サーバー変数</span><span class="sxs-lookup"><span data-stu-id="6236d-320">Custom Server Variables</span></span>
* <span data-ttu-id="6236d-321">ワイルドカード</span><span class="sxs-lookup"><span data-stu-id="6236d-321">Wildcards</span></span>
* <span data-ttu-id="6236d-322">Action:CustomResponse</span><span class="sxs-lookup"><span data-stu-id="6236d-322">Action:CustomResponse</span></span>
* <span data-ttu-id="6236d-323">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="6236d-323">LogRewrittenUrl</span></span>

::: moniker-end

#### <a name="supported-server-variables"></a><span data-ttu-id="6236d-324">サポートされるサーバー変数</span><span class="sxs-lookup"><span data-stu-id="6236d-324">Supported server variables</span></span>

<span data-ttu-id="6236d-325">ミドルウェアは、次の IIS URL リライト モジュール サーバー変数をサポートします。</span><span class="sxs-lookup"><span data-stu-id="6236d-325">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>

* <span data-ttu-id="6236d-326">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="6236d-326">CONTENT_LENGTH</span></span>
* <span data-ttu-id="6236d-327">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="6236d-327">CONTENT_TYPE</span></span>
* <span data-ttu-id="6236d-328">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="6236d-328">HTTP_ACCEPT</span></span>
* <span data-ttu-id="6236d-329">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="6236d-329">HTTP_CONNECTION</span></span>
* <span data-ttu-id="6236d-330">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="6236d-330">HTTP_COOKIE</span></span>
* <span data-ttu-id="6236d-331">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="6236d-331">HTTP_HOST</span></span>
* <span data-ttu-id="6236d-332">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="6236d-332">HTTP_REFERER</span></span>
* <span data-ttu-id="6236d-333">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="6236d-333">HTTP_URL</span></span>
* <span data-ttu-id="6236d-334">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="6236d-334">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="6236d-335">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6236d-335">HTTPS</span></span>
* <span data-ttu-id="6236d-336">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="6236d-336">LOCAL_ADDR</span></span>
* <span data-ttu-id="6236d-337">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="6236d-337">QUERY_STRING</span></span>
* <span data-ttu-id="6236d-338">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="6236d-338">REMOTE_ADDR</span></span>
* <span data-ttu-id="6236d-339">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="6236d-339">REMOTE_PORT</span></span>
* <span data-ttu-id="6236d-340">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="6236d-340">REQUEST_FILENAME</span></span>
* <span data-ttu-id="6236d-341">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="6236d-341">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="6236d-342">`PhysicalFileProvider` を介して `IFileProvider` を取得することもできます。</span><span class="sxs-lookup"><span data-stu-id="6236d-342">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="6236d-343">このアプローチは、書き換えルール ファイルの場所に関する大きな柔軟性を提供する場合があります。</span><span class="sxs-lookup"><span data-stu-id="6236d-343">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="6236d-344">書き換えルール ファイルを指定したパスでサーバーに導入されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="6236d-344">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="6236d-345">メソッド ベースのルール</span><span class="sxs-lookup"><span data-stu-id="6236d-345">Method-based rule</span></span>

<span data-ttu-id="6236d-346">メソッド内で独自のルール ロジックを実装するには、`Add(Action<RewriteContext> applyRule)` を使用します。</span><span class="sxs-lookup"><span data-stu-id="6236d-346">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="6236d-347">`RewriteContext` は、メソッドで使用するための `HttpContext` を公開します。</span><span class="sxs-lookup"><span data-stu-id="6236d-347">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="6236d-348">`context.Result` は、追加のパイプライン処理の実行方法を決定します。</span><span class="sxs-lookup"><span data-stu-id="6236d-348">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="6236d-349">context.Result</span><span class="sxs-lookup"><span data-stu-id="6236d-349">context.Result</span></span>                       | <span data-ttu-id="6236d-350">アクション</span><span class="sxs-lookup"><span data-stu-id="6236d-350">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="6236d-351">`RuleResult.ContinueRules` (既定値)</span><span class="sxs-lookup"><span data-stu-id="6236d-351">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="6236d-352">ルールの適用を続行する</span><span class="sxs-lookup"><span data-stu-id="6236d-352">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="6236d-353">ルールの適用を停止し、応答を送信する</span><span class="sxs-lookup"><span data-stu-id="6236d-353">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="6236d-354">ルールの適用を停止し、次のミドルウェアにコンテキストを送信する</span><span class="sxs-lookup"><span data-stu-id="6236d-354">Stop applying rules and send the context to the next middleware</span></span> |

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="6236d-355">サンプル アプリは、*.xml* で終了するパスの要求をリダイレクトするメソッドの例を示します。</span><span class="sxs-lookup"><span data-stu-id="6236d-355">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="6236d-356">`/file.xml` の要求を行う場合、`/xmlfiles/file.xml` にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="6236d-356">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="6236d-357">状態コードが 301 (完全な移動) に設定されます。</span><span class="sxs-lookup"><span data-stu-id="6236d-357">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="6236d-358">リダイレクトの場合、応答の状態コードを明示的に設定する必要があります。そのようにしないと、200 (OK) の状態コードが返され、クライアントでリダイレクトは発生しません。</span><span class="sxs-lookup"><span data-stu-id="6236d-358">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

<span data-ttu-id="6236d-359">元の要求: `/file.xml`</span><span class="sxs-lookup"><span data-stu-id="6236d-359">Original Request: `/file.xml`</span></span>

![file.xml の要求および応答を追跡する開発者ツールを備えたブラウザー ウィンドウ](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="6236d-361">IRule ベースのルール</span><span class="sxs-lookup"><span data-stu-id="6236d-361">IRule-based rule</span></span>

<span data-ttu-id="6236d-362">`IRule` から派生するクラス内で独自のルール ロジックを実装するには、`Add(IRule)` を使用します。</span><span class="sxs-lookup"><span data-stu-id="6236d-362">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="6236d-363">`IRule` を使用すると、メソッド ベースのルールのアプローチを使用する場合により高い柔軟性が実現します。</span><span class="sxs-lookup"><span data-stu-id="6236d-363">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="6236d-364">派生クラスにコンストラクターを含め、`ApplyRule` メソッドのパラメーターで渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="6236d-364">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="6236d-365">サンプル アプリの `extension` と `newPath` のパラメーターの値は、いくつかの条件を満たすかチェックされます。</span><span class="sxs-lookup"><span data-stu-id="6236d-365">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="6236d-366">`extension` は、値を含んでいる必要があり、値は *.png*、*.jpg*、または *.gif* でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="6236d-366">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="6236d-367">`newPath` が有効ではない場合、`ArgumentException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="6236d-367">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="6236d-368">*image.png* の要求を行う場合、`/png-images/image.png` にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="6236d-368">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="6236d-369">*image.jpg* の要求を行う場合、`/jpg-images/image.jpg` にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="6236d-369">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="6236d-370">状態コードが 301 (完全な移動) に設定され、`context.Result` がルールの処理を停止し、応答を送信するように設定されます。</span><span class="sxs-lookup"><span data-stu-id="6236d-370">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

<span data-ttu-id="6236d-371">元の要求: `/image.png`</span><span class="sxs-lookup"><span data-stu-id="6236d-371">Original Request: `/image.png`</span></span>

![image.png の要求および応答を追跡する開発者ツールを備えたブラウザー ウィンドウ](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="6236d-373">元の要求: `/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="6236d-373">Original Request: `/image.jpg`</span></span>

![image.jpg の要求および応答を追跡する開発者ツールを備えたブラウザー ウィンドウ](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="6236d-375">正規表現の例</span><span class="sxs-lookup"><span data-stu-id="6236d-375">Regex examples</span></span>

| <span data-ttu-id="6236d-376">Goal</span><span class="sxs-lookup"><span data-stu-id="6236d-376">Goal</span></span> | <span data-ttu-id="6236d-377">正規表現文字列と</span><span class="sxs-lookup"><span data-stu-id="6236d-377">Regex String &</span></span><br><span data-ttu-id="6236d-378">一致の例</span><span class="sxs-lookup"><span data-stu-id="6236d-378">Match Example</span></span> | <span data-ttu-id="6236d-379">置換文字列と</span><span class="sxs-lookup"><span data-stu-id="6236d-379">Replacement String &</span></span><br><span data-ttu-id="6236d-380">出力の例</span><span class="sxs-lookup"><span data-stu-id="6236d-380">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="6236d-381">クエリ文字列内のパスを書き換える</span><span class="sxs-lookup"><span data-stu-id="6236d-381">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="6236d-382">末尾のスラッシュを除去する</span><span class="sxs-lookup"><span data-stu-id="6236d-382">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="6236d-383">末尾のスラッシュを強制する</span><span class="sxs-lookup"><span data-stu-id="6236d-383">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="6236d-384">特定の要求を書き換えを回避する</span><span class="sxs-lookup"><span data-stu-id="6236d-384">Avoid rewriting specific requests</span></span> | <span data-ttu-id="6236d-385">`^(.*)(?<!\.axd)$` または `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="6236d-385">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="6236d-386">はい: `/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="6236d-386">Yes: `/resource.htm`</span></span><br><span data-ttu-id="6236d-387">いいえ: `/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="6236d-387">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="6236d-388">URL セグメントを再配置する</span><span class="sxs-lookup"><span data-stu-id="6236d-388">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="6236d-389">URL セグメントを置き換える</span><span class="sxs-lookup"><span data-stu-id="6236d-389">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="6236d-390">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="6236d-390">Additional resources</span></span>

* [<span data-ttu-id="6236d-391">アプリケーションの起動</span><span class="sxs-lookup"><span data-stu-id="6236d-391">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="6236d-392">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="6236d-392">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="6236d-393">.NET の正規表現</span><span class="sxs-lookup"><span data-stu-id="6236d-393">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="6236d-394">正規表現言語 - クイック リファレンス</span><span class="sxs-lookup"><span data-stu-id="6236d-394">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="6236d-395">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="6236d-395">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="6236d-396">URL リライト モジュール 2.0 (IIS 用) の使用</span><span class="sxs-lookup"><span data-stu-id="6236d-396">Using Url Rewrite Module 2.0 (for IIS)</span></span>](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="6236d-397">URL リライト モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="6236d-397">URL Rewrite Module Configuration Reference</span></span>](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="6236d-398">IIS URL リライト モジュール フォーラム</span><span class="sxs-lookup"><span data-stu-id="6236d-398">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="6236d-399">単純な URL 構造を保持する</span><span class="sxs-lookup"><span data-stu-id="6236d-399">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="6236d-400">URL 書き換えの 10 のヒントとテクニック</span><span class="sxs-lookup"><span data-stu-id="6236d-400">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="6236d-401">スラッシュを使用すべきかどうか</span><span class="sxs-lookup"><span data-stu-id="6236d-401">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
