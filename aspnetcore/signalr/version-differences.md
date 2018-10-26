---
title: SignalR と ASP.NET Core SignalR の違い
author: tdykstra
description: SignalR と ASP.NET Core SignalR の違い
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 09/10/2018
uid: signalr/version-differences
ms.openlocfilehash: 4ac7952f26500285fc1c8f9453feb3ea8b33851a
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089830"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="977ee-103">ASP.NET SignalR、ASP.NET Core SignalR の相違点</span><span class="sxs-lookup"><span data-stu-id="977ee-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="977ee-104">ASP.NET Core SignalR は、クライアントまたは ASP.NET SignalR のサーバーとの互換性はありません。</span><span class="sxs-lookup"><span data-stu-id="977ee-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="977ee-105">この記事では、削除または ASP.NET Core SignalR で変更された機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="977ee-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="977ee-106">SignalR のバージョンを識別する方法</span><span class="sxs-lookup"><span data-stu-id="977ee-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="977ee-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="977ee-107">ASP.NET SignalR</span></span> | <span data-ttu-id="977ee-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="977ee-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="977ee-109">サーバーの NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="977ee-109">Server NuGet Package</span></span> | [<span data-ttu-id="977ee-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="977ee-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="977ee-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="977ee-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="977ee-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="977ee-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="977ee-113">クライアントの NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="977ee-113">Client NuGet Packages</span></span> | [<span data-ttu-id="977ee-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="977ee-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="977ee-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="977ee-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="977ee-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="977ee-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="977ee-117">クライアントの npm パッケージ</span><span class="sxs-lookup"><span data-stu-id="977ee-117">Client npm Package</span></span> | [<span data-ttu-id="977ee-118">signalr</span><span class="sxs-lookup"><span data-stu-id="977ee-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="977ee-119">サーバー アプリの種類</span><span class="sxs-lookup"><span data-stu-id="977ee-119">Server App Type</span></span> | <span data-ttu-id="977ee-120">ASP.NET (System.Web) または OWIN 自己ホスト</span><span class="sxs-lookup"><span data-stu-id="977ee-120">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="977ee-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="977ee-121">ASP.NET Core</span></span> |
| <span data-ttu-id="977ee-122">サーバーがサポートされているプラットフォーム</span><span class="sxs-lookup"><span data-stu-id="977ee-122">Supported Server Platforms</span></span> | <span data-ttu-id="977ee-123">.NET framework 4.5 またはそれ以降</span><span class="sxs-lookup"><span data-stu-id="977ee-123">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="977ee-124">.NET Framework 4.6.1 以降</span><span class="sxs-lookup"><span data-stu-id="977ee-124">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="977ee-125">.NET core 2.1 以降</span><span class="sxs-lookup"><span data-stu-id="977ee-125">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="977ee-126">機能の相違点</span><span class="sxs-lookup"><span data-stu-id="977ee-126">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="977ee-127">自動再接続</span><span class="sxs-lookup"><span data-stu-id="977ee-127">Automatic reconnects</span></span>

<span data-ttu-id="977ee-128">自動再接続は、ASP.NET Core SignalR でサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="977ee-128">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="977ee-129">クライアントが切断された場合は、再接続する場合は、ユーザーは明示的に新しい接続を開始する必要があります。</span><span class="sxs-lookup"><span data-stu-id="977ee-129">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="977ee-130">ASP.NET SignalR、SignalR は、接続が削除された場合、サーバーに再接続しようとします。</span><span class="sxs-lookup"><span data-stu-id="977ee-130">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

### <a name="protocol-support"></a><span data-ttu-id="977ee-131">プロトコルのサポート</span><span class="sxs-lookup"><span data-stu-id="977ee-131">Protocol support</span></span>

<span data-ttu-id="977ee-132">ASP.NET Core SignalR は、JSON、ほかに基づく新しいバイナリ プロトコルをサポート[MessagePack](xref:signalr/messagepackhubprotocol)します。</span><span class="sxs-lookup"><span data-stu-id="977ee-132">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="977ee-133">さらに、カスタム プロトコルを作成できます。</span><span class="sxs-lookup"><span data-stu-id="977ee-133">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="977ee-134">サーバー上の相違点</span><span class="sxs-lookup"><span data-stu-id="977ee-134">Differences on the server</span></span>

<span data-ttu-id="977ee-135">ASP.NET Core SignalR のサーバー側のライブラリに含まれる、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)パッケージの一部である、 **ASP.NET Core Web アプリケーション**Razor と MVC の両方のテンプレートプロジェクト。</span><span class="sxs-lookup"><span data-stu-id="977ee-135">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="977ee-136">呼び出すことによって構成する必要がありますので、ASP.NET Core SignalR は ASP.NET Core のミドルウェア、 [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)で`Startup.ConfigureServices`します。</span><span class="sxs-lookup"><span data-stu-id="977ee-136">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="977ee-137">ルーティングを構成するには、ハブ内にルートをマップ、 [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr)でメソッドを呼び出す、`Startup.Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="977ee-137">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="977ee-138">スティッキー セッションが必要になりました</span><span class="sxs-lookup"><span data-stu-id="977ee-138">Sticky sessions now required</span></span>

<span data-ttu-id="977ee-139">により、どのようにスケール アウトは、ASP.NET SignalR で作業した、クライアントは再接続し、ファーム内の任意のサーバーにメッセージを送信でした。</span><span class="sxs-lookup"><span data-stu-id="977ee-139">Because of how scale-out worked in ASP.NET SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="977ee-140">再接続をサポートしていないほか、スケール アウト モデルの変更によりこれがサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="977ee-140">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="977ee-141">クライアントは、サーバーに接続すると、接続の間の同じサーバーと対話する必要があります。</span><span class="sxs-lookup"><span data-stu-id="977ee-141">Once the client connects to the server, it must interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="977ee-142">接続ごとに 1 つのハブ</span><span class="sxs-lookup"><span data-stu-id="977ee-142">Single hub per connection</span></span>

<span data-ttu-id="977ee-143">ASP.NET Core signalr では、接続モデルを簡略化されました。</span><span class="sxs-lookup"><span data-stu-id="977ee-143">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="977ee-144">複数のハブへのアクセスを共有するために使用されている 1 つの接続ではなく、1 つのハブに直接接続が行われます。</span><span class="sxs-lookup"><span data-stu-id="977ee-144">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="977ee-145">ストリーム</span><span class="sxs-lookup"><span data-stu-id="977ee-145">Streaming</span></span>

<span data-ttu-id="977ee-146">ASP.NET Core SignalR なりました[ストリーミング データ](xref:signalr/streaming)クライアント ハブから。</span><span class="sxs-lookup"><span data-stu-id="977ee-146">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="977ee-147">状態</span><span class="sxs-lookup"><span data-stu-id="977ee-147">State</span></span>

<span data-ttu-id="977ee-148">進行状況メッセージのサポートと、クライアントと (HubState と呼ばれる多くの場合)、ハブ間で任意の状態を渡す機能が削除されました。</span><span class="sxs-lookup"><span data-stu-id="977ee-148">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="977ee-149">現時点では、ハブ プロキシの対応はありません。</span><span class="sxs-lookup"><span data-stu-id="977ee-149">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="977ee-150">クライアント上の相違点</span><span class="sxs-lookup"><span data-stu-id="977ee-150">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="977ee-151">TypeScript</span><span class="sxs-lookup"><span data-stu-id="977ee-151">TypeScript</span></span>

<span data-ttu-id="977ee-152">ASP.NET Core SignalR クライアントがで記述された[TypeScript](https://www.typescriptlang.org/)します。</span><span class="sxs-lookup"><span data-stu-id="977ee-152">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="977ee-153">使用する場合は、JavaScript または TypeScript で記述することができます、 [JavaScript クライアント](xref:signalr/javascript-client)します。</span><span class="sxs-lookup"><span data-stu-id="977ee-153">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="977ee-154">ホストされている JavaScript クライアント[npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="977ee-154">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="977ee-155">以前のバージョンで、JavaScript クライアントは、Visual Studio で NuGet パッケージから取得されました。</span><span class="sxs-lookup"><span data-stu-id="977ee-155">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="977ee-156">Core のバージョン、 [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) npm パッケージには、JavaScript ライブラリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="977ee-156">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="977ee-157">このパッケージに含まれていない、 **ASP.NET Core Web アプリケーション**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="977ee-157">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="977ee-158">Npm を入手してインストールを使用して、 `@aspnet/signalr` npm パッケージ。</span><span class="sxs-lookup"><span data-stu-id="977ee-158">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="977ee-159">jQuery</span><span class="sxs-lookup"><span data-stu-id="977ee-159">jQuery</span></span>

<span data-ttu-id="977ee-160">JQuery に依存関係は削除されたが、プロジェクトは、jQuery を引き続き使用できます。</span><span class="sxs-lookup"><span data-stu-id="977ee-160">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="977ee-161">JavaScript クライアント メソッドの構文</span><span class="sxs-lookup"><span data-stu-id="977ee-161">JavaScript client method syntax</span></span>

<span data-ttu-id="977ee-162">JavaScript 構文は、SignalR の以前のバージョンから変更されました。</span><span class="sxs-lookup"><span data-stu-id="977ee-162">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="977ee-163">使用してではなく、`$connection`オブジェクトを使用して接続を作成、 [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API。</span><span class="sxs-lookup"><span data-stu-id="977ee-163">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="977ee-164">使用して、[で](/javascript/api/@aspnet/signalr/HubConnection#on)ハブで呼び出すことができるクライアント メソッドを指定します。</span><span class="sxs-lookup"><span data-stu-id="977ee-164">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="977ee-165">クライアントのメソッドを作成した後、ハブの接続を開始します。</span><span class="sxs-lookup"><span data-stu-id="977ee-165">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="977ee-166">チェーンを[キャッチ](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)ログまたはエラーを処理するメソッド。</span><span class="sxs-lookup"><span data-stu-id="977ee-166">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="977ee-167">ハブ プロキシ</span><span class="sxs-lookup"><span data-stu-id="977ee-167">Hub proxies</span></span>

<span data-ttu-id="977ee-168">ハブ プロキシが自動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="977ee-168">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="977ee-169">代わりに、メソッド名に渡される、[呼び出す](/javascript/api/%40aspnet/signalr/hubconnection#invoke)を文字列としての API。</span><span class="sxs-lookup"><span data-stu-id="977ee-169">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="977ee-170">.NET およびその他のクライアント</span><span class="sxs-lookup"><span data-stu-id="977ee-170">.NET and other clients</span></span>

<span data-ttu-id="977ee-171">`Microsoft.AspNetCore.SignalR.Client` NuGet パッケージには、ASP.NET Core SignalR の .NET クライアント ライブラリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="977ee-171">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="977ee-172">使用して、 [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)作成し、ハブへの接続のインスタンスを構築します。</span><span class="sxs-lookup"><span data-stu-id="977ee-172">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="977ee-173">スケール アウトの違い</span><span class="sxs-lookup"><span data-stu-id="977ee-173">Scaleout differences</span></span>

<span data-ttu-id="977ee-174">ASP.NET SignalR は、SQL Server と Redis をサポートします。</span><span class="sxs-lookup"><span data-stu-id="977ee-174">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="977ee-175">ASP.NET Core SignalR は、Azure SignalR サービスと Redis をサポートします。</span><span class="sxs-lookup"><span data-stu-id="977ee-175">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="977ee-176">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="977ee-176">ASP.NET</span></span>

* [<span data-ttu-id="977ee-177">Azure Service Bus による SignalR スケール アウト</span><span class="sxs-lookup"><span data-stu-id="977ee-177">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="977ee-178">Redis による SignalR スケール アウト</span><span class="sxs-lookup"><span data-stu-id="977ee-178">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="977ee-179">SQL Server による SignalR スケール アウト</span><span class="sxs-lookup"><span data-stu-id="977ee-179">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="977ee-180">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="977ee-180">ASP.NET Core</span></span>

* [<span data-ttu-id="977ee-181">Azure SignalR サービス</span><span class="sxs-lookup"><span data-stu-id="977ee-181">Azure SignalR Service</span></span>](/azure/azure-signalr/)

## <a name="additional-resources"></a><span data-ttu-id="977ee-182">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="977ee-182">Additional resources</span></span>

* [<span data-ttu-id="977ee-183">ハブ</span><span class="sxs-lookup"><span data-stu-id="977ee-183">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="977ee-184">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="977ee-184">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="977ee-185">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="977ee-185">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="977ee-186">サポートされているプラットフォーム</span><span class="sxs-lookup"><span data-stu-id="977ee-186">Supported platforms</span></span>](xref:signalr/supported-platforms)
