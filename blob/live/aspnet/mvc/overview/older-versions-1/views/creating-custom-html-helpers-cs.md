---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: "カスタム HTML ヘルパー (c#) の作成 |Microsoft ドキュメント"
author: microsoft
description: "このチュートリアルの目的では、MVC ビュー内で使用できるカスタムの HTML ヘルパーの作成方法を示しています。 HTML ヘルパーを活用しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: a0b6d67eb7aab51ba2b422fab0788e34255f2c8c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="creating-custom-html-helpers-c"></a><span data-ttu-id="ce299-104">カスタム HTML ヘルパー (c#) の作成</span><span class="sxs-lookup"><span data-stu-id="ce299-104">Creating Custom HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="ce299-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ce299-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="ce299-106">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="ce299-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> <span data-ttu-id="ce299-107">このチュートリアルの目的では、MVC ビュー内で使用できるカスタムの HTML ヘルパーの作成方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="ce299-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="ce299-108">HTML ヘルパーを利用して、標準の HTML ページを作成する必要がありますを HTML タグの面倒な入力量を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="ce299-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>


<span data-ttu-id="ce299-109">このチュートリアルの目的では、MVC ビュー内で使用できるカスタムの HTML ヘルパーの作成方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="ce299-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="ce299-110">HTML ヘルパーを利用して、標準の HTML ページを作成する必要がありますを HTML タグの面倒な入力量を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="ce299-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="ce299-111">このチュートリアルの最初の部分では、ASP.NET MVC フレームワークに含まれている既存の HTML ヘルパーの一部を説明します。</span><span class="sxs-lookup"><span data-stu-id="ce299-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="ce299-112">次に、カスタム HTML ヘルパーの作成の 2 つの方法を説明しました。 静的メソッドを作成し、拡張メソッドを作成することで、カスタム HTML ヘルパーを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ce299-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a static method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="ce299-113">HTML ヘルパーを理解します。</span><span class="sxs-lookup"><span data-stu-id="ce299-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="ce299-114">HTML ヘルパーは、文字列を返すメソッドのみです。</span><span class="sxs-lookup"><span data-stu-id="ce299-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="ce299-115">文字列は、内容の任意の型を表すことができます。</span><span class="sxs-lookup"><span data-stu-id="ce299-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="ce299-116">たとえば、HTML などの標準の HTML タグを表示するために HTML ヘルパーを使用できます`<input>`と`<img>`タグ。</span><span class="sxs-lookup"><span data-stu-id="ce299-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="ce299-117">タブ ストリップまたはデータベースのデータの HTML テーブルなどのより複雑なコンテンツを表示するために HTML ヘルパーを使用することができますも。</span><span class="sxs-lookup"><span data-stu-id="ce299-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="ce299-118">ASP.NET MVC フレームワークには、次の (完全な一覧では無効) 標準の HTML ヘルパーのセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ce299-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="ce299-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="ce299-119">Html.ActionLink()</span></span>
- <span data-ttu-id="ce299-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="ce299-120">Html.BeginForm()</span></span>
- <span data-ttu-id="ce299-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="ce299-121">Html.CheckBox()</span></span>
- <span data-ttu-id="ce299-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="ce299-122">Html.DropDownList()</span></span>
- <span data-ttu-id="ce299-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="ce299-123">Html.EndForm()</span></span>
- <span data-ttu-id="ce299-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="ce299-124">Html.Hidden()</span></span>
- <span data-ttu-id="ce299-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="ce299-125">Html.ListBox()</span></span>
- <span data-ttu-id="ce299-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="ce299-126">Html.Password()</span></span>
- <span data-ttu-id="ce299-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="ce299-127">Html.RadioButton()</span></span>
- <span data-ttu-id="ce299-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="ce299-128">Html.TextArea()</span></span>
- <span data-ttu-id="ce299-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="ce299-129">Html.TextBox()</span></span>

<span data-ttu-id="ce299-130">たとえば、1 を一覧表示するフォームがあるとします。</span><span class="sxs-lookup"><span data-stu-id="ce299-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="ce299-131">(図 1 を参照してください)、標準の HTML ヘルパーの 2 つのヘルプでは、このフォームが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ce299-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="ce299-132">このフォームを使用して、`Html.BeginForm()`と`Html.TextBox()`単純な HTML フォームを表示するためにヘルパー メソッドです。</span><span class="sxs-lookup"><span data-stu-id="ce299-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods to render a simple HTML form.</span></span>


<span data-ttu-id="ce299-133">[![HTML ヘルパー ページが表示されます。](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ce299-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="ce299-134">**図 01**: HTML ヘルパー ページが表示されます ([フルサイズのイメージを表示するをクリックして](creating-custom-html-helpers-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ce299-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image3.png))</span></span>


<span data-ttu-id="ce299-135">**1 – を一覧表示します。`Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="ce299-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

<span data-ttu-id="ce299-136">Html.BeginForm() ヘルパー メソッドを使用開始と終了の HTML の作成を`<form>`タグ。</span><span class="sxs-lookup"><span data-stu-id="ce299-136">The Html.BeginForm() Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="ce299-137">注意して、`Html.BeginForm()`を使用して、内でメソッドが呼び出されたステートメントです。</span><span class="sxs-lookup"><span data-stu-id="ce299-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="ce299-138">確実にステートメントを使用して、`<form>`を使用して、最後にタグが閉じられるブロックします。</span><span class="sxs-lookup"><span data-stu-id="ce299-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="ce299-139">場合を使用して、作成する代わりに、ブロックを終了する Html.EndForm() ヘルパー メソッドを呼び出すことができます、`<form>`タグ。</span><span class="sxs-lookup"><span data-stu-id="ce299-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="ce299-140">タグを作成して、終了するには、どの方法を使用して`<form>`ように最も理解できるタグです。</span><span class="sxs-lookup"><span data-stu-id="ce299-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="ce299-141">`Html.TextBox()`ヘルパー メソッドは HTML を表示するリスト 1 で使用される`<input>`タグ。</span><span class="sxs-lookup"><span data-stu-id="ce299-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="ce299-142">お使いのブラウザーでソースの表示を選択した場合をリスト 2 での HTML ソースを表示します。</span><span class="sxs-lookup"><span data-stu-id="ce299-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="ce299-143">ソースが標準の HTML タグが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="ce299-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ce299-144">注意して、 `Html.TextBox()`HTML ヘルパーでレンダリングされて`<%= %>`の代わりにタグ`<% %>`タグ。</span><span class="sxs-lookup"><span data-stu-id="ce299-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="ce299-145">等号 (=) を含めない場合は、ブラウザーにレンダリング取得が何も行われません。</span><span class="sxs-lookup"><span data-stu-id="ce299-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="ce299-146">ASP.NET MVC フレームワークには、ヘルパーの少数のセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ce299-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="ce299-147">ほとんどの場合、カスタム HTML ヘルパーに MVC フレームワークを拡張する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce299-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="ce299-148">このチュートリアルの残りの部分では、カスタム HTML ヘルパーの作成の 2 つの方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="ce299-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="ce299-149">**2 – を一覧表示します。`Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="ce299-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a><span data-ttu-id="ce299-150">静的メソッドと HTML ヘルパーの作成</span><span class="sxs-lookup"><span data-stu-id="ce299-150">Creating HTML Helpers with Static Methods</span></span>

<span data-ttu-id="ce299-151">新しい HTML ヘルパーを作成する最も簡単な方法では、文字列を返す静的メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="ce299-151">The easiest way to create a new HTML Helper is to create a static method that returns a string.</span></span> <span data-ttu-id="ce299-152">たとえば、HTML をレンダリングする新しい HTML ヘルパーの作成を決定すること`<label>`タグ。</span><span class="sxs-lookup"><span data-stu-id="ce299-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="ce299-153">表示するために一覧表示する 2 でクラスを使用することができます、`<label>`です。</span><span class="sxs-lookup"><span data-stu-id="ce299-153">You can use the class in Listing 2 to render a `<label>` .</span></span>

<span data-ttu-id="ce299-154">**2 – を一覧表示します。`Helpers\LabelHelper.cs`**</span><span class="sxs-lookup"><span data-stu-id="ce299-154">**Listing 2 – `Helpers\LabelHelper.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

<span data-ttu-id="ce299-155">2 のリスト内のクラスに関する特別なはありません。</span><span class="sxs-lookup"><span data-stu-id="ce299-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="ce299-156">`Label()`メソッドは単に文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="ce299-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="ce299-157">リスト 3 の変更、インデックス ビューを使用して、 `LabelHelper` HTML をレンダリングする`<label>`タグ。</span><span class="sxs-lookup"><span data-stu-id="ce299-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="ce299-158">ビューに含まれることに注意してください、`<%@ imports %>`をインポートするディレクティブ、`Application1.Helpers`名前空間。</span><span class="sxs-lookup"><span data-stu-id="ce299-158">Notice that the view includes an `<%@ imports %>` directive that imports the `Application1.Helpers` namespace.</span></span>

<span data-ttu-id="ce299-159">**2 – を一覧表示します。`Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="ce299-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="ce299-160">拡張メソッドで HTML ヘルパーの作成</span><span class="sxs-lookup"><span data-stu-id="ce299-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="ce299-161">作成する場合だけに使用できる HTML ヘルパーなどの拡張メソッドを作成する必要があります、ASP.NET MVC フレームワークに含まれる標準の HTML ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="ce299-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="ce299-162">拡張メソッドを使用すると、既存のクラスに新しいメソッドを追加できます。</span><span class="sxs-lookup"><span data-stu-id="ce299-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="ce299-163">HTML ヘルパー メソッドを作成する場合は、新しいメソッドをビューの Html プロパティによって表される HtmlHelper クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="ce299-163">When creating an HTML Helper method, you add new methods to the HtmlHelper class represented by a view's Html property.</span></span>

<span data-ttu-id="ce299-164">リスト 3 のクラスを追加する拡張メソッド、`HtmlHelper`という名前のクラス`Label()`です。</span><span class="sxs-lookup"><span data-stu-id="ce299-164">The class in Listing 3 adds an extension method to the `HtmlHelper` class named `Label()`.</span></span> <span data-ttu-id="ce299-165">このクラスについて注意すべき点が 2 つがあります。</span><span class="sxs-lookup"><span data-stu-id="ce299-165">There are a couple of things that you should notice about this class.</span></span> <span data-ttu-id="ce299-166">最初に、クラスが、静的クラスであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="ce299-166">First, notice that the class is a static class.</span></span> <span data-ttu-id="ce299-167">静的クラスを持つ拡張メソッドを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce299-167">You must define an extension method with a static class.</span></span>

<span data-ttu-id="ce299-168">次に、ことに注意しての最初のパラメーター、`Label()`メソッドは、キーワードに続く`this`です。</span><span class="sxs-lookup"><span data-stu-id="ce299-168">Second, notice that the first parameter of the `Label()` method is preceded by the keyword `this`.</span></span> <span data-ttu-id="ce299-169">拡張メソッドの最初のパラメーターでは、拡張メソッドを拡張するクラスを示します。</span><span class="sxs-lookup"><span data-stu-id="ce299-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="ce299-170">**3 – を一覧表示します。`Helpers\LabelExtensions.cs`**</span><span class="sxs-lookup"><span data-stu-id="ce299-170">**Listing 3 – `Helpers\LabelExtensions.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

<span data-ttu-id="ce299-171">拡張メソッドがすべてのクラスの他のメソッドと同様に Visual Studio Intellisense に表示されます、拡張メソッドを作成し、アプリケーションを正常にビルドした後 (図 2 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="ce299-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="ce299-172">唯一の違いは、その拡張メソッドは、特別な記号が横に (下向きの矢印のアイコン) で表示されます。</span><span class="sxs-lookup"><span data-stu-id="ce299-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>


<span data-ttu-id="ce299-173">[![Html.Label() 拡張メソッドの使用](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="ce299-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span></span>

<span data-ttu-id="ce299-174">**図 02**: Html.Label() 拡張メソッドの使用 ([フルサイズのイメージを表示するをクリックして](creating-custom-html-helpers-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="ce299-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image6.png))</span></span>


<span data-ttu-id="ce299-175">インデックス ビューを一覧表示する 4 でのすべてを表示するために Html.Label() 拡張メソッドを使用してその`<label>`タグ。</span><span class="sxs-lookup"><span data-stu-id="ce299-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its `<label>` tags.</span></span>

<span data-ttu-id="ce299-176">**4 – を一覧表示します。`Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="ce299-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="ce299-177">概要</span><span class="sxs-lookup"><span data-stu-id="ce299-177">Summary</span></span>

<span data-ttu-id="ce299-178">このチュートリアルでは、カスタム HTML ヘルパーの作成の 2 つの方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="ce299-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="ce299-179">最初に、カスタムを作成する方法を学習しました`Label()`静的メソッドを作成することで HTML ヘルパーは、文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="ce299-179">First, you learned how to create a custom `Label()` HTML Helper by creating a static method that returns a string.</span></span> <span data-ttu-id="ce299-180">次に、カスタムを作成する方法を学習しました`Label()`HTML ヘルパー メソッドの拡張メソッドを作成することで、`HtmlHelper`クラスです。</span><span class="sxs-lookup"><span data-stu-id="ce299-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="ce299-181">このチュートリアルでは、非常に単純な HTML ヘルパー メソッドを構築に注目しました。</span><span class="sxs-lookup"><span data-stu-id="ce299-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="ce299-182">HTML ヘルパーが必要な複雑な作業できることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="ce299-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="ce299-183">ツリー ビュー、メニューのまたはデータベースのデータのテーブルなどの豊富なコンテンツを表示する HTML ヘルパーをビルドすることができます。</span><span class="sxs-lookup"><span data-stu-id="ce299-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ce299-184">[前へ](asp-net-mvc-views-overview-cs.md)
[次へ](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span><span class="sxs-lookup"><span data-stu-id="ce299-184">[Previous](asp-net-mvc-views-overview-cs.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span></span>