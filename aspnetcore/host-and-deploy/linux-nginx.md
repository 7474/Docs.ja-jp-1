---
title: Nginx 搭載の Linux で ASP.NET Core をホストする
author: rick-anderson
description: Ubuntu 16.04 でリバース プロキシとして Nginx をセットアップし、Kestrel で実行している ASP.NET Core Web アプリに HTTP トラフィックを転送する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 10/09/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 8d3c158b44c9f30e7c0746398306aa1c0fd9e15b
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912117"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="fa17a-103">Nginx 搭載の Linux で ASP.NET Core をホストする</span><span class="sxs-lookup"><span data-stu-id="fa17a-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="fa17a-104">[Sourabh Shirhatti](https://twitter.com/sshirhatti) による投稿</span><span class="sxs-lookup"><span data-stu-id="fa17a-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="fa17a-105">このガイドでは、Ubuntu 16.04 サーバーで本稼働対応の ASP.NET Core 環境をセットアップする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="fa17a-106">これらの手順は、Ubuntu のより新しいバージョンで動作する可能性が高いですが、手順はまだ新しいバージョンでテストされていません。</span><span class="sxs-lookup"><span data-stu-id="fa17a-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

<span data-ttu-id="fa17a-107">ASP.NET Core でサポートされている他の Linux ディストリビューションについて詳しくは、「[Linux における .NET Core の前提条件](/dotnet/core/linux-prerequisites)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="fa17a-107">For information on other Linux distributions supported by ASP.NET Core, see [Prerequisites for .NET Core on Linux](/dotnet/core/linux-prerequisites).</span></span>

> [!NOTE]
> <span data-ttu-id="fa17a-108">Ubuntu 14.04 の場合、Kestrel プロセスを監視するためのソリューションとして *supervisord* が推奨されます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-108">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="fa17a-109">*systemd* は Ubuntu 14.04 ではご利用いただけません。</span><span class="sxs-lookup"><span data-stu-id="fa17a-109">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="fa17a-110">Ubuntu 14.04 の手順については、[このトピックの前のバージョンを参照してください](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)。</span><span class="sxs-lookup"><span data-stu-id="fa17a-110">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="fa17a-111">このガイドでは:</span><span class="sxs-lookup"><span data-stu-id="fa17a-111">This guide:</span></span>

* <span data-ttu-id="fa17a-112">リバース プロキシ サーバーの背後に既存の ASP.NET Core アプリを配置します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-112">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="fa17a-113">Kestrel Web サーバーに要求を転送するようにリバース プロキシ サーバーを設定します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-113">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="fa17a-114">Web アプリを起動時にデーモンとして実行します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-114">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="fa17a-115">Web アプリの再起動に役立つようにプロセス管理ツールを構成します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-115">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fa17a-116">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="fa17a-116">Prerequisites</span></span>

1. <span data-ttu-id="fa17a-117">Ubuntu 16.04 サーバーへのアクセスと sudo 特権が与えられた標準ユーザー アカウント。</span><span class="sxs-lookup"><span data-stu-id="fa17a-117">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="fa17a-118">サーバーへの .NET Core ランタイムのインストール。</span><span class="sxs-lookup"><span data-stu-id="fa17a-118">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="fa17a-119">[.NET の「All Downloads」 (すべてのダウンロード) ページ](https://www.microsoft.com/net/download/all)に移動します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-119">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="fa17a-120">プレビューではない最新のランタイムを、**Runtime** (ランタイム) の一覧から選択します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-120">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="fa17a-121">サーバーの Ubuntu のバージョンに一致する Ubuntu の説明を選択し、これに従います。</span><span class="sxs-lookup"><span data-stu-id="fa17a-121">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="fa17a-122">既存の ASP.NET Core アプリ。</span><span class="sxs-lookup"><span data-stu-id="fa17a-122">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="fa17a-123">アプリを介して発行およびコピーする</span><span class="sxs-lookup"><span data-stu-id="fa17a-123">Publish and copy over the app</span></span>

<span data-ttu-id="fa17a-124">[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)用にアプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-124">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="fa17a-125">開発環境から [dotnet publish](/dotnet/core/tools/dotnet-publish) を実行し、サーバー上で実行できるディレクトリ (たとえば、*bin/Release/&lt;target_framework_moniker&gt;/publish*) にアプリをパッケージします。</span><span class="sxs-lookup"><span data-stu-id="fa17a-125">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="fa17a-126">サーバーで .NET Core ランタイムを管理しない場合、アプリは[独立した展開](/dotnet/core/deploying/#self-contained-deployments-scd)として発行することもできます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-126">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="fa17a-127">組織のワークフローに統合されているツール (SCP や SFTP など) を使用して、サーバーに ASP.NET Core アプリをコピーします。</span><span class="sxs-lookup"><span data-stu-id="fa17a-127">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="fa17a-128">Web アプリは一般的に *var* ディレクトリの下に配置されます (たとえば、*var/www/helloapp*)。</span><span class="sxs-lookup"><span data-stu-id="fa17a-128">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="fa17a-129">運用展開シナリオの場合、継続的インテグレーション ワークフローが、アプリの発行処理とサーバーへの資産のコピーを行います。</span><span class="sxs-lookup"><span data-stu-id="fa17a-129">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="fa17a-130">アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="fa17a-130">Test the app:</span></span>

1. <span data-ttu-id="fa17a-131">コマンド ラインから `dotnet <app_assembly>.dll` アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-131">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="fa17a-132">ブラウザーで、`http://<serveraddress>:<port>` に移動し、アプリが Linux のローカルで動作することを検証します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-132">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="fa17a-133">リバース プロキシ サーバーを構成する</span><span class="sxs-lookup"><span data-stu-id="fa17a-133">Configure a reverse proxy server</span></span>

<span data-ttu-id="fa17a-134">リバース プロキシは、動的 Web アプリを提供するための一般的な仕組みです。</span><span class="sxs-lookup"><span data-stu-id="fa17a-134">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="fa17a-135">リバース プロキシは HTTP 要求を終了させ、ASP.NET Core アプリに転送します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-135">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="fa17a-136">リバース プロキシ サーバーの有無に関わらず、いずれの構成も有効で、ASP.NET Core 2.0 またはそれ以降のアプリ用にホスティング構成がサポートされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="fa17a-136">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="fa17a-137">詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fa17a-137">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="fa17a-138">リバース プロキシ サーバーを利用する</span><span class="sxs-lookup"><span data-stu-id="fa17a-138">Use a reverse proxy server</span></span>

<span data-ttu-id="fa17a-139">Kestrel は、ASP.NET Core から動的なコンテンツを提供するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-139">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="fa17a-140">ただし、Web サーバーとしての機能は、IIS、Apache、Nginx などのサーバーと比べると制限されます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-140">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="fa17a-141">リバース プロキシ サーバーは、静的コンテンツ サービス、要求のキャッシュ、要求の圧縮、HTTP サーバーからの SSL 終了などの作業の負荷を軽減します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-141">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="fa17a-142">リバース プロキシ サーバーは専用コンピューター上に置かれることもあれば、HTTP サーバーと並んで展開されることもあります。</span><span class="sxs-lookup"><span data-stu-id="fa17a-142">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="fa17a-143">このガイドの目的のために、単一インスタンスの Nginx が使用されます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-143">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="fa17a-144">HTTP サーバーと並んで、同じサーバー上で実行されます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-144">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="fa17a-145">要件に応じて、別のセットアップを選択することも可能です。</span><span class="sxs-lookup"><span data-stu-id="fa17a-145">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="fa17a-146">要求はリバース プロキシによって転送されます。そのため、[Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) パッケージの [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) を使用します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-146">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="fa17a-147">リダイレクト URI とその他のセキュリティ ポリシーを正しく機能させるために、このミドルウェアは、`X-Forwarded-Proto` ヘッダーを利用して、`Request.Scheme` を更新します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-147">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="fa17a-148">認証、リンクの生成、リダイレクト、および地理的位置情報など、スキームに依存するすべてのコンポーネントは、Forwarded Headers Middleware の呼び出し後に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fa17a-148">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="fa17a-149">一般的な規則として、Forwarded Headers Middleware は、診断およびエラー処理ミドルウェアを除くその他のミドルウェアより前に実行される必要があります。</span><span class="sxs-lookup"><span data-stu-id="fa17a-149">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="fa17a-150">この順序により、転送されるヘッダー情報に依存するミドルウェアが処理にヘッダー値を使用できます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-150">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fa17a-151">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fa17a-151">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fa17a-152">[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) や同様の認証スキームのミドルウェアを呼び出す前に、`Startup.Configure` で [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-152">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="fa17a-153">ミドルウェアを構成して、`X-Forwarded-For` および `X-Forwarded-Proto` ヘッダーを転送します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-153">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fa17a-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fa17a-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fa17a-155">[UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) や [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication)、または同様の認証スキームのミドルウェアを呼び出す前に、`Startup.Configure` で [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-155">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="fa17a-156">ミドルウェアを構成して、`X-Forwarded-For` および `X-Forwarded-Proto` ヘッダーを転送します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-156">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="fa17a-157">ミドルウェアに対して [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) が指定されていない場合、転送される既定のヘッダーは `None` です。</span><span class="sxs-lookup"><span data-stu-id="fa17a-157">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="fa17a-158">localhost (127.0.0.1, [::1]) で実行されるプロキシのみが既定で信頼されます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-158">Only proxies running on localhost (127.0.0.1, [::1]) are trusted by default.</span></span> <span data-ttu-id="fa17a-159">組織内のその他の信頼されているプロキシまたはネットワークによってインターネットと Web サーバーの間の要求が処理される場合は、それらを、<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> を使用して <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> または <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> のリストに追加します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-159">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="fa17a-160">次の例では、IP アドレス 10.0.0.100 にある信頼されているプロキシ サーバーが `Startup.ConfigureServices` 内の Forwarded Headers Middleware `KnownProxies` に追加されます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-160">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="fa17a-161">詳細については、「<xref:host-and-deploy/proxy-load-balancer>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fa17a-161">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-nginx"></a><span data-ttu-id="fa17a-162">Nginx をインストールする</span><span class="sxs-lookup"><span data-stu-id="fa17a-162">Install Nginx</span></span>

<span data-ttu-id="fa17a-163">`apt-get` を利用し、Nginx をインストールします。</span><span class="sxs-lookup"><span data-stu-id="fa17a-163">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="fa17a-164">インストーラーにより *systemd* init スクリプトが作成されます。このスクリプトがシステム起動時に Nginx をデーモンとして実行します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-164">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="fa17a-165">Ubuntu 用のインストール手順については、[Nginx: Debian/Ubuntu の公式パッケージ](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="fa17a-165">Follow the installation instructions for Ubuntu at [Nginx: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="fa17a-166">オプションの Nginx モジュールが必要な場合、Nginx をソースからビルドする必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="fa17a-166">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="fa17a-167">Nginx は初めてのインストールとなるので、次を実行して明示的に起動します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-167">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="fa17a-168">ブラウザーで Nginx の既定のランディング ページが表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-168">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="fa17a-169">ランディング ページは `http://<server_IP_address>/index.nginx-debian.html` からアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-169">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="fa17a-170">Nginx を構成する</span><span class="sxs-lookup"><span data-stu-id="fa17a-170">Configure Nginx</span></span>

<span data-ttu-id="fa17a-171">Nginx をリバース プロキシとして構成し、ASP.NET Core アプリに要求を転送するには、*/etc/nginx/sites-available/default* を変更します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-171">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="fa17a-172">テキスト エディターで開き、中身を次のものに変更します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-172">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

<span data-ttu-id="fa17a-173">`server_name` が一致しない場合、Nginx では既定のサーバーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-173">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="fa17a-174">既定のサーバーが定義されていない場合、構成ファイルの最初のサーバーが既定のサーバーとなります。</span><span class="sxs-lookup"><span data-stu-id="fa17a-174">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="fa17a-175">ベスト プラクティスとして、構成ファイルで 444 の状態コードを返す既定のサーバーを具体的に追加します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-175">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="fa17a-176">既定のサーバーの構成例は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="fa17a-176">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="fa17a-177">上記の構成ファイルと既定のサーバーでは、Nginx は、ホスト ヘッダー `example.com` または `*.example.com` で、ポート 80 でパブリック トラフィックを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-177">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="fa17a-178">これらのホストと一致しない要求は、Kestrel に転送されません。</span><span class="sxs-lookup"><span data-stu-id="fa17a-178">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="fa17a-179">Nginx は一致する要求を Kestrel (`http://localhost:5000`) に転送します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-179">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="fa17a-180">詳細については、「[How nginx processes a request](https://nginx.org/docs/http/request_processing.html)」(Nginx が要求を処理する方法) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="fa17a-180">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span> <span data-ttu-id="fa17a-181">Kestrel の IP/ポートを変更する場合は、[Kestrel のエンドポイントの構成](xref:fundamentals/servers/kestrel#endpoint-configuration)に関するセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="fa17a-181">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="fa17a-182">適切な [server_name directive](https://nginx.org/docs/http/server_names.html) を指定しないと、アプリにセキュリティ上の脆弱性が生じます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-182">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="fa17a-183">親ドメイン全体を制御する場合、サブドメイン ワイルドカード バインド (たとえば、`*.example.com`) にこのセキュリティ リスクはありません (脆弱である `*.com` とは対照的)。</span><span class="sxs-lookup"><span data-stu-id="fa17a-183">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="fa17a-184">詳細については、[rfc7230 セクション-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fa17a-184">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="fa17a-185">Nginx の構成を確立したら、`sudo nginx -t` を実行して構成ファイルの構文を確認します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-185">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="fa17a-186">構成ファイルがテストに合格したら、`sudo nginx -s reload` を実行することで、強制的に Nginx に変更を反映させます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-186">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="fa17a-187">アプリをサーバーで直接実行するには、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-187">To directly run the app on the server:</span></span>

1. <span data-ttu-id="fa17a-188">アプリのディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-188">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="fa17a-189">アプリを実行する:`dotnet <app_assembly.dll>`。ここで、`app_assembly.dll` はアプリのアセンブリ ファイル名です。</span><span class="sxs-lookup"><span data-stu-id="fa17a-189">Run the app: `dotnet <app_assembly.dll>`, where `app_assembly.dll` is the assembly file name of the app.</span></span>

<span data-ttu-id="fa17a-190">アプリがサーバーで実行されているにも関わらず、インターネット上で応答がない場合、サーバーのファイアウォールを確認し、ポート 80 が開かれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-190">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="fa17a-191">Azure Ubuntu VM を使用している場合、受信ポート 80 のトラフィックを有効にするネットワーク セキュリティ グループ (NSG) 規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-191">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="fa17a-192">送信トラフィックは、受信規則が有効になると自動的に生成されるので、送信ポート 80 規則を有効にする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="fa17a-192">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="fa17a-193">アプリのテストが終了したら、コマンド プロンプトで `Ctrl+C` を使用してアプリをシャットダウンします。</span><span class="sxs-lookup"><span data-stu-id="fa17a-193">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="fa17a-194">アプリの監視</span><span class="sxs-lookup"><span data-stu-id="fa17a-194">Monitoring the app</span></span>

<span data-ttu-id="fa17a-195">サーバーは、`http://<serveraddress>:80` に対する要求を Kestrel で実行されている ASP.NET Core アプリ (`http://127.0.0.1:5000`) に転送するようにセットアップされました。</span><span class="sxs-lookup"><span data-stu-id="fa17a-195">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="fa17a-196">ただし、Nginx は Kestrel プロセスを管理するようには設定されていません。</span><span class="sxs-lookup"><span data-stu-id="fa17a-196">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="fa17a-197">*systemd* を使用してサービス ファイルを作成し、基になる Web アプリを起動して監視できます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-197">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="fa17a-198">*systemd* は init システムであり、プロセスを起動、停止、管理するためのさまざまな高性能機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-198">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="fa17a-199">サービス ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="fa17a-199">Create the service file</span></span>

<span data-ttu-id="fa17a-200">次のように、サービス定義ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-200">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="fa17a-201">アプリのサービス ファイルの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-201">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/www/helloapp
ExecStart=/usr/bin/dotnet /var/www/helloapp/helloapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="fa17a-202">ユーザー *www-data* が構成で使用されない場合、ここで定義するユーザーを先に作成し、ファイルの適切な所有権を与える必要があります。</span><span class="sxs-lookup"><span data-stu-id="fa17a-202">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="fa17a-203">アプリが最初の割り込み信号を受信してからシャットダウンするのを待機する期間を構成するには、`TimeoutStopSec` を使用します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-203">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="fa17a-204">この期間内にアプリがシャットダウンしない場合は、SIGKILL を発行してアプリを終了します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-204">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="fa17a-205">タイムアウトを無効にするには、値として、単位なしの秒数 (`150` など)、期間の値 (`2min 30s` など)、または `infinity` を指定します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-205">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="fa17a-206">`TimeoutStopSec` は、既定ではマネージャー構成ファイル (*systemd-system.conf*、*system.conf.d*、*systemd-user.conf*、*user.conf.d*) 内の `DefaultTimeoutStopSec` の値に設定されます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-206">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="fa17a-207">ほとんどのディストリビューションにおいて、タイムアウトの既定値は 90 秒となります。</span><span class="sxs-lookup"><span data-stu-id="fa17a-207">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="fa17a-208">Linux のファイル システムは大文字と小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-208">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="fa17a-209">ASPNETCORE_ENVIRONMENT を "Production" に設定すると、構成ファイル *appsettings.Production.json* が検索されます。*appsettings.production.json* ではありません。</span><span class="sxs-lookup"><span data-stu-id="fa17a-209">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="fa17a-210">構成プロバイダーが環境変数を読み取れるようにするために、一部の値 (たとえば SQL の接続文字列) をエスケープする必要があります。</span><span class="sxs-lookup"><span data-stu-id="fa17a-210">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="fa17a-211">次のコマンドを使用して、構成ファイルで使用するために適切にエスケープされた値を生成します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-211">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="fa17a-212">ファイルを保存し、サービスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="fa17a-212">Save the file and enable the service.</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="fa17a-213">サービスを起動し、動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-213">Start the service and verify that it's running.</span></span>

```
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

● kestrel-helloapp.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

<span data-ttu-id="fa17a-214">リバース プロキシが構成され、Kestrel は systemd 経由で管理されます。これで Web アプリは完全に構成され、`http://localhost` でローカル コンピューター上のブラウザーからアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-214">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="fa17a-215">妨げとなるファイアウォールがなければ、リモート コンピューターからもアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-215">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="fa17a-216">応答ヘッダーを調べると、ASP.NET Core アプリが Kestrel によってサービス提供されていることが `Server` ヘッダーに示されています。</span><span class="sxs-lookup"><span data-stu-id="fa17a-216">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="fa17a-217">ログを表示する</span><span class="sxs-lookup"><span data-stu-id="fa17a-217">Viewing logs</span></span>

<span data-ttu-id="fa17a-218">Kestrel を利用する Web アプリは `systemd` を使用して管理されるため、すべてのイベントとプロセスが記録され、中心的ジャーナルが生成されます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-218">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="fa17a-219">ただし、このジャーナルには、`systemd` が管理するすべてのサービスとプロセスのすべてのエントリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-219">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="fa17a-220">`kestrel-helloapp.service` 固有の項目を表示するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-220">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="fa17a-221">さらに絞り込むには、時間オプションとして `--since today`、`--until 1 hour ago`、あるいはこの 2 つの組み合わせを利用すれば、返されるエントリの数を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-221">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="fa17a-222">データの保護</span><span class="sxs-lookup"><span data-stu-id="fa17a-222">Data protection</span></span>

<span data-ttu-id="fa17a-223">[ASP.NET Core データ保護スタック](xref:security/data-protection/index)は、認証ミドルウェア (Cookie ミドルウェアなど) やクロスサイト リクエスト フォージェリ (CSRF) 保護を含む、いくつかの ASP.NET Core [ ミドルウェア](xref:fundamentals/middleware/index)で使用されます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-223">The [ASP.NET Core Data Protection stack](xref:security/data-protection/index) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="fa17a-224">データ保護 API がユーザーのコードから呼び出されない場合でも、永続的な暗号化[キー ストア](xref:security/data-protection/implementation/key-management)を作成するようにデータ保護を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fa17a-224">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="fa17a-225">データ保護を構成しない場合、既定でキーはメモリ内に保持され、アプリが再起動すると破棄されます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-225">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="fa17a-226">キーリングがメモリに格納されている場合、アプリを再起動すると次のことが行われます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-226">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="fa17a-227">すべての cookie ベースの認証トークンは無効になります。</span><span class="sxs-lookup"><span data-stu-id="fa17a-227">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="fa17a-228">ユーザーは、次回の要求時に再度サインインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="fa17a-228">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="fa17a-229">キーリングで保護されているデータは、いずれも復号化できなくなります。</span><span class="sxs-lookup"><span data-stu-id="fa17a-229">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="fa17a-230">これには、[CSRF トークン](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)と [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-230">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="fa17a-231">キー リングを永続化して暗号化するようにデータ保護を構成する場合は、次を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fa17a-231">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="securing-the-app"></a><span data-ttu-id="fa17a-232">アプリのセキュリティ保護</span><span class="sxs-lookup"><span data-stu-id="fa17a-232">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="fa17a-233">AppArmor を有効にする</span><span class="sxs-lookup"><span data-stu-id="fa17a-233">Enable AppArmor</span></span>

<span data-ttu-id="fa17a-234">Linux Security Modules (LSM) は、Linux 2.6 以降の Linux カーネルに含まれるフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="fa17a-234">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="fa17a-235">LSM は、セキュリティ モジュールのさまざまな実装に対応しています。</span><span class="sxs-lookup"><span data-stu-id="fa17a-235">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="fa17a-236">[AppArmor](https://wiki.ubuntu.com/AppArmor) は Mandatory Access Control システムを実装する LSM です。このシステムは、プログラムのリソース範囲を限定できます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-236">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="fa17a-237">AppArmor が有効であり、正しく構成されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-237">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="fa17a-238">ファイアウォールの構成</span><span class="sxs-lookup"><span data-stu-id="fa17a-238">Configuring the firewall</span></span>

<span data-ttu-id="fa17a-239">使用されていないすべての外部ポートを閉じます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-239">Close off all external ports that are not in use.</span></span> <span data-ttu-id="fa17a-240">ufw (uncomplicated firewall/複雑ではないファイアウォール) は `iptables` のフロント エンドとなり、ファイアウォールを構成するためのコマンド ライン インターフェイスを提供します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-240">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span>

> [!WARNING]
> <span data-ttu-id="fa17a-241">ファイアウォールが正しく構成されていない場合、システム全体にアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="fa17a-241">A firewall will prevent access to the whole system if not configured correctly.</span></span> <span data-ttu-id="fa17a-242">正しい SSH ポートを指定しないと、SSH を使用してシステムに接続する場合に、そのシステムから事実上閉め出されることになります。</span><span class="sxs-lookup"><span data-stu-id="fa17a-242">Failure to specify the correct SSH port will effectively lock you out of the system if you are using SSH to connect to it.</span></span> <span data-ttu-id="fa17a-243">既定のポートは 22 です。</span><span class="sxs-lookup"><span data-stu-id="fa17a-243">The default port is 22.</span></span> <span data-ttu-id="fa17a-244">詳細については、[ufw の概要](https://help.ubuntu.com/community/UFW)と[マニュアル](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fa17a-244">For more information, see the [introduction to ufw](https://help.ubuntu.com/community/UFW) and the [manual](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span></span>

<span data-ttu-id="fa17a-245">`ufw` をインストールし、必要なすべてのポートでトラフィックを許可するように構成します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-245">Install `ufw` and configure it to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

### <a name="securing-nginx"></a><span data-ttu-id="fa17a-246">Nginx のセキュリティを強化する</span><span class="sxs-lookup"><span data-stu-id="fa17a-246">Securing Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="fa17a-247">Nginx 応答名を変更する</span><span class="sxs-lookup"><span data-stu-id="fa17a-247">Change the Nginx response name</span></span>

<span data-ttu-id="fa17a-248">*src/http/ngx_http_header_filter_module.c* を編集します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-248">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="fa17a-249">構成オプション</span><span class="sxs-lookup"><span data-stu-id="fa17a-249">Configure options</span></span>

<span data-ttu-id="fa17a-250">必要なその他のモジュールでサーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-250">Configure the server with additional required modules.</span></span> <span data-ttu-id="fa17a-251">アプリのセキュリティを強化するために、[ModSecurity](https://www.modsecurity.org/) のような Web アプリのファイアウォールの使用を検討してください。</span><span class="sxs-lookup"><span data-stu-id="fa17a-251">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="configure-ssl"></a><span data-ttu-id="fa17a-252">SSL を構成する</span><span class="sxs-lookup"><span data-stu-id="fa17a-252">Configure SSL</span></span>

* <span data-ttu-id="fa17a-253">信頼できる証明機関 (CA) が発行した、有効な証明書を指定することで、ポート `443` で HTTPS トラフィックを待ち受けるようにサーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-253">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="fa17a-254">次の */etc/nginx/nginx.conf* ファイルで示されているプラクティスの一部を採用することで、セキュリティを強化します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-254">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="fa17a-255">たとえば、強力な暗号を選択したり、HTTP 経由のすべてのトラフィックを HTTPS にリダイレクトしたりします。</span><span class="sxs-lookup"><span data-stu-id="fa17a-255">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="fa17a-256">`HTTP Strict-Transport-Security` (HSTS) ヘッダーを追加すると、クライアントが行う後続のすべての要求が HTTPS 経由のみになります。</span><span class="sxs-lookup"><span data-stu-id="fa17a-256">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="fa17a-257">Strict-Transport-Security ヘッダーを追加しないでください。または、今後 SSL を利用できないことがあれば、適切な `max-age` を選択してください。</span><span class="sxs-lookup"><span data-stu-id="fa17a-257">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="fa17a-258">*/etc/nginx/proxy.conf* 構成ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-258">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="fa17a-259">*/etc/nginx/nginx.conf* 構成ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-259">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="fa17a-260">この例では、1 つの構成ファイルに `http` セクションと `server` セクションの両方が含まれています。</span><span class="sxs-lookup"><span data-stu-id="fa17a-260">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="fa17a-261">Nginx をクリックジャッキングから守る</span><span class="sxs-lookup"><span data-stu-id="fa17a-261">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="fa17a-262">クリックジャッキングは、感染したユーザーのクリックを集めるという悪意のある手法です。</span><span class="sxs-lookup"><span data-stu-id="fa17a-262">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="fa17a-263">クリックジャッキングは被害者 (訪問者) をだまし、感染したサイトでクリックさせます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-263">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="fa17a-264">X-FRAME-OPTIONS を使用して、サイトをセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-264">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="fa17a-265">*nginx.conf* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-265">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="fa17a-266">行 `add_header X-Frame-Options "SAMEORIGIN";` を追加し、ファイルを保存し、Nginx を再起動します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-266">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="fa17a-267">MIME タイプ スニッフィング</span><span class="sxs-lookup"><span data-stu-id="fa17a-267">MIME-type sniffing</span></span>

<span data-ttu-id="fa17a-268">このヘッダーは応答コンテンツの種類をオーバーライドしないようにブラウザーに指示するので、ほとんどのブラウザーで MIME スニッフィングが阻止されます。</span><span class="sxs-lookup"><span data-stu-id="fa17a-268">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="fa17a-269">`nosniff` オプションを指定すると、サーバーがコンテンツを "text/html" と読むとき、ブラウザーはそれを "text/html" として表示します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-269">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="fa17a-270">*nginx.conf* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-270">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="fa17a-271">行 `add_header X-Content-Type-Options "nosniff";` を追加し、ファイルを保存し、Nginx を再起動します。</span><span class="sxs-lookup"><span data-stu-id="fa17a-271">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fa17a-272">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="fa17a-272">Additional resources</span></span>

* [<span data-ttu-id="fa17a-273">Linux における .NET Core の前提条件</span><span class="sxs-lookup"><span data-stu-id="fa17a-273">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* [<span data-ttu-id="fa17a-274">Nginx: バイナリ リリース: 公式 Debian/Ubuntu パッケージ</span><span class="sxs-lookup"><span data-stu-id="fa17a-274">Nginx: Binary Releases: Official Debian/Ubuntu packages</span></span>](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* [<span data-ttu-id="fa17a-275">プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する</span><span class="sxs-lookup"><span data-stu-id="fa17a-275">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
* [<span data-ttu-id="fa17a-276">NGINX: 転送されるヘッダーの使用</span><span class="sxs-lookup"><span data-stu-id="fa17a-276">NGINX: Using the Forwarded header</span></span>](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
