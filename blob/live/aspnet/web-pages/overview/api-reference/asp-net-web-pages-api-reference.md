---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: "ASP.NET Web Pages (Razor) API クイック リファレンス |Microsoft ドキュメント"
author: tfitzmac
description: "このページには、最もよく使用されるオブジェクト、プロパティ、および ASP.NET Web ページ Razor 構文を使用したプログラミング方法の簡単な例を含む一覧が含まれています。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: 35f91f4dbea4881d9dabc4ab7c6b96dbb6a01ea2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a><span data-ttu-id="cb5eb-103">ASP.NET Web Pages (Razor) API クイック リファレンス</span><span class="sxs-lookup"><span data-stu-id="cb5eb-103">ASP.NET Web Pages (Razor) API Quick Reference</span></span>
====================
<span data-ttu-id="cb5eb-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="cb5eb-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="cb5eb-105">このページには、最もよく使用されるオブジェクト、プロパティ、および ASP.NET Web ページ Razor 構文を使用したプログラミング方法の簡単な例を含む一覧が含まれています。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-105">This page contains a list with brief examples of the most commonly used objects, properties, and methods for programming ASP.NET Web Pages with Razor syntax.</span></span>
> 
> <span data-ttu-id="cb5eb-106">説明"(v2)"とマークされている ASP.NET Web Pages 2 のバージョンで導入されました。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-106">Descriptions marked with "(v2)" were introduced in ASP.NET Web Pages version 2.</span></span>
> 
> <span data-ttu-id="cb5eb-107">API リファレンス ドキュメントを参照してください、 [ASP.NET Web Pages のリファレンス ドキュメント](https://go.microsoft.com/fwlink/?LinkId=208659)msdn です。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-107">For API reference documentation, see the [ASP.NET Web Pages Reference Documentation](https://go.microsoft.com/fwlink/?LinkId=208659) on MSDN.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="cb5eb-108">ソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="cb5eb-108">Software versions</span></span>
> 
> 
> - <span data-ttu-id="cb5eb-109">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="cb5eb-109">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="cb5eb-110">このチュートリアルは、(v2 マークされている機能) を除く ASP.NET Web Pages 2 と ASP.NET Web Pages 1.0 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-110">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0 (except for features marked v2).</span></span>


<span data-ttu-id="cb5eb-111">このページには、次の参照情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-111">This page contains reference information for the following:</span></span>

- [<span data-ttu-id="cb5eb-112">クラス</span><span class="sxs-lookup"><span data-stu-id="cb5eb-112">Classes</span></span>](#Classes)
- [<span data-ttu-id="cb5eb-113">データ</span><span class="sxs-lookup"><span data-stu-id="cb5eb-113">Data</span></span>](#Data)
- [<span data-ttu-id="cb5eb-114">ヘルパー</span><span class="sxs-lookup"><span data-stu-id="cb5eb-114">Helpers</span></span>](#Helpers)
- [<span data-ttu-id="cb5eb-115">検証</span><span class="sxs-lookup"><span data-stu-id="cb5eb-115">Validation</span></span>](#Validation)

<a id="Classes"></a>
## <a name="classes"></a><span data-ttu-id="cb5eb-116">クラス</span><span class="sxs-lookup"><span data-stu-id="cb5eb-116">Classes</span></span>

### `AppState[key], AppState[index],App`

<span data-ttu-id="cb5eb-117">アプリケーション内のすべてのページで共有できるデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-117">Contains data that can be shared by any pages in the application.</span></span> <span data-ttu-id="cb5eb-118">動的を使用する`App`プロパティを次の例のように、同じデータにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-118">You can use the dynamic `App` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

<span data-ttu-id="cb5eb-119">文字列値をブール値 (true または false) に変換します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-119">Converts a string value to a Boolean value (true/false).</span></span> <span data-ttu-id="cb5eb-120">False を返します。 または、文字列が true または false を表していない場合は、指定された値。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-120">Returns false or the specified value if the string does not represent true/false.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

<span data-ttu-id="cb5eb-121">日付/時刻を文字列値に変換します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-121">Converts a string value to date/time.</span></span> <span data-ttu-id="cb5eb-122">返します`DateTime.MinValue`または文字列が日付/時刻を表していない場合は、指定された値。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-122">Returns `DateTime.MinValue` or the specified value if the string does not represent a date/time.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

<span data-ttu-id="cb5eb-123">文字列値を 10 進値に変換します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-123">Converts a string value to a decimal value.</span></span> <span data-ttu-id="cb5eb-124">指定した値文字列は 10 進数値を表していない場合または 0.0 を返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-124">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

<span data-ttu-id="cb5eb-125">文字列値を float に変換します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-125">Converts a string value to a float.</span></span> <span data-ttu-id="cb5eb-126">指定した値文字列は 10 進数値を表していない場合または 0.0 を返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-126">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

<span data-ttu-id="cb5eb-127">文字列値を整数に変換します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-127">Converts a string value to an integer.</span></span> <span data-ttu-id="cb5eb-128">文字列は整数値を表していない場合は、指定した値または 0 を返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-128">Returns 0 or the specified value if the string does not represent an integer.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

<span data-ttu-id="cb5eb-129">省略可能な追加のパス部分とのローカル ファイル パスから、ブラウザーと互換性のある URL を作成します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-129">Creates a browser-compatible URL from a local file path, with optional additional path parts.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

<span data-ttu-id="cb5eb-130">レンダリング*値*に HTML エンコードされた出力をレンダリングするのではなく HTML マークアップとして。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-130">Renders *value* as HTML markup instead of rendering it as HTML-encoded output.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

<span data-ttu-id="cb5eb-131">値は、文字列から指定された型に変換できる場合は true を返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-131">Returns true if the value can be converted from a string to the specified type.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

<span data-ttu-id="cb5eb-132">オブジェクトまたは変数に値があるない場合は true を返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-132">Returns true if the object or variable has no value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

<span data-ttu-id="cb5eb-133">要求が POST である場合は true を返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-133">Returns true if the request is a POST.</span></span> <span data-ttu-id="cb5eb-134">(最初の要求には通常、GET) です。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-134">(Initial requests are usually a GET.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

<span data-ttu-id="cb5eb-135">このページに適用するレイアウト ページのパスを指定します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-135">Specifies the path of a layout page to apply to this page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

<span data-ttu-id="cb5eb-136">現在の要求のページ、レイアウト ページ、および部分ページ間で共有データが含まれています。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-136">Contains data shared between the page, layout pages, and partial pages in the current request.</span></span> <span data-ttu-id="cb5eb-137">動的を使用する`Page`プロパティを次の例のように、同じデータにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-137">You can use the dynamic `Page` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

<span data-ttu-id="cb5eb-138">(レイアウト ページ)任意の名前付きセクションに含まれていないコンテンツ ページのコンテンツをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-138">(Layout pages) Renders the content of a content page that is not in any named sections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

<span data-ttu-id="cb5eb-139">指定したパスと省略可能な追加データを使用し、コンテンツ ページを表示します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-139">Renders a content page using the specified path and optional extra data.</span></span> <span data-ttu-id="cb5eb-140">余分なパラメーターの値を取得できる`PageData`位置 (例: 1) またはキー (例: 2) 使用しています。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-140">You can get the values of the extra parameters from `PageData` by position (example 1) or key (example 2).</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

<span data-ttu-id="cb5eb-141">(レイアウト ページ)名前を持つコンテンツ セクションを表示します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-141">(Layout pages) Renders a content section that has a name.</span></span> <span data-ttu-id="cb5eb-142">設定*必要*が false にセクションを省略可能です。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-142">Set *required* to false to make a section optional.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

<span data-ttu-id="cb5eb-143">取得または HTTP クッキーの値を設定します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-143">Gets or sets the value of an HTTP cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

<span data-ttu-id="cb5eb-144">現在の要求でアップロードされたファイルを取得します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-144">Gets the files that were uploaded in the current request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

<span data-ttu-id="cb5eb-145">(文字列) として、フォームがポストされたデータを取得します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-145">Gets data that was posted in a form (as strings).</span></span> <span data-ttu-id="cb5eb-146">`Request[key]`両方のチェック、`Request.Form`と`Request.QueryString`コレクション。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-146">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

<span data-ttu-id="cb5eb-147">URL クエリ文字列で指定されているデータを取得します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-147">Gets data that was specified in the URL query string.</span></span> <span data-ttu-id="cb5eb-148">`Request[key]`両方のチェック、`Request.Form`と`Request.QueryString`コレクション。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-148">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

<span data-ttu-id="cb5eb-149">選択的に無効には、form 要素は、クエリ文字列値、cookie、またはヘッダーの値の検証を要求します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-149">Selectively disables request validation for a form element, query-string value, cookie, or header value.</span></span> <span data-ttu-id="cb5eb-150">要求の検証は、既定で有効にし、ユーザーがマークアップやその他の危険性のあるコンテンツを送信するを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-150">Request validation is enabled by default and prevents users from posting markup or other potentially dangerous content.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

<span data-ttu-id="cb5eb-151">HTTP サーバーのヘッダーを応答に追加します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-151">Adds an HTTP server header to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

<span data-ttu-id="cb5eb-152">指定した時間、ページの出力をキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-152">Caches the page output for a specified time.</span></span> <span data-ttu-id="cb5eb-153">必要に応じて設定*スライディング*を各ページにアクセスでのタイムアウトをリセットして*varyByParams*ページ要求内の各別のクエリ文字列のページの異なるバージョンをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-153">Optionally set *sliding* to reset the timeout on each page access and *varyByParams* to cache different versions of the page for each different query string in the page request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

<span data-ttu-id="cb5eb-154">ブラウザー要求を新しい場所にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-154">Redirects the browser request to a new location.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

<span data-ttu-id="cb5eb-155">ブラウザーに送信する HTTP ステータス コードを設定します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-155">Sets the HTTP status code sent to the browser.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

<span data-ttu-id="cb5eb-156">内容を書き込みます*データ*省略可能な MIME の種類の応答にします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-156">Writes the contents of *data* to the response with an optional MIME type.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

<span data-ttu-id="cb5eb-157">ファイルの内容を応答に書き込みます。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-157">Writes the contents of a file to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

<span data-ttu-id="cb5eb-158">(レイアウト ページ)名前を持つコンテンツ セクションを定義します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-158">(Layout pages) Defines a content section that has a name.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

<span data-ttu-id="cb5eb-159">Html エンコードする文字列をデコードします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-159">Decodes a string that is HTML encoded.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

<span data-ttu-id="cb5eb-160">HTML マークアップで表示するための文字列をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-160">Encodes a string for rendering in HTML markup.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

<span data-ttu-id="cb5eb-161">指定した仮想パスのサーバーの物理パスを返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-161">Returns the server physical path for the specified virtual path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

<span data-ttu-id="cb5eb-162">URL からテキストをデコードします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-162">Decodes text from a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

<span data-ttu-id="cb5eb-163">URL に格納するテキストをエンコードします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-163">Encodes text to put in a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

<span data-ttu-id="cb5eb-164">取得またはユーザーがブラウザーを閉じるまでに存在する値を設定します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-164">Gets or sets a value that exists until the user closes the browser.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

<span data-ttu-id="cb5eb-165">オブジェクトの値の文字列表現を表示します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-165">Displays a string representation of the object's value.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

<span data-ttu-id="cb5eb-166">URL から取得された追加データ (たとえば、 *MyPage/ExtraData*)。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-166">Gets additional data from the URL (for example, */MyPage/ExtraData*).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

<span data-ttu-id="cb5eb-167">指定したユーザーのパスワードを変更します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-167">Changes the password for the specified user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

<span data-ttu-id="cb5eb-168">アカウント確認トークンを使用して、アカウントを確認します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-168">Confirms an account using the account confirmation token.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

<span data-ttu-id="cb5eb-169">指定されたユーザー名とパスワードを持つ新しいユーザー アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-169">Creates a new user account with the specified user name and password.</span></span> <span data-ttu-id="cb5eb-170">確認トークンを要求するに true を渡します*requireConfirmationToken です。*</span><span class="sxs-lookup"><span data-stu-id="cb5eb-170">To require a confirmation token, pass true for *requireConfirmationToken.*</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

<span data-ttu-id="cb5eb-171">現在のログイン ユーザーの整数識別子を取得します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-171">Gets the integer identifier for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

<span data-ttu-id="cb5eb-172">現在のログイン ユーザーの名前を取得します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-172">Gets the name for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

<span data-ttu-id="cb5eb-173">送信できる電子メールでユーザーに、ユーザーがパスワードをリセットできるようにパスワード リセット トークンを生成します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-173">Generates a password-reset token that can be sent in email to a user so that the user can reset the password.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

<span data-ttu-id="cb5eb-174">ユーザー名とユーザー ID を返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-174">Returns the user ID from the user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

<span data-ttu-id="cb5eb-175">現在のユーザーがログオンしている場合に true を返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-175">Returns true if the current user is logged in.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

<span data-ttu-id="cb5eb-176">(たとえば、確認の電子メール) を通じて、ユーザーが確認されている場合は true を返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-176">Returns true if the user has been confirmed (for example, through a confirmation email).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

<span data-ttu-id="cb5eb-177">現在のユーザーの名前が指定されたユーザー名と一致する場合は true を返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-177">Returns true if the current user's name matches the specified user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

<span data-ttu-id="cb5eb-178">内のユーザーは、cookie に認証トークンを設定してログに記録します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-178">Logs the user in by setting an authentication token in the cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

<span data-ttu-id="cb5eb-179">認証トークンのクッキーを削除することで、ユーザーをサインアウト ログオンします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-179">Logs the user out by removing the authentication token cookie.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

<span data-ttu-id="cb5eb-180">ユーザーが認証されていない場合、HTTP ステータスを 401 (Unauthorized) に設定します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-180">If the user is not authenticated, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

<span data-ttu-id="cb5eb-181">現在のユーザーが指定されたロールのいずれかのメンバーでない場合、HTTP ステータスを 401 (Unauthorized) に設定します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-181">If the current user is not a member of one of the specified roles, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

<span data-ttu-id="cb5eb-182">現在のユーザーがで指定されたユーザーではないかどうか*username*、HTTP ステータス 401 (Unauthorized) を設定します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-182">If the current user is not the user specified by *username*, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

<span data-ttu-id="cb5eb-183">パスワード リセット トークンが有効な場合は、新しいパスワードにユーザーのパスワードを変更します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-183">If the password reset token is valid, changes the user's password to the new password.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a><span data-ttu-id="cb5eb-184">データ</span><span class="sxs-lookup"><span data-stu-id="cb5eb-184">Data</span></span>

### `Database.Execute(SQLstatement [,parameters]`

<span data-ttu-id="cb5eb-185">実行*SQLstatement* (省略可能なパラメーター) の INSERT、DELETE、または UPDATE などの影響を受けるレコードの数を返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-185">Executes *SQLstatement* (with optional parameters) such as INSERT, DELETE, or UPDATE and returns a count of affected records.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

<span data-ttu-id="cb5eb-186">最近挿入された行から id 列を返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-186">Returns the identity column from the most recently inserted row.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

<span data-ttu-id="cb5eb-187">指定されたデータベース ファイルまたは名前付き接続文字列を使用して指定されたデータベースのいずれかを開き、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-187">Opens either the specified database file or the database specified using a named connection string from the *Web.config* file.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

<span data-ttu-id="cb5eb-188">接続文字列を使用してデータベースを開きます。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-188">Opens a database using the connection string.</span></span> <span data-ttu-id="cb5eb-189">(これとは対照的`Database.Open`、接続文字列名を使用します)。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-189">(This contrasts with `Database.Open`, which uses a connection string name.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

<span data-ttu-id="cb5eb-190">使用してデータベースを照会*SQLstatement* (必要に応じてパラメーターを渡す) し、結果をコレクションとして返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-190">Queries the database using *SQLstatement* (optionally passing parameters) and returns the results as a collection.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

<span data-ttu-id="cb5eb-191">実行*SQLstatement* (省略可能なパラメーターは) を使用し、1 つのレコードを返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-191">Executes *SQLstatement* (with optional parameters) and returns a single record.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

<span data-ttu-id="cb5eb-192">実行*SQLstatement* (省略可能なパラメーターは) を使用し、1 つの値を返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-192">Executes *SQLstatement* (with optional parameters) and returns a single value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a><span data-ttu-id="cb5eb-193">ヘルパー</span><span class="sxs-lookup"><span data-stu-id="cb5eb-193">Helpers</span></span>

### `Analytics.GetGoogleHtml(webPropertyId)`

<span data-ttu-id="cb5eb-194">指定した ID の Google Analytics JavaScript コードを表示します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-194">Renders the Google Analytics JavaScript code for the specified ID.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

<span data-ttu-id="cb5eb-195">指定されたプロジェクトの StatCounter Analytics JavaScript コードを表示します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-195">Renders the StatCounter Analytics JavaScript code for the specified project.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

<span data-ttu-id="cb5eb-196">指定されたアカウント用の Yahoo Analytics JavaScript コードを表示します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-196">Renders the Yahoo Analytics JavaScript code for the specified account.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

<span data-ttu-id="cb5eb-197">検索を Bing に渡します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-197">Passes a search to Bing.</span></span> <span data-ttu-id="cb5eb-198">設定することができます、サイトを検索し、検索ボックスのタイトルを指定する、`Bing.SiteUrl`と`Bing.SiteTitle`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-198">To specify the site to search and a title for the search box, you can set the `Bing.SiteUrl` and `Bing.SiteTitle` properties.</span></span> <span data-ttu-id="cb5eb-199">これらのプロパティを設定する通常、  *\_AppStart*ページ。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-199">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

<span data-ttu-id="cb5eb-200">グラフを初期化します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-200">Initializes a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

<span data-ttu-id="cb5eb-201">グラフに凡例を追加します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-201">Adds a legend to a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

<span data-ttu-id="cb5eb-202">グラフには、一連の値を追加します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-202">Adds a series of values to the chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

<span data-ttu-id="cb5eb-203">指定されたデータのハッシュを返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-203">Returns a hash for the specified data.</span></span> <span data-ttu-id="cb5eb-204">既定のアルゴリズムは`sha256`します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-204">The default algorithm is `sha256`.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

<span data-ttu-id="cb5eb-205">Facebook ユーザー ページへの接続を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-205">Lets Facebook users make a connection to pages.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

<span data-ttu-id="cb5eb-206">ファイルをアップロードするには、UI をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-206">Renders UI for uploading files.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

<span data-ttu-id="cb5eb-207">指定した Xbox ゲーマー タグをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-207">Renders the specified Xbox gamer tag.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

<span data-ttu-id="cb5eb-208">指定された電子メール アドレスの Gravatar イメージをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-208">Renders the Gravatar image for the specified email address.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

<span data-ttu-id="cb5eb-209">データ オブジェクトを JavaScript Object Notation (JSON) 形式の文字列に変換します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-209">Converts a data object to a string in the JavaScript Object Notation (JSON) format.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

<span data-ttu-id="cb5eb-210">JSON でエンコードされた入力文字列を反復処理したり、データベースに挿入できるデータ オブジェクトに変換します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-210">Converts a JSON-encoded input string to a data object that you can iterate over or insert into a database.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

<span data-ttu-id="cb5eb-211">指定されたタイトルと省略可能な URL を使用して、ソーシャル ネットワーク リンクを表示します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-211">Renders social networking links using the specified title and optional URL.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

<span data-ttu-id="cb5eb-212">エラー メッセージをフォーム フィールドに関連付けます。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-212">Associates an error message with a form field.</span></span> <span data-ttu-id="cb5eb-213">使用して、`ModelState`このメンバーにアクセスするためのヘルパー。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-213">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

<span data-ttu-id="cb5eb-214">エラー メッセージをフォームに関連付けます。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-214">Associates an error message with a form.</span></span> <span data-ttu-id="cb5eb-215">使用して、`ModelState`このメンバーにアクセスするためのヘルパー。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-215">Use the `ModelState` helper to access this member.</span></span>

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

<span data-ttu-id="cb5eb-216">検証エラーがない場合は true を返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-216">Returns true if there are no validation errors.</span></span> <span data-ttu-id="cb5eb-217">使用して、`ModelState`このメンバーにアクセスするためのヘルパー。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-217">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

<span data-ttu-id="cb5eb-218">オブジェクト プロパティと、オブジェクトとその子の値を表示します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-218">Renders the properties and values of an object and any child objects.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

<span data-ttu-id="cb5eb-219">ReCAPTCHA 検証テストを表示します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-219">Renders the reCAPTCHA verification test.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

<span data-ttu-id="cb5eb-220">ReCAPTCHA サービスのパブリックおよびプライベートのキーを設定します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-220">Sets public and private keys for the reCAPTCHA service.</span></span> <span data-ttu-id="cb5eb-221">これらのプロパティを設定する通常、  *\_AppStart*ページ。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-221">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

<span data-ttu-id="cb5eb-222">ReCAPTCHA テストの結果を返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-222">Returns the result of the reCAPTCHA test.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

<span data-ttu-id="cb5eb-223">状態情報については、ASP.NET Web ページを表示します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-223">Renders status information about ASP.NET Web Pages.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

<span data-ttu-id="cb5eb-224">指定したユーザーの Twitter のストリームを表示します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-224">Renders a Twitter stream for the specified user.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

<span data-ttu-id="cb5eb-225">指定された検索テキストの Twitter のストリームを表示します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-225">Renders a Twitter stream for the specified search text.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

<span data-ttu-id="cb5eb-226">省略可能な幅と高さで指定したファイルのフラッシュ ビデオ プレーヤーをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-226">Renders a Flash video player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

<span data-ttu-id="cb5eb-227">省略可能な幅と高さで指定したファイルの Windows Media player をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-227">Renders a Windows Media player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

<span data-ttu-id="cb5eb-228">指定された Silverlight プレーヤーをレンダリング*.xap*必要な幅と高さを持つファイルです。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-228">Renders a Silverlight player for the specified *.xap* file with required width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

<span data-ttu-id="cb5eb-229">指定されたオブジェクトを返します*キー*、またはオブジェクトが見つからない場合は null です。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-229">Returns the object specified by *key*, or null if the object is not found.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

<span data-ttu-id="cb5eb-230">指定されたオブジェクトを削除*キー*キャッシュからです。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-230">Removes the object specified by *key* from the cache.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

<span data-ttu-id="cb5eb-231">配置*値*で指定された名前でキャッシュに*キー*です。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-231">Puts *value* into the cache under the name specified by *key*.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

<span data-ttu-id="cb5eb-232">新たに作成`WebGrid`オブジェクト クエリからのデータを使用します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-232">Creates a new `WebGrid` object using data from a query.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

<span data-ttu-id="cb5eb-233">HTML テーブルのデータを表示するマークアップを表示します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-233">Renders markup to display data in an HTML table.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

<span data-ttu-id="cb5eb-234">レンダリングのポケットベル、`WebGrid`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-234">Renders a pager for the `WebGrid` object.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

<span data-ttu-id="cb5eb-235">指定されたパスからイメージを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-235">Loads an image from the specified path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

<span data-ttu-id="cb5eb-236">ウォーターマークとして、指定されたイメージを追加します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-236">Adds the specified image as a watermark.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

<span data-ttu-id="cb5eb-237">イメージを指定したテキストを追加します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-237">Adds the specified text to the image.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

<span data-ttu-id="cb5eb-238">水平方向または垂直方向には、イメージを反転します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-238">Flips the image horizontally or vertically.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

<span data-ttu-id="cb5eb-239">ファイルのアップロード中にイメージがページにポストされたときに、イメージを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-239">Loads an image when an image is posted to a page during a file upload.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

<span data-ttu-id="cb5eb-240">サイズを変更する画像。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-240">Resizes an the image.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

<span data-ttu-id="cb5eb-241">左または右に画像を回転します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-241">Rotates the image to the left or the right.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

<span data-ttu-id="cb5eb-242">指定されたパスにイメージを保存します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-242">Saves the image to the specified path.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

<span data-ttu-id="cb5eb-243">SMTP サーバーのパスワードを設定します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-243">Sets the password for the SMTP server.</span></span> <span data-ttu-id="cb5eb-244">このプロパティを設定する通常、  *\_AppStart*ページ。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-244">Normally you set this property in the *\_AppStart* page.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

<span data-ttu-id="cb5eb-245">電子メール メッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-245">Sends an email message.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

<span data-ttu-id="cb5eb-246">SMTP サーバー名を設定します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-246">Sets the SMTP server name.</span></span> <span data-ttu-id="cb5eb-247">このプロパティを設定する通常、*\_AppStart*ページ。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-247">Normally you set this property in the*\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

<span data-ttu-id="cb5eb-248">SMTP サーバーのユーザー名を設定します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-248">Sets the user name for the SMTP server.</span></span> <span data-ttu-id="cb5eb-249">通常このプロパティを設定する必要があります、  *\_AppStart*ページ。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-249">Normally you should set this property in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a><span data-ttu-id="cb5eb-250">検証</span><span class="sxs-lookup"><span data-stu-id="cb5eb-250">Validation</span></span>

### `Html.ValidationMessage(field)`

<span data-ttu-id="cb5eb-251">(v2)指定したフィールドの検証エラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-251">(v2) Renders a validation error message for the specified field.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

<span data-ttu-id="cb5eb-252">(v2)すべての検証エラーの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-252">(v2) Displays a list of all validation errors.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

<span data-ttu-id="cb5eb-253">(v2)検証の指定した型のユーザー入力要素を登録します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-253">(v2) Registers a user input element for the specified type of validation.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

<span data-ttu-id="cb5eb-254">(v2)検証エラー メッセージをフォーマットすることができるように、クライアント側の検証の CSS クラス属性を動的にレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-254">(v2) Dynamically renders CSS class attributes for client-side validation so that you can format validation error messages.</span></span> <span data-ttu-id="cb5eb-255">(適切なクライアント スクリプト ライブラリを参照していると、CSS クラスを定義することが必要)。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-255">(Requires that you reference the appropriate client-script libraries and that you define CSS classes.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

<span data-ttu-id="cb5eb-256">(v2)ユーザーの入力フィールドのクライアント側の検証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-256">(v2) Enables client-side validation for the user input field.</span></span> <span data-ttu-id="cb5eb-257">(適切なクライアント スクリプト ライブラリを参照している必要があります)。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-257">(Requires that you reference the appropriate client-script libraries.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

<span data-ttu-id="cb5eb-258">(v2)検証のための登録は、すべてのユーザー入力要素には、有効な値が含まれている場合に true を返します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-258">(v2) Returns true if all user input elements that are registred for validation contain valid values.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

<span data-ttu-id="cb5eb-259">(v2)ユーザーが、ユーザー入力要素の値を指定する必要がありますを指定します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-259">(v2) Specifies that users must provide a value for the user input element.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

<span data-ttu-id="cb5eb-260">(v2)ユーザーがユーザー入力要素の各値を指定する必要がありますを指定します。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-260">(v2) Specifies that users must provide values for each of the user input elements.</span></span> <span data-ttu-id="cb5eb-261">このメソッドでは、カスタム エラー メッセージを指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-261">This method does not let you specify a custom error message.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

<span data-ttu-id="cb5eb-262">(v2)使用すると、検証テストを指定、`Validation.Add`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="cb5eb-262">(v2) Specifies a validation test when you use the `Validation.Add` method.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]