---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: "データ バインドする (c#)、アコーディオン |Microsoft ドキュメント"
author: wenz
description: "AJAX コントロールのツールキットでアコーディオン コントロールは、複数のペインがあり、時にそれらのいずれかを表示することができます。 パネルは通常、w を宣言しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: a8250f58655b8fe8638d8e7a7b084ee9c33fe986
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="databinding-to-an-accordion-c"></a><span data-ttu-id="6a4df-104">(C#)、アコーディオンへのデータ バインド</span><span class="sxs-lookup"><span data-stu-id="6a4df-104">Databinding to an Accordion (C#)</span></span>
====================
<span data-ttu-id="6a4df-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6a4df-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6a4df-106">[コードをダウンロードする](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="6a4df-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)</span></span>

> <span data-ttu-id="6a4df-107">AJAX コントロールのツールキットでアコーディオン コントロールは、複数のペインがあり、時にそれらのいずれかを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="6a4df-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="6a4df-108">パネルは通常ページ自体内で宣言されていますが、データ ソースへのバインディングが複数の柔軟性を提供します。</span><span class="sxs-lookup"><span data-stu-id="6a4df-108">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="6a4df-109">概要</span><span class="sxs-lookup"><span data-stu-id="6a4df-109">Overview</span></span>

<span data-ttu-id="6a4df-110">AJAX コントロールのツールキットでアコーディオン コントロールは、複数のペインがあり、時にそれらのいずれかを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="6a4df-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="6a4df-111">パネルは通常ページ自体内で宣言されていますが、データ ソースへのバインディングが複数の柔軟性を提供します。</span><span class="sxs-lookup"><span data-stu-id="6a4df-111">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="6a4df-112">手順</span><span class="sxs-lookup"><span data-stu-id="6a4df-112">Steps</span></span>

<span data-ttu-id="6a4df-113">まず、データ ソースが必要です。</span><span class="sxs-lookup"><span data-stu-id="6a4df-113">First of all, a data source is required.</span></span> <span data-ttu-id="6a4df-114">このサンプルでは、AdventureWorks データベースと、Microsoft SQL Server 2005 Express Edition を使用します。</span><span class="sxs-lookup"><span data-stu-id="6a4df-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="6a4df-115">データベース (express エディションを含む)、Visual Studio のインストールのオプションの一部であるし、下にある個別のダウンロードとしても利用[https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)です。</span><span class="sxs-lookup"><span data-stu-id="6a4df-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="6a4df-116">AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (でダウンロード[https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。</span><span class="sxs-lookup"><span data-stu-id="6a4df-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="6a4df-117">データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する ([https://www.microsoft.com/downloads/details.aspx?表示 = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) をアタッチし、`AdventureWorks.mdf`データベース ファイル。</span><span class="sxs-lookup"><span data-stu-id="6a4df-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="6a4df-118">このサンプルのものと、SQL Server 2005 Express Edition のインスタンスが呼び出される`SQLEXPRESS`web サーバーと同じコンピューター上に存在し、これは既定の設定にもします。</span><span class="sxs-lookup"><span data-stu-id="6a4df-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="6a4df-119">セットアップが異なっている場合は、データベースの接続情報を調整する必要です。</span><span class="sxs-lookup"><span data-stu-id="6a4df-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="6a4df-120">ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="6a4df-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

<span data-ttu-id="6a4df-121">次に、ページに、データ ソースを追加します。</span><span class="sxs-lookup"><span data-stu-id="6a4df-121">Then, add a data source to the page.</span></span> <span data-ttu-id="6a4df-122">限られた量のデータを使用するのにはのみ、AdventureWorks データベースの仕入先テーブルの最初の 5 つのエントリを選択します。</span><span class="sxs-lookup"><span data-stu-id="6a4df-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="6a4df-123">データ ソースを作成する Visual Studio のアシスタントを使用している場合、バグを現在のバージョンでは、テーブル名のプレフィックスにしないことに注意してください (`Vendor`) と`Purchasing`です。</span><span class="sxs-lookup"><span data-stu-id="6a4df-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="6a4df-124">次のマークアップは、正しい構文を示しています。</span><span class="sxs-lookup"><span data-stu-id="6a4df-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

<span data-ttu-id="6a4df-125">データ ソースの名前 (ID) に注意してください。</span><span class="sxs-lookup"><span data-stu-id="6a4df-125">Remember the name (ID) of the data source.</span></span> <span data-ttu-id="6a4df-126">非常にこの id を使用し、必要があります、`DataSourceID`アコーディオン コントロールのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="6a4df-126">This very identification must then be used in the `DataSourceID` property of the Accordion control:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

<span data-ttu-id="6a4df-127">アコーディオン コントロール内では、ヘッダーを含む、コントロールのさまざまな部分のテンプレートを提供できます (`<HeaderTemplate>`) とコンテンツ (`<ContentTemplate>`)。</span><span class="sxs-lookup"><span data-stu-id="6a4df-127">Within the Accordion control, you can provide templates for various parts of the control, including the header (`<HeaderTemplate>`) and the content (`<ContentTemplate>`).</span></span> <span data-ttu-id="6a4df-128">これらの要素内のデータからデータを出力だけを使用して、ソース、`DataBinder.Eval()`メソッド。</span><span class="sxs-lookup"><span data-stu-id="6a4df-128">Within these elements, just output the data from the data source, using the `DataBinder.Eval()` method:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

<span data-ttu-id="6a4df-129">ページが読み込まれるときに、このサーバー側コードで、アコーディオンにデータ ソースをバインドしてください。</span><span class="sxs-lookup"><span data-stu-id="6a4df-129">When the page is loaded, the data source must be bound to the accordion with this server-side code:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

<span data-ttu-id="6a4df-130">このサンプルを完了するまでには、アコーディオン コントロールで参照されている 2 つの CSS クラスを定義する必要があります (そのプロパティで`HeaderCssClass`と`ContentCssClass`)。</span><span class="sxs-lookup"><span data-stu-id="6a4df-130">To conclude this sample, you need to define the two CSS classes that are referenced in the Accordion control (in its properties `HeaderCssClass` and `ContentCssClass`).</span></span> <span data-ttu-id="6a4df-131">次のマークアップを格納、`<head>`ページのセクション。</span><span class="sxs-lookup"><span data-stu-id="6a4df-131">Put the following markup in the `<head>` section of the page:</span></span>

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


<span data-ttu-id="6a4df-132">[![データ ソースから直接、アコーディオンのデータのソースします。](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6a4df-132">[![The data in the accordion comes directly from the data source](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)</span></span>

<span data-ttu-id="6a4df-133">データ ソースから直接、アコーディオンのデータのソース ([フルサイズのイメージを表示するをクリックして](databinding-to-an-accordion-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6a4df-133">The data in the accordion comes directly from the data source ([Click to view full-size image](databinding-to-an-accordion-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="6a4df-134">次へ</span><span class="sxs-lookup"><span data-stu-id="6a4df-134">Next</span></span>](dynamically-adding-an-accordion-pane-cs.md)