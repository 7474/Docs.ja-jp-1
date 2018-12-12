---
title: ASP.NET Core の Web ホストおよび汎用ホスト
author: guardrex
description: アプリの起動と有効期間の管理を担当する、ASP.NET Core Web ホストと .NET 汎用ホストについて説明します。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 43a68f67368ce37a1fb4032579c6c4e4c05d2719
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284579"
---
# <a name="web-host-and-generic-host-in-aspnet-core"></a><span data-ttu-id="3db0e-103">ASP.NET Core の Web ホストおよび汎用ホスト</span><span class="sxs-lookup"><span data-stu-id="3db0e-103">Web Host and Generic Host in ASP.NET Core</span></span>

<span data-ttu-id="3db0e-104">.NET アプリは*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="3db0e-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="3db0e-105">ホストはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="3db0e-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="3db0e-106">次の 2 つのホスト API を使用できます。</span><span class="sxs-lookup"><span data-stu-id="3db0e-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="3db0e-107">[Web ホスト](xref:fundamentals/host/web-host) &ndash; Web アプリのホスティングに適しています。</span><span class="sxs-lookup"><span data-stu-id="3db0e-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="3db0e-108">[汎用ホスト](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 以降)&ndash; Web 以外のアプリ (バックグラウンド タスクを実行するアプリなど) のホスティングに適しています。</span><span class="sxs-lookup"><span data-stu-id="3db0e-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="3db0e-109">将来のリリースでは、汎用ホストが Web アプリを含むすべての種類のアプリのホスティングに適するようになります。</span><span class="sxs-lookup"><span data-stu-id="3db0e-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="3db0e-110">Web ホストは最終的に汎用ホストに置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="3db0e-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="3db0e-111">ASP.NET Core *Web アプリ*をホスティングする場合は、<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> に基づいた Web ホストを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3db0e-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span></span> <span data-ttu-id="3db0e-112">*Web アプリ以外*をホスティングする場合は、<xref:Microsoft.Extensions.Hosting.HostBuilder> に基づいた汎用ホストを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3db0e-112">For hosting *non-web apps*, developers should use the Generic Host based on <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span></span>

<xref:fundamentals/host/hosted-services>  
<span data-ttu-id="3db0e-113">ASP.NET Core でホステッド サービスを使用するバックグラウンド タスクの実装方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3db0e-113">Learn how to implement background tasks with hosted services in ASP.NET Core.</span></span>

<xref:fundamentals/configuration/platform-specific-configuration>  
<span data-ttu-id="3db0e-114"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 実装を使用して、参照されるアセンブリまたは参照されないアセンブリから ASP.NET Core アプリを拡張する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3db0e-114">Discover how to enhance an ASP.NET Core app from a referenced or unreferenced assembly using an <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation.</span></span>
