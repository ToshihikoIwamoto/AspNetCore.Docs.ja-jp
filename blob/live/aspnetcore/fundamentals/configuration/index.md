---
title: "ASP.NET Core の構成"
author: rick-anderson
description: "構成 API を使用して、複数の方法で ASP.NET Core アプリを構成します。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: 6281d6ba254670b111964715410fc0694ae4d149
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/29/2017
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="b457b-103">ASP.NET Core アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="b457b-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="b457b-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)、[Mark Michaelis](http://intellitect.com/author/mark-michaelis/)、[Steve Smith](https://ardalis.com/)、[Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b457b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b457b-105">構成 API は、名前と値のペアのリストに基づく ASP.NET Core Web アプリの構成方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="b457b-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="b457b-106">構成は実行時に複数のソースから読み取られます。</span><span class="sxs-lookup"><span data-stu-id="b457b-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="b457b-107">これらの名前と値のペアを複数レベルの階層にグループ化することができます。</span><span class="sxs-lookup"><span data-stu-id="b457b-107">You can group these name-value pairs into a multi-level hierarchy.</span></span> 

<span data-ttu-id="b457b-108">以下の構成プロバイダーがあります。</span><span class="sxs-lookup"><span data-stu-id="b457b-108">There are configuration providers for:</span></span>

* <span data-ttu-id="b457b-109">ファイル形式 (INI、JSON、および XML)</span><span class="sxs-lookup"><span data-stu-id="b457b-109">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="b457b-110">コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="b457b-110">Command-line arguments</span></span>
* <span data-ttu-id="b457b-111">環境変数</span><span class="sxs-lookup"><span data-stu-id="b457b-111">Environment variables</span></span>
* <span data-ttu-id="b457b-112">メモリ内 .NET オブジェクト</span><span class="sxs-lookup"><span data-stu-id="b457b-112">In-memory .NET objects</span></span>
* <span data-ttu-id="b457b-113">暗号化されたユーザー ストア</span><span class="sxs-lookup"><span data-stu-id="b457b-113">An encrypted user store</span></span>
* [<span data-ttu-id="b457b-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="b457b-114">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="b457b-115">カスタム プロバイダー (インストール済みまたは作成済み)</span><span class="sxs-lookup"><span data-stu-id="b457b-115">Custom providers (installed or created)</span></span>

<span data-ttu-id="b457b-116">各構成値は文字列キーにマップされます。</span><span class="sxs-lookup"><span data-stu-id="b457b-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="b457b-117">設定をカスタム [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) オブジェクト (プロパティを持つ単純な .NET クラス) に逆シリアル化するための組み込みのバインド サポートがあります。</span><span class="sxs-lookup"><span data-stu-id="b457b-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="b457b-118">オプション パターンではオプション クラスを使用して、関連する設定のグループを表します。</span><span class="sxs-lookup"><span data-stu-id="b457b-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="b457b-119">オプション パターンの使用の詳細については、「[オプション](xref:fundamentals/configuration/options)」トピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="b457b-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="b457b-120">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="b457b-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="b457b-121">JSON 構成</span><span class="sxs-lookup"><span data-stu-id="b457b-121">JSON configuration</span></span>

<span data-ttu-id="b457b-122">次のコンソール アプリでは JSON 構成プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="b457b-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="b457b-123">アプリは以下の構成設定を読み取り、表示します。</span><span class="sxs-lookup"><span data-stu-id="b457b-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="b457b-124">構成は、コロンで区切られたノードの名前と値のペアの階層リストで構成されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="b457b-125">値を取得するには、対応する項目のキーを使用して `Configuration` インデクサーにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="b457b-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="b457b-126">JSON 形式の構成ソースの配列を操作する場合は、コロンで区切られた文字列の一部として配列インデックスを使用します。</span><span class="sxs-lookup"><span data-stu-id="b457b-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="b457b-127">次の例では、上記の `wizards` 配列の最初の項目の名前を取得します。</span><span class="sxs-lookup"><span data-stu-id="b457b-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="b457b-128">組み込みの `Configuration` プロバイダーに書き込まれた名前と値のペアは永続化**されません**。</span><span class="sxs-lookup"><span data-stu-id="b457b-128">Name-value pairs written to the built-in `Configuration` providers are **not** persisted.</span></span> <span data-ttu-id="b457b-129">ただし、値を保存するカスタム プロバイダーを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="b457b-129">However, you can create a custom provider that saves values.</span></span> <span data-ttu-id="b457b-130">[カスタム構成プロバイダー](xref:fundamentals/configuration/index#custom-config-providers)に関する記述を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b457b-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="b457b-131">上記のサンプルでは、構成インデクサーを使用して値を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="b457b-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="b457b-132">`Startup` の外部の構成にアクセスする場合は、*オプション パターン*を使用します。</span><span class="sxs-lookup"><span data-stu-id="b457b-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="b457b-133">詳細については、「[オプション](xref:fundamentals/configuration/options)」トピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="b457b-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="b457b-134">開発、テスト、運用などの異なる環境に応じて異なる構成を設定するのが一般的です。</span><span class="sxs-lookup"><span data-stu-id="b457b-134">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="b457b-135">ASP.NET Core 2.x アプリの `CreateDefaultBuilder` 拡張メソッド (ASP.NET Core 1.x アプリの場合は、`AddJsonFile` および `AddEnvironmentVariables` を直接使用) では、JSON ファイルとシステム構成ソースを読み取るために構成プロバイダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="b457b-135">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="b457b-136">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="b457b-136">*appsettings.json*</span></span>
* <span data-ttu-id="b457b-137">*appsettings.\<EnvironmentName>.json*</span><span class="sxs-lookup"><span data-stu-id="b457b-137">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="b457b-138">環境変数</span><span class="sxs-lookup"><span data-stu-id="b457b-138">Environment variables</span></span>

<span data-ttu-id="b457b-139">パラメーターの説明については、「[AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b457b-139">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="b457b-140">`reloadOnChange` は、ASP.NET Core 1.1 以降でのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="b457b-140">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span> 

<span data-ttu-id="b457b-141">構成ソースは指定された順に読み取られます。</span><span class="sxs-lookup"><span data-stu-id="b457b-141">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="b457b-142">上記のコードでは、環境変数が最後に読み取られます。</span><span class="sxs-lookup"><span data-stu-id="b457b-142">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="b457b-143">環境を使用して設定された構成値で、2 つの前述のプロバイダーで設定された値が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="b457b-143">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="b457b-144">通常、環境は `Development`、`Staging`、または `Production` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-144">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="b457b-145">詳細については、「[複数の環境の使用](xref:fundamentals/environments)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b457b-145">See [Working with multiple environments](xref:fundamentals/environments) for more information.</span></span>

<span data-ttu-id="b457b-146">構成に関する考慮事項:</span><span class="sxs-lookup"><span data-stu-id="b457b-146">Configuration considerations:</span></span>

* <span data-ttu-id="b457b-147">`IOptionsSnapshot` では、構成データを変更時に再読み込みできます。</span><span class="sxs-lookup"><span data-stu-id="b457b-147">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="b457b-148">詳細については、「[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b457b-148">See [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="b457b-149">構成キーは大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="b457b-149">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="b457b-150">展開された構成ファイルの設定をローカル環境がオーバーライドできるように、最後に環境変数を指定します。</span><span class="sxs-lookup"><span data-stu-id="b457b-150">Specify environment variables last so that the local environment can override settings in deployed configuration files.</span></span>
* <span data-ttu-id="b457b-151">構成プロバイダー コードやプレーンテキストの構成ファイルには、パスワードなどの機密データは格納**しないでください**。</span><span class="sxs-lookup"><span data-stu-id="b457b-151">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="b457b-152">開発環境やテスト環境では運用シークレットを使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="b457b-152">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="b457b-153">代わりに、プロジェクトの外部にシークレットを指定してください。そうすれば、誤ってリポジトリにコミットされることはありません。</span><span class="sxs-lookup"><span data-stu-id="b457b-153">Instead, specify secrets outside of the project so that they can't be accidentally committed to your repository.</span></span> <span data-ttu-id="b457b-154">[複数の環境での作業](xref:fundamentals/environments)および[開発中のアプリ シークレットの安全な格納場所](xref:security/app-secrets)の管理の詳細を確認してください。</span><span class="sxs-lookup"><span data-stu-id="b457b-154">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="b457b-155">ご利用のシステムの環境変数でコロン (`:`) を使用できない場合は、コロン (`:`) をアンダースコア 2 つ (`__`) に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="b457b-155">If a colon (`:`) can't be used in environment variables on your system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="b457b-156">メモリ内プロバイダーと POCO クラスへのバインディング</span><span class="sxs-lookup"><span data-stu-id="b457b-156">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="b457b-157">次の例では、メモリ内プロバイダーを使用して、クラスにバインドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b457b-157">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

<span data-ttu-id="b457b-158">構成値は文字列として返されますが、バインディングすることでプロジェクトを構築できます。</span><span class="sxs-lookup"><span data-stu-id="b457b-158">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="b457b-159">バインディングすることで POCO オブジェクトや、オブジェクト グラフ全体を取得できます。</span><span class="sxs-lookup"><span data-stu-id="b457b-159">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="b457b-160">GetValue</span><span class="sxs-lookup"><span data-stu-id="b457b-160">GetValue</span></span>

<span data-ttu-id="b457b-161">次の例では、[GetValue&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) 拡張メソッドを示します。</span><span class="sxs-lookup"><span data-stu-id="b457b-161">The following sample demonstrates the [GetValue&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

<span data-ttu-id="b457b-162">ConfigurationBinder の `GetValue<T>` メソッドでは、既定値 (サンプルでは 80) を指定できます。</span><span class="sxs-lookup"><span data-stu-id="b457b-162">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="b457b-163">`GetValue<T>` は単純なシナリオ用であり、セクション全体にはバインドされません。</span><span class="sxs-lookup"><span data-stu-id="b457b-163">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="b457b-164">`GetValue<T>` は、特定の型に変換された `GetSection(key).Value` からスカラー値を取得します。</span><span class="sxs-lookup"><span data-stu-id="b457b-164">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="b457b-165">オブジェクト グラフにバインドする</span><span class="sxs-lookup"><span data-stu-id="b457b-165">Bind to an object graph</span></span>

<span data-ttu-id="b457b-166">クラス内の各オブジェクトに再帰的にバインドすることができます。</span><span class="sxs-lookup"><span data-stu-id="b457b-166">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="b457b-167">次の `AppSettings` クラスがあるとします。</span><span class="sxs-lookup"><span data-stu-id="b457b-167">Consider the following `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="b457b-168">次のサンプルでは `AppSettings` クラスにバインドします。</span><span class="sxs-lookup"><span data-stu-id="b457b-168">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="b457b-169">**ASP.NET Core 1.1** 以上では、セクション全体を操作する `Get<T>` を使用できます。</span><span class="sxs-lookup"><span data-stu-id="b457b-169">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="b457b-170">`Get<T>` は `Bind` を使用するよりも便利な場合があります。</span><span class="sxs-lookup"><span data-stu-id="b457b-170">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="b457b-171">次のコードでは、上記のサンプルで `Get<T>` を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b457b-171">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="b457b-172">以下の *appsettings.json* ファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="b457b-172">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="b457b-173">プログラムは `Height 11` を表示します。</span><span class="sxs-lookup"><span data-stu-id="b457b-173">The program displays `Height 11`.</span></span>

<span data-ttu-id="b457b-174">次のコードを使用して、構成の単体テストを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="b457b-174">The following code can be used to unit test the configuration:</span></span>

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="b457b-175">Entity Framework のカスタム プロバイダーを作成する</span><span class="sxs-lookup"><span data-stu-id="b457b-175">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="b457b-176">このセクションでは、EF を使用してデータベースから名前と値のペアを読み取る基本的な構成プロバイダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-176">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="b457b-177">データベースに構成値を格納するための `ConfigurationValue` エンティティを定義します。</span><span class="sxs-lookup"><span data-stu-id="b457b-177">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="b457b-178">構成した値を格納し、その値にアクセスするための `ConfigurationContext` を追加します。</span><span class="sxs-lookup"><span data-stu-id="b457b-178">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="b457b-179">[IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource) を実装するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="b457b-179">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="b457b-180">[ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider) から継承して、カスタム構成プロバイダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="b457b-180">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="b457b-181">構成プロバイダーは、空の場合にデータベースを初期化します。</span><span class="sxs-lookup"><span data-stu-id="b457b-181">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="b457b-182">サンプルを実行すると、データベースからの強調表示された値 ("value_from_ef_1" および "value_from_ef_2") が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-182">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="b457b-183">構成ソースを追加するための `EFConfigSource` 拡張メソッドを追加できます。</span><span class="sxs-lookup"><span data-stu-id="b457b-183">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="b457b-184">次のコードに、カスタム `EFConfigProvider` の使用方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b457b-184">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="b457b-185">サンプルでは JSON プロバイダーの後にカスタム `EFConfigProvider` を追加するため、データベースからのすべての設定で *appsettings.json* ファイルからの設定がオーバーライドされることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="b457b-185">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="b457b-186">以下の *appsettings.json* ファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="b457b-186">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="b457b-187">次が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-187">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="b457b-188">CommandLine 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="b457b-188">CommandLine configuration provider</span></span>

<span data-ttu-id="b457b-189">[CommandLine 構成プロバイダー](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider)は、実行時に構成するコマンドライン引数のキーと値のペアを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="b457b-189">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="b457b-190">CommandLine 構成サンプルを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="b457b-190">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="b457b-191">CommandLine 構成プロバイダーをセットアップして使用する</span><span class="sxs-lookup"><span data-stu-id="b457b-191">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="b457b-192">基本構成</span><span class="sxs-lookup"><span data-stu-id="b457b-192">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="b457b-193">コマンド ライン構成をアクティブにするには、[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) のインスタンスで `AddCommandLine` 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="b457b-193">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="b457b-194">コードを実行すると、次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-194">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="b457b-195">コマンド ラインで引数のキーと値のペアを渡すと、`Profile:MachineName` と `App:MainWindow:Left` の値が変更されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-195">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="b457b-196">コンソール ウィンドウには次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-196">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="b457b-197">他の構成プロバイダーによって提供された構成をコマンドライン構成でオーバーライドするには、`ConfigurationBuilder` で最後に `AddCommandLine` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="b457b-197">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b457b-198">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b457b-198">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b457b-199">一般的な ASP.NET Core 2.x アプリでは、便利な静的メソッド `CreateDefaultBuilder` を使用してホストを構築します。</span><span class="sxs-lookup"><span data-stu-id="b457b-199">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="b457b-200">`CreateDefaultBuilder` では、省略可能構成を *appsettings.json*、*appsettings.{Environment}.json*、[user secrets](xref:security/app-secrets) (`Development` 環境の場合)、環境変数、およびコマンドライン引数から読み込みます。</span><span class="sxs-lookup"><span data-stu-id="b457b-200">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="b457b-201">CommandLine 構成プロバイダーは最後に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-201">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="b457b-202">最後にプロバイダーを呼び出すことにより、実行時に渡されたコマンドライン引数で、前に呼び出された他の構成プロバイダーによって設定された構成をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="b457b-202">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="b457b-203">*appsettings* ファイルで `reloadOnChange` が有効になっていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="b457b-203">Note that for *appsettings* files that `reloadOnChange` is enabled.</span></span> <span data-ttu-id="b457b-204">*appsettings* ファイルの一致する構成値がアプリの起動後に変更された場合は、コマンドライン引数がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="b457b-204">Command-line arguments are overridden if a matching configuration value in an *appsettings* file is changed after the app starts.</span></span>

> [!NOTE]
> <span data-ttu-id="b457b-205">ASP.NET Core 2.x では、`CreateDefaultBuilder` メソッドを使用する代わりに、[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) を使用してホストを作成し、[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) を使用して手動で構成をビルドする方法がサポートされます。</span><span class="sxs-lookup"><span data-stu-id="b457b-205">As an alternative to using the `CreateDefaultBuilder` method, creating a host using [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) and manually building configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) is supported in ASP.NET Core 2.x.</span></span> <span data-ttu-id="b457b-206">詳細については、ASP.NET Core 1.x のタブを参照してください。</span><span class="sxs-lookup"><span data-stu-id="b457b-206">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b457b-207">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b457b-207">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b457b-208">[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) を作成し、`AddCommandLine` メソッドを呼び出して CommandLine 構成プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="b457b-208">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="b457b-209">最後にプロバイダーを呼び出すことにより、実行時に渡されたコマンドライン引数で、前に呼び出された他の構成プロバイダーによって設定された構成をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="b457b-209">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="b457b-210">`UseConfiguration` メソッドを使用して、[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="b457b-210">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="b457b-211">引数</span><span class="sxs-lookup"><span data-stu-id="b457b-211">Arguments</span></span>

<span data-ttu-id="b457b-212">コマンド ラインで渡される引数は、次の表に示すように 2 つの形式のいずれかに従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="b457b-212">Arguments passed on the command line must conform to one of two formats shown in the following table.</span></span>

| <span data-ttu-id="b457b-213">引数の形式</span><span class="sxs-lookup"><span data-stu-id="b457b-213">Argument format</span></span>                                                     | <span data-ttu-id="b457b-214">例</span><span class="sxs-lookup"><span data-stu-id="b457b-214">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="b457b-215">単一引数: 等号 (`=`) で区切られたキーと値のペア</span><span class="sxs-lookup"><span data-stu-id="b457b-215">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="b457b-216">2 つの引数のシーケンス: スペースで区切られたキーと値のペア</span><span class="sxs-lookup"><span data-stu-id="b457b-216">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="b457b-217">**単一引数**</span><span class="sxs-lookup"><span data-stu-id="b457b-217">**Single argument**</span></span>

<span data-ttu-id="b457b-218">値は等号 (`=`) の後に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b457b-218">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="b457b-219">値を null にすることができます (例: `mykey=`)。</span><span class="sxs-lookup"><span data-stu-id="b457b-219">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="b457b-220">キーにはプレフィックスが付く場合があります。</span><span class="sxs-lookup"><span data-stu-id="b457b-220">The key may have a prefix.</span></span>

| <span data-ttu-id="b457b-221">キーのプレフィックス</span><span class="sxs-lookup"><span data-stu-id="b457b-221">Key prefix</span></span>               | <span data-ttu-id="b457b-222">例</span><span class="sxs-lookup"><span data-stu-id="b457b-222">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="b457b-223">プレフィックスなし</span><span class="sxs-lookup"><span data-stu-id="b457b-223">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="b457b-224">単一のダッシュ (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="b457b-224">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="b457b-225">2 つのダッシュ (`--`)</span><span class="sxs-lookup"><span data-stu-id="b457b-225">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="b457b-226">スラッシュ (`/`)</span><span class="sxs-lookup"><span data-stu-id="b457b-226">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="b457b-227">&#8224;単一のダッシュ プレフィックス (`-`) が付いたキーは、[スイッチ マッピング](#switch-mappings)で指定する必要があります (以下を参照)。</span><span class="sxs-lookup"><span data-stu-id="b457b-227">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="b457b-228">コマンド例:</span><span class="sxs-lookup"><span data-stu-id="b457b-228">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="b457b-229">注: `-key1` が構成プロバイダーに指定された[スイッチ マッピング](#switch-mappings)に表示されていない場合は、`FormatException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="b457b-229">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="b457b-230">**2 つの引数のシーケンス**</span><span class="sxs-lookup"><span data-stu-id="b457b-230">**Sequence of two arguments**</span></span>

<span data-ttu-id="b457b-231">値を null にすることはできません。値はスペースで区切られたキーの後に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b457b-231">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="b457b-232">キーにはプレフィックスを付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="b457b-232">The key must have a prefix.</span></span>

| <span data-ttu-id="b457b-233">キーのプレフィックス</span><span class="sxs-lookup"><span data-stu-id="b457b-233">Key prefix</span></span>               | <span data-ttu-id="b457b-234">例</span><span class="sxs-lookup"><span data-stu-id="b457b-234">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="b457b-235">単一のダッシュ (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="b457b-235">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="b457b-236">2 つのダッシュ (`--`)</span><span class="sxs-lookup"><span data-stu-id="b457b-236">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="b457b-237">スラッシュ (`/`)</span><span class="sxs-lookup"><span data-stu-id="b457b-237">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="b457b-238">&#8224;単一のダッシュ プレフィックス (`-`) が付いたキーは、[スイッチ マッピング](#switch-mappings)で指定する必要があります (以下を参照)。</span><span class="sxs-lookup"><span data-stu-id="b457b-238">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="b457b-239">コマンド例:</span><span class="sxs-lookup"><span data-stu-id="b457b-239">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="b457b-240">注: `-key1` が構成プロバイダーに指定された[スイッチ マッピング](#switch-mappings)に表示されていない場合は、`FormatException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="b457b-240">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="b457b-241">重複キー</span><span class="sxs-lookup"><span data-stu-id="b457b-241">Duplicate keys</span></span>

<span data-ttu-id="b457b-242">重複キーが指定されている場合は、最後のキーと値のペアが使用されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-242">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="b457b-243">スイッチ マッピング</span><span class="sxs-lookup"><span data-stu-id="b457b-243">Switch mappings</span></span>

<span data-ttu-id="b457b-244">`ConfigurationBuilder` を使用して構成を手動でビルドする場合、必要に応じて、スイッチ マッピング ディクショナリを `AddCommandLine` メソッドに指定できます。</span><span class="sxs-lookup"><span data-stu-id="b457b-244">When manually building configuration with `ConfigurationBuilder`, you can optionally provide a switch mappings dictionary to the `AddCommandLine` method.</span></span> <span data-ttu-id="b457b-245">スイッチ マッピングでは、キー名の交換ロジックを指定できます。</span><span class="sxs-lookup"><span data-stu-id="b457b-245">Switch mappings allow you to provide key name replacement logic.</span></span>

<span data-ttu-id="b457b-246">スイッチ マッピング ディクショナリが使用されている場合、そのディレクトリで、コマンドライン引数によって指定されたキーと一致するキーが確認されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-246">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="b457b-247">ディクショナリでコマンドライン キーが見つかった場合は、構成を設定するためにディクショナリの値 (キー交換) が返されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-247">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="b457b-248">スイッチ マッピングは、単一のダッシュ (`-`) が前に付いたすべてのコマンドライン キーに必要です。</span><span class="sxs-lookup"><span data-stu-id="b457b-248">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="b457b-249">スイッチ マッピング ディクショナリ キーの規則:</span><span class="sxs-lookup"><span data-stu-id="b457b-249">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="b457b-250">スイッチはダッシュ (`-`) または二重ダッシュ (`--`) で開始する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b457b-250">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="b457b-251">スイッチ マッピング ディクショナリに重複キーを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="b457b-251">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="b457b-252">以下の例では、`GetSwitchMappings` メソッドを使用します。これにより、コマンドライン引数で単一ダッシュ (`-`) キー プレフィックスを使用でき、先頭にサブキーのプレフィックスが付かないようにすることができます。</span><span class="sxs-lookup"><span data-stu-id="b457b-252">In the following example, the `GetSwitchMappings` method allows your command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="b457b-253">コマンドライン引数を指定しないと、`AddInMemoryCollection` に指定されたディクショナリで構成値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-253">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="b457b-254">次のコマンドでアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="b457b-254">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="b457b-255">コンソール ウィンドウには次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-255">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="b457b-256">構成設定を渡す場合は、以下のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="b457b-256">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="b457b-257">コンソール ウィンドウには次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-257">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="b457b-258">スイッチ マッピング ディクショナリが作成されると、以下の表に示すデータが含まれます。</span><span class="sxs-lookup"><span data-stu-id="b457b-258">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="b457b-259">キー</span><span class="sxs-lookup"><span data-stu-id="b457b-259">Key</span></span>            | <span data-ttu-id="b457b-260">値</span><span class="sxs-lookup"><span data-stu-id="b457b-260">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="b457b-261">ディクショナリを使用してキーの切り替えを示すために、以下のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="b457b-261">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="b457b-262">コマンドライン キーが交換されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-262">The command-line keys are swapped.</span></span> <span data-ttu-id="b457b-263">コンソール ウィンドウに、`Profile:MachineName` と `App:MainWindow:Left` の構成値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-263">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="b457b-264">web.config ファイル</span><span class="sxs-lookup"><span data-stu-id="b457b-264">The web.config file</span></span>

<span data-ttu-id="b457b-265">IIS または IIS-Express でアプリをホストする場合は、*web.config* ファイルが必要です。</span><span class="sxs-lookup"><span data-stu-id="b457b-265">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="b457b-266">*web.config* は、IIS の AspNetCoreModule を有効にしてアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="b457b-266">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="b457b-267">*web.config* を設定することで、IIS の AspNetCoreModule を有効にし、アプリを起動して他の IIS 設定とモジュールを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="b457b-267">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="b457b-268">Visual Studio を使用して *web.config* を削除すると、Visual Studio で新しく作成されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-268">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="b457b-269">補足メモ</span><span class="sxs-lookup"><span data-stu-id="b457b-269">Additional notes</span></span>

* <span data-ttu-id="b457b-270">依存関係挿入 (DI) は、`ConfigureServices` が呼び出されるまでセットアップされません。</span><span class="sxs-lookup"><span data-stu-id="b457b-270">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="b457b-271">構成システムは DI に対応していません。</span><span class="sxs-lookup"><span data-stu-id="b457b-271">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="b457b-272">`IConfiguration` には次の 2 つ特性があります。</span><span class="sxs-lookup"><span data-stu-id="b457b-272">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="b457b-273">`IConfigurationRoot`  ルート ノードに使用されます。</span><span class="sxs-lookup"><span data-stu-id="b457b-273">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="b457b-274">再読み込みをトリガーできます。</span><span class="sxs-lookup"><span data-stu-id="b457b-274">Can trigger a reload.</span></span>
  * <span data-ttu-id="b457b-275">`IConfigurationSection`  構成値のセクションを表します。</span><span class="sxs-lookup"><span data-stu-id="b457b-275">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="b457b-276">`GetSection` および `GetChildren` メソッドは `IConfigurationSection` を返します。</span><span class="sxs-lookup"><span data-stu-id="b457b-276">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b457b-277">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="b457b-277">Additional resources</span></span>

* [<span data-ttu-id="b457b-278">オプション</span><span class="sxs-lookup"><span data-stu-id="b457b-278">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="b457b-279">複数の環境の使用</span><span class="sxs-lookup"><span data-stu-id="b457b-279">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="b457b-280">開発中のアプリ シークレットの安全な保存</span><span class="sxs-lookup"><span data-stu-id="b457b-280">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="b457b-281">ASP.NET Core でのホスティング</span><span class="sxs-lookup"><span data-stu-id="b457b-281">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="b457b-282">依存性の注入</span><span class="sxs-lookup"><span data-stu-id="b457b-282">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="b457b-283">Azure Key Vault 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="b457b-283">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)