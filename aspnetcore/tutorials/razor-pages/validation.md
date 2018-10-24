---
title: ASP.NET Core Razor ページに検証を追加する
author: rick-anderson
description: ASP.NET Core で Razor ページに検証を追加する方法について説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/validation
ms.openlocfilehash: cd958b9c084de4b3e12784774544610873a519f9
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045524"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a><span data-ttu-id="09013-103">ASP.NET Core Razor ページに検証を追加する</span><span class="sxs-lookup"><span data-stu-id="09013-103">Add validation to an ASP.NET Core Razor Page</span></span>

<span data-ttu-id="09013-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="09013-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="09013-105">ここでは、`Movie` モデルに検証ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="09013-105">In this section, validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="09013-106">検証規則は、ユーザーがムービーを作成または編集するときに、いつでも適用できます。</span><span class="sxs-lookup"><span data-stu-id="09013-106">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="09013-107">検証</span><span class="sxs-lookup"><span data-stu-id="09013-107">Validation</span></span>

<span data-ttu-id="09013-108">ソフトウェア開発には、[DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself" (繰り返しを避けること)) という重要な理念があります。</span><span class="sxs-lookup"><span data-stu-id="09013-108">A key tenet of software development is called [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="09013-109">Razor ページでは、機能を一度規定したら、それをアプリ全体に反映する開発を推奨しています。</span><span class="sxs-lookup"><span data-stu-id="09013-109">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="09013-110">DRY によって、アプリのコード量を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="09013-110">DRY can help reduce the amount of code in an app.</span></span> <span data-ttu-id="09013-111">DRY を実施すると、コードのエラーが発生する可能性が低くなり、テストと保守が簡単になります。</span><span class="sxs-lookup"><span data-stu-id="09013-111">DRY makes the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="09013-112">Razor ページと Entity Framework が提供している検証のサポートは、DRY 原則の好例です。</span><span class="sxs-lookup"><span data-stu-id="09013-112">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="09013-113">検証規則は、1 つの場所 (モデル クラス内) で宣言的に規定され、アプリの任意の場所で適用されます。</span><span class="sxs-lookup"><span data-stu-id="09013-113">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

### <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="09013-114">検証規則をムービー モデルに追加する</span><span class="sxs-lookup"><span data-stu-id="09013-114">Adding validation rules to the movie model</span></span>

<span data-ttu-id="09013-115">*Models/Movie.cs* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="09013-115">Open the *Models/Movie.cs* file.</span></span> <span data-ttu-id="09013-116">[DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) には、クラスまたはプロパティに宣言的に適用される組み込みの検証属性セットがあります。</span><span class="sxs-lookup"><span data-stu-id="09013-116">[DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) provides a built-in set of validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="09013-117">また、DataAnnotations には、書式設定を支援し、検証を行わない `DataType` のような書式設定属性もあります。</span><span class="sxs-lookup"><span data-stu-id="09013-117">DataAnnotations also contains formatting attributes like `DataType` that help with formatting and don't provide validation.</span></span>

<span data-ttu-id="09013-118">`Required`、`StringLength`、`RegularExpression`、および `Range` 検証属性を利用するように `Movie` クラスを更新します。</span><span class="sxs-lookup"><span data-stu-id="09013-118">Update the `Movie` class to take advantage of the `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="09013-119">検証属性で、モデルのプロパティに適用されている動作を指定します。</span><span class="sxs-lookup"><span data-stu-id="09013-119">Validation attributes specify behavior that's enforced on model properties:</span></span>

* <span data-ttu-id="09013-120">`Required` と `MinimumLength` の属性は、プロパティに値が必要なことを示します。</span><span class="sxs-lookup"><span data-stu-id="09013-120">The `Required` and `MinimumLength` attributes indicate that a property must have a value.</span></span> <span data-ttu-id="09013-121">ただし、ユーザーがnull 許容型の妥当性制約を満たすために空白を入力することを防ぐことはできません。</span><span class="sxs-lookup"><span data-stu-id="09013-121">However, nothing prevents a user from entering whitespace to satisfy the validation constraint for a nullable type.</span></span> <span data-ttu-id="09013-122">null 非許容の[値の型](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value-types) (`decimal`、`int`、`float`、`DateTime` など) は本質的に必須ではなく、`Required` 属性を必要としません。</span><span class="sxs-lookup"><span data-stu-id="09013-122">Non-nullable [value types](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, and `DateTime`) are inherently required and don't need the `Required` attribute.</span></span>
* <span data-ttu-id="09013-123">`RegularExpression` 属性は、ユーザーが入力できる文字を制限します。</span><span class="sxs-lookup"><span data-stu-id="09013-123">The `RegularExpression` attribute limits the characters that the user can enter.</span></span> <span data-ttu-id="09013-124">前のコードでは、`Genre` は 1 文字以上の大文字で始まり、0 文字以上の英字、一重引用符または二重引用符、空白文字、またはダッシュを続ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="09013-124">In the preceding code, `Genre` must start with one or more capital letters and follow with zero or more letters, single or double quotes, whitespace characters, or dashes.</span></span> <span data-ttu-id="09013-125">`Rating` は 1 文字以上の大文字で始まり、0 文字以上の英字、数字、一重引用符または二重引用符、空白文字、またはダッシュを続ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="09013-125">`Rating` must start with one or more capital letters and follow with zero or more letters, numbers, single or double quotes, whitespace characters, or dashes.</span></span>
* <span data-ttu-id="09013-126">`Range` 属性は、指定した範囲に値を制限します。</span><span class="sxs-lookup"><span data-stu-id="09013-126">The `Range` attribute constrains a value to a specified range.</span></span>
* <span data-ttu-id="09013-127">`StringLength` 属性は文字列の最大長を設定します。必要に応じて、最短長も設定できます。</span><span class="sxs-lookup"><span data-stu-id="09013-127">The `StringLength` attribute sets the maximum length of a string, and optionally the minimum length.</span></span> 

<span data-ttu-id="09013-128">ASP.NET Core で検証規則を自動的に適用すると、アプリをより堅牢にすることができます。</span><span class="sxs-lookup"><span data-stu-id="09013-128">Having validation rules automatically enforced by ASP.NET Core helps make an app more robust.</span></span> <span data-ttu-id="09013-129">モデルに自動検証を適用すると、新しいコードを追加したときに適用を思い出す必要がないので、アプリの保護に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="09013-129">Automatic validation on models helps protect the app because you don't have to remember to apply them when new code is added.</span></span>

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="09013-130">Razor ページの検証エラー UI</span><span class="sxs-lookup"><span data-stu-id="09013-130">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="09013-131">アプリを実行し、[Pages/Movies]/(ページ/ムービー/) に移動します。</span><span class="sxs-lookup"><span data-stu-id="09013-131">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="09013-132">**[Create New]\(新規作成\)** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="09013-132">Select the **Create New** link.</span></span> <span data-ttu-id="09013-133">いくつか無効な値を入力してフォームを設定します。</span><span class="sxs-lookup"><span data-stu-id="09013-133">Complete the form with some invalid values.</span></span> <span data-ttu-id="09013-134">jQuery クライアント側検証がエラーを検出すると、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="09013-134">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![複数 jQuery クライアント側検証エラーが表示されたムービー ビュー フォーム](validation/_static/val.png)

> [!NOTE]
> <span data-ttu-id="09013-136">`Price` フィールドに小数点またはコンマを入力することはできない場合があります。</span><span class="sxs-lookup"><span data-stu-id="09013-136">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="09013-137">小数点にコンマ (",") を使用し、英語 (米国) 以外の日付形式を使用する英語以外のロケールの [jQuery 検証](https://jqueryvalidation.org/)をサポートするには、アプリをグローバル化する手順を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="09013-137">To support [jQuery validation](https://jqueryvalidation.org/) in non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="09013-138">詳しくは、「[その他の技術情報](#additional-resources)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="09013-138">See [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="09013-139">ここでは、単に 10 のような整数を入力します。</span><span class="sxs-lookup"><span data-stu-id="09013-139">For now, just enter whole numbers like 10.</span></span>

<span data-ttu-id="09013-140">無効な値を含む各フィールドに、検証エラー メッセージが自動的に表示されることがわかります。</span><span class="sxs-lookup"><span data-stu-id="09013-140">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="09013-141">エラーは、(JavaScript と jQuery を使用している) クライアント側とサーバー側 (ユーザーが JavaScript を無効にしている場合) の両方に適用されます。</span><span class="sxs-lookup"><span data-stu-id="09013-141">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="09013-142">大きな利点は、[Create]/(作成/) ページまたは [Edit]/(編集/) ページの変更が**必要ない**ことです。</span><span class="sxs-lookup"><span data-stu-id="09013-142">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="09013-143">一度 DataAnnotations がモデルに適用されると、検証 UI は有効化されます。</span><span class="sxs-lookup"><span data-stu-id="09013-143">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="09013-144">このチュートリアルで作成した Razor ページは、(`Movie` モデル クラスのプロパティの検証属性を使用して) 検証規則を自動的に抽出します。</span><span class="sxs-lookup"><span data-stu-id="09013-144">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="09013-145">[Edit]/(編集/) ページを使用して検証をテストすると、同じ検証が適用されます。</span><span class="sxs-lookup"><span data-stu-id="09013-145">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="09013-146">クライアント側の検証エラーがなくなるまで、フォーム データはサーバーにポストされません。</span><span class="sxs-lookup"><span data-stu-id="09013-146">The form data isn't posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="09013-147">次のうち 1 つまたは複数の方法で、フォーム データがポストされていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="09013-147">Verify form data isn't posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="09013-148">`OnPostAsync` メソッドにブレークポイントを設定します。</span><span class="sxs-lookup"><span data-stu-id="09013-148">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="09013-149">フォームを送信します (**[Create]/(作成/)** または **[Save]/(保存/)** を選択します)。</span><span class="sxs-lookup"><span data-stu-id="09013-149">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="09013-150">ブレークポイントがヒットすることはありません。</span><span class="sxs-lookup"><span data-stu-id="09013-150">The break point is never hit.</span></span>
* <span data-ttu-id="09013-151">[Fiddler ツール](http://www.telerik.com/fiddler)を使用します。</span><span class="sxs-lookup"><span data-stu-id="09013-151">Use the [Fiddler tool](http://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="09013-152">ブラウザー開発者向けツールを使用して、ネットワーク トラフィックを監視します。</span><span class="sxs-lookup"><span data-stu-id="09013-152">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="09013-153">サーバー側の検証</span><span class="sxs-lookup"><span data-stu-id="09013-153">Server-side validation</span></span>

<span data-ttu-id="09013-154">ブラウザーで JavaScript が無効にされている場合、エラーがあるフォームを送信すると、サーバーにポストされます。</span><span class="sxs-lookup"><span data-stu-id="09013-154">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="09013-155">必要に応じて、サーバー側の検証をテストします。</span><span class="sxs-lookup"><span data-stu-id="09013-155">Optional, test server-side validation:</span></span>

* <span data-ttu-id="09013-156">ブラウザーで JavaScript を無効にします。</span><span class="sxs-lookup"><span data-stu-id="09013-156">Disable JavaScript in the browser.</span></span> <span data-ttu-id="09013-157">ブラウザーの開発者ツールを使用して、これを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="09013-157">You can do this using your browser's developer tools.</span></span> <span data-ttu-id="09013-158">ブラウザーで JavaScript を無効にすることができない場合は、別のブラウザーを試してください。</span><span class="sxs-lookup"><span data-stu-id="09013-158">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="09013-159">[Create] または [Edit] ページの `OnPostAsync` メソッドにブレークポイントを設定します。</span><span class="sxs-lookup"><span data-stu-id="09013-159">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="09013-160">検証エラーがあるフォームを送信します。</span><span class="sxs-lookup"><span data-stu-id="09013-160">Submit a form with validation errors.</span></span>
* <span data-ttu-id="09013-161">モデルの状態が無効であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="09013-161">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="09013-162">次のコードは、以前のチュートリアルでスキャフォールディング処理した *Create.cshtml* ページの一部です。</span><span class="sxs-lookup"><span data-stu-id="09013-162">The following code shows a portion of the *Create.cshtml* page that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="09013-163">[Create] または [Edit] ページにおいて、最初のフォームの表示と、エラーイベント時におけるフォームの再表示のために使用されます。</span><span class="sxs-lookup"><span data-stu-id="09013-163">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

<span data-ttu-id="09013-164">[入力タグ ヘルパー](xref:mvc/views/working-with-forms)は [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) 属性を使用し、クライアント側で jQuery 検証に必要な HTML 属性を生成します。</span><span class="sxs-lookup"><span data-stu-id="09013-164">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="09013-165">[検証タグ ヘルパー](xref:mvc/views/working-with-forms#the-validation-tag-helpers)には検証エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="09013-165">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="09013-166">詳しくは、[検証に関する記事](xref:mvc/models/validation)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="09013-166">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="09013-167">[Create] ページと [Edit] ページ内に検証規則はありません。</span><span class="sxs-lookup"><span data-stu-id="09013-167">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="09013-168">検証規則とエラー文字列は、`Movie` クラスでのみ指定されています。</span><span class="sxs-lookup"><span data-stu-id="09013-168">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="09013-169">これらの検証規則は、`Movie` モデルを編集する Razor ページに自動的に適用されます。</span><span class="sxs-lookup"><span data-stu-id="09013-169">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="09013-170">検証ロジックの変更が必要な場合は、モデルでのみ変更します。</span><span class="sxs-lookup"><span data-stu-id="09013-170">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="09013-171">検証は常にアプリケーション全体に適用されます (検証ロジックは 1 か所で定義されます)。</span><span class="sxs-lookup"><span data-stu-id="09013-171">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="09013-172">検証が 1 か所であることは、コードが整理された状態を保つことを助け、保守と更新を簡単にします。</span><span class="sxs-lookup"><span data-stu-id="09013-172">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="09013-173">DataType 属性の使用</span><span class="sxs-lookup"><span data-stu-id="09013-173">Using DataType Attributes</span></span>

<span data-ttu-id="09013-174">`Movie` クラスを調べます。</span><span class="sxs-lookup"><span data-stu-id="09013-174">Examine the `Movie` class.</span></span> <span data-ttu-id="09013-175">`System.ComponentModel.DataAnnotations` 名前空間には、組み込みの検証属性セットに加え、書式設定の属性もあります。</span><span class="sxs-lookup"><span data-stu-id="09013-175">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="09013-176">`DataType` 属性は、`ReleaseDate` および `Price` プロパティに適用されます。</span><span class="sxs-lookup"><span data-stu-id="09013-176">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="09013-177">`DataType` 属性は、ビュー エンジンに対して、データの書式設定のヒントのみを提供します (また、URL の場合に `<a>`、電子メールの場合に `<a href="mailto:EmailAddress.com">` などの属性を提供します)。</span><span class="sxs-lookup"><span data-stu-id="09013-177">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="09013-178">データの書式を検証するためには、`RegularExpression` 属性を使用してください。</span><span class="sxs-lookup"><span data-stu-id="09013-178">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="09013-179">`DataType` 属性は、データベースの組み込み型よりも具体的なデータ型を指定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="09013-179">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="09013-180">`DataType` 属性は、検証属性ではありません。</span><span class="sxs-lookup"><span data-stu-id="09013-180">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="09013-181">サンプル アプリケーションでは、日付のみが表示され、時刻は表示されません。</span><span class="sxs-lookup"><span data-stu-id="09013-181">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="09013-182">`DataType` 列挙型は、Date、Time、PhoneNumber、Currency、EmailAddress など、多くの型のために用意されています。</span><span class="sxs-lookup"><span data-stu-id="09013-182">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="09013-183">また、`DataType` 属性を使用して、アプリケーションで型固有の機能を自動的に提供することもできます。</span><span class="sxs-lookup"><span data-stu-id="09013-183">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="09013-184">たとえば、`DataType.EmailAddress` に対して `mailto:` リンクを作成できます。</span><span class="sxs-lookup"><span data-stu-id="09013-184">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="09013-185">HTML 5 をサポートしているブラウザーでは、`DataType.Date` に日付セレクターを提供できます。</span><span class="sxs-lookup"><span data-stu-id="09013-185">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="09013-186">`DataType` 属性は、HTML 5 ブラウザーが使用する HTML 5 `data-` ("データ ダッシュ" と読みます) 属性を出力します。</span><span class="sxs-lookup"><span data-stu-id="09013-186">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="09013-187">`DataType` 属性は、検証を**提供していません**。</span><span class="sxs-lookup"><span data-stu-id="09013-187">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="09013-188">`DataType.Date` は、表示される日付の書式を指定しません。</span><span class="sxs-lookup"><span data-stu-id="09013-188">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="09013-189">既定で、日付フィールドはサーバーの `CultureInfo` に基づき、既定の書式に従って表示されます。</span><span class="sxs-lookup"><span data-stu-id="09013-189">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="09013-190">`[Column(TypeName = "decimal(18, 2)")]` データ注釈は、Entity Framework Core がデータベースの通貨と `Price` を正しくマッピングできるようにするために必要です。</span><span class="sxs-lookup"><span data-stu-id="09013-190">The `[Column(TypeName = "decimal(18, 2)")]` data annotation is required so Entity Framework Core can correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="09013-191">詳細については、「[Data Types](/ef/core/modeling/relational/data-types)」(データ型) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="09013-191">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

::: moniker-end

<span data-ttu-id="09013-192">`DisplayFormat` 属性は、日付の書式を明示的に指定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="09013-192">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="09013-193">`ApplyFormatInEditMode` 設定では、編集対象の値を表示するときに適用する必要がある書式設定を指定します。</span><span class="sxs-lookup"><span data-stu-id="09013-193">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="09013-194">一部のフィールドでは、この動作が不要なことがあります。</span><span class="sxs-lookup"><span data-stu-id="09013-194">You might not want that behavior for some fields.</span></span> <span data-ttu-id="09013-195">たとえば、通貨値の場合、編集 UI で通貨記号が不要なことがあります。</span><span class="sxs-lookup"><span data-stu-id="09013-195">For example, in currency values, you probably don't want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="09013-196">`DisplayFormat` 属性を単独で使用できますが、一般的に、`DataType` 属性を使用することが推奨されます。</span><span class="sxs-lookup"><span data-stu-id="09013-196">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="09013-197">`DataType` 属性は、画面でのレンダリング方法とは異なり、データのセマンティクスを伝達します。また、DisplayFormat にはない、次の利点があります。</span><span class="sxs-lookup"><span data-stu-id="09013-197">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="09013-198">ブラウザーは HTML5 機能を有効にすることができます (たとえば、カレンダー コントロール、ロケールに適した通貨記号、電子メール リンクを表示するときなど)。</span><span class="sxs-lookup"><span data-stu-id="09013-198">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="09013-199">ブラウザーの既定では、ロケールに基づいて正しい書式を使ってデータがレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="09013-199">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="09013-200">`DataType` 属性を使用すると、ASP.NET Core フレームワークで、適切なフィールド テンプレートを選択し、データをレンダリングすることができます。</span><span class="sxs-lookup"><span data-stu-id="09013-200">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="09013-201">`DisplayFormat` を単独で使用する場合、文字列テンプレートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="09013-201">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="09013-202">注: jQuery の検証は、`Range` 属性と `DateTime` では機能しません。</span><span class="sxs-lookup"><span data-stu-id="09013-202">Note: jQuery validation doesn't work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="09013-203">たとえば、次のコードでは、指定した範囲内の日付であっても、クライアント側の検証エラーが常に表示されます。</span><span class="sxs-lookup"><span data-stu-id="09013-203">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="09013-204">一般的に、モデルに日付をハードコーディングしてコンパイルすることは推奨されません。そのため、`Range` 属性と `DateTime` の使用は推奨されません。</span><span class="sxs-lookup"><span data-stu-id="09013-204">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="09013-205">次のコードは、1 行で複数の属性を組み合わせる例です。</span><span class="sxs-lookup"><span data-stu-id="09013-205">The following code shows combining attributes on one line:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="09013-206">[Razor Pages と EF Core の概要](xref:data/ef-rp/intro)に関するページでは、Razor Pages での EF Core 操作についてより詳しく説明されています。</span><span class="sxs-lookup"><span data-stu-id="09013-206">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) shows advanced EF Core operations with Razor Pages.</span></span>

### <a name="publish-to-azure"></a><span data-ttu-id="09013-207">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="09013-207">Publish to Azure</span></span>

<span data-ttu-id="09013-208">Azure へのデプロイについては、「[チュートリアル: SQL Database を使用して Azure に ASP.NET アプリを作成する](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="09013-208">For information on deploying to Azure, See [Tutorial: Build an ASP.NET app in Azure with SQL Database](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span></span> <span data-ttu-id="09013-209">指示は ASP.NET Core アプリではなく、ASP.NET アプリに関するものですが、手順は同じです。</span><span class="sxs-lookup"><span data-stu-id="09013-209">The instruction are for an ASP.NET app, not an ASP.NET Core app, but the steps are the same.</span></span>

<span data-ttu-id="09013-210">このたびは、この Razor ページの紹介を最後までお読みいただきありがとうございました。</span><span class="sxs-lookup"><span data-stu-id="09013-210">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="09013-211">貴重なご意見をお寄せいただき心より感謝いたします。</span><span class="sxs-lookup"><span data-stu-id="09013-211">We appreciate feedback.</span></span> <span data-ttu-id="09013-212">このチュートリアルの後は、[Razor ページと EF Core の概要](xref:data/ef-rp/intro)に関するページにお進みいただくことが推奨されます。</span><span class="sxs-lookup"><span data-stu-id="09013-212">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="09013-213">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="09013-213">Additional resources</span></span>

* [<span data-ttu-id="09013-214">フォームの操作</span><span class="sxs-lookup"><span data-stu-id="09013-214">Working with Forms</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="09013-215">グローバライズとローカライズ</span><span class="sxs-lookup"><span data-stu-id="09013-215">Globalization and localization</span></span>](xref:fundamentals/localization)
* [<span data-ttu-id="09013-216">Tag Helpers の概要</span><span class="sxs-lookup"><span data-stu-id="09013-216">Introduction to Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="09013-217">タグ ヘルパーの作成</span><span class="sxs-lookup"><span data-stu-id="09013-217">Author Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)

> [!div class="step-by-step"]
> [<span data-ttu-id="09013-218">前へ: 新しいフィールドの追加</span><span class="sxs-lookup"><span data-stu-id="09013-218">Previous: Adding a new field</span></span>](xref:tutorials/razor-pages/new-field)
