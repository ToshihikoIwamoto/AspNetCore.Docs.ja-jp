---
title: "ホスティングと展開の概要 - ASP.NET Core"
author: tdykstra
description: "ホスティング環境を設定し、それらに ASP.NET Core アプリを展開する方法の概要。"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: f0930c68-4d17-4748-adbf-801e17601eb6
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/index
ms.openlocfilehash: 0de459128426c4d027606951592b1fe3fdd24fd9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="hosting-and-deployment-overview-for-aspnet-core-apps"></a><span data-ttu-id="8e782-104">ASP.NET Core アプリのホスティングと展開の概要</span><span class="sxs-lookup"><span data-stu-id="8e782-104">Hosting and deployment overview for ASP.NET Core apps</span></span>

<span data-ttu-id="8e782-105">ASP.NET Core アプリをホスティング環境に展開する場合に実行する主な手順を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="8e782-105">Here are the main steps you perform to deploy an ASP.NET Core app to a hosting environment:</span></span>

* <span data-ttu-id="8e782-106">ホスティング サーバー上のフォルダーにアプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="8e782-106">Publish the app to a folder on the hosting server.</span></span>
* <span data-ttu-id="8e782-107">要求の着信時にアプリを開始し、クラッシュ後、またはサーバーの再起動後に再始動するプロセス マネージャーを設定します。</span><span class="sxs-lookup"><span data-stu-id="8e782-107">Set up a process manager that starts the app when requests come in and restarts it after it crashes or the server reboots.</span></span>
* <span data-ttu-id="8e782-108">要求をアプリに転送するリバース プロキシを設定します。</span><span class="sxs-lookup"><span data-stu-id="8e782-108">Set up a reverse proxy that forwards requests to the app.</span></span>

## <a name="publish-to-a-folder"></a><span data-ttu-id="8e782-109">フォルダーに発行する</span><span class="sxs-lookup"><span data-stu-id="8e782-109">Publish to a folder</span></span> 

<span data-ttu-id="8e782-110">[dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) CLI コマンドはアプリケーション コードをコンパイルし、アプリケーションを実行するために必要なファイルを *publish* フォルダーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="8e782-110">The [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) CLI command compiles application code and copies the files needed to run the application into a *publish* folder.</span></span> <span data-ttu-id="8e782-111">Visual Studio から展開する場合は、ファイルが展開先にコピーされる前に `dotnet publish` ステップが自動的に行われます。</span><span class="sxs-lookup"><span data-stu-id="8e782-111">When you deploy from Visual Studio the `dotnet publish` step is done for you automatically before files are copied to the deployment destination.</span></span>

### <a name="folder-contents"></a><span data-ttu-id="8e782-112">フォルダーの内容</span><span class="sxs-lookup"><span data-stu-id="8e782-112">Folder contents</span></span>

<span data-ttu-id="8e782-113">*publish* フォルダーには、アプリケーションとその依存関係、および必要に応じて .NET ランタイムの *.exe* ファイルと *.dll* ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8e782-113">The *publish* folder contains *.exe* and *.dll* files for the application, its dependencies, and optionally the .NET runtime.</span></span>

<span data-ttu-id="8e782-114">.NET Core アプリは、*自己完結型*または*フレームワーク依存*として発行できます。</span><span class="sxs-lookup"><span data-stu-id="8e782-114">A .NET Core app can be published as *self-contained* or *framework-dependent*.</span></span> <span data-ttu-id="8e782-115">アプリが自己完結型の場合、.NET ランタイムを含む *.dll* ファイルが *publish* フォルダーに含まれています。</span><span class="sxs-lookup"><span data-stu-id="8e782-115">If the app is self-contained, the *.dll* files that contain the .NET runtime are included in the *publish* folder.</span></span>  <span data-ttu-id="8e782-116">アプリがフレームワークに依存する場合、.NET ランタイムのファイルは含まれていません。これは、コンピューターにインストールされている .NET のバージョンへの参照がアプリに含まれていないためです。</span><span class="sxs-lookup"><span data-stu-id="8e782-116">If the app is framework-dependent, the .NET runtime files are not included because the app has a reference to a version of .NET that is installed on the computer.</span></span> <span data-ttu-id="8e782-117">既定の展開モデルはフレームワークに依存します。</span><span class="sxs-lookup"><span data-stu-id="8e782-117">The default deployment model is framework-dependent.</span></span> <span data-ttu-id="8e782-118">詳細については、「[.NET Core アプリケーションの展開](https://docs.microsoft.com/dotnet/articles/core/deploying/index)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8e782-118">For more information, see [.NET Core application deployment](https://docs.microsoft.com/dotnet/articles/core/deploying/index).</span></span>

<span data-ttu-id="8e782-119">*.exe* ファイルと *.dll* ファイルに加え、ASP.NET Core アプリの *publish* フォルダーには、通常、構成ファイル、静的資産、および MVC ビューが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8e782-119">In addition to *.exe* and *.dll* files, the *publish* folder for an ASP.NET Core app typically contains configuration files, static assets, and MVC views.</span></span>  <span data-ttu-id="8e782-120">詳細については、[ディレクトリ構造](xref:hosting/directory-structure)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8e782-120">For more information, see [Directory structure](xref:hosting/directory-structure).</span></span>

## <a name="set-up-a-process-manager"></a><span data-ttu-id="8e782-121">プロセス マネージャーを設定する</span><span class="sxs-lookup"><span data-stu-id="8e782-121">Set up a process manager</span></span>

<span data-ttu-id="8e782-122">ASP.NET Core アプリは、サーバーの起動時に開始し、クラッシュ後に再始動する必要があるコンソール アプリです。</span><span class="sxs-lookup"><span data-stu-id="8e782-122">An ASP.NET Core app is a console app that has to be started when a server boots and restarted after crashes.</span></span> <span data-ttu-id="8e782-123">開始と再始動を自動化するには、プロセス マネージャーが必要です。</span><span class="sxs-lookup"><span data-stu-id="8e782-123">To automate starts and restarts you need a process manager.</span></span> <span data-ttu-id="8e782-124">ASP.NET Core の最も一般的なプロセス マネージャーは、Linux の場合は [Nginx](xref:publishing/linuxproduction) と [Apache](xref:publishing/apache-proxy)、Windows の場合は [IIS](xref:publishing/iis) と [Windows Service](xref:hosting/windows-service) です。</span><span class="sxs-lookup"><span data-stu-id="8e782-124">The most common process managers for ASP.NET Core are [Nginx](xref:publishing/linuxproduction) and [Apache](xref:publishing/apache-proxy) on Linux, and [IIS](xref:publishing/iis) and [Windows Service](xref:hosting/windows-service) on Windows.</span></span>

## <a name="set-up-a-reverse-proxy"></a><span data-ttu-id="8e782-125">リバース プロキシを設定する</span><span class="sxs-lookup"><span data-stu-id="8e782-125">Set up a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8e782-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8e782-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8e782-127">アプリで [Kestrel](xref:fundamentals/servers/kestrel) Web サーバーを使用する場合は、リバース プロキシ サーバーとして、[Nginx](xref:publishing/linuxproduction)、[Apache](xref:publishing/apache-proxy)、または [IIS](xref:publishing/iis) を使用できます。</span><span class="sxs-lookup"><span data-stu-id="8e782-127">If your app uses the [Kestrel](xref:fundamentals/servers/kestrel) web server, you can use [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy), or [IIS](xref:publishing/iis) as a reverse proxy server.</span></span> <span data-ttu-id="8e782-128">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="8e782-128">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span> <span data-ttu-id="8e782-129">詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8e782-129">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8e782-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8e782-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8e782-131">アプリを [Kestrel](xref:fundamentals/servers/kestrel) Web サーバーを使用して、インターネットに公開する場合は、リバース プロキシ サーバーとして、[Nginx](xref:publishing/linuxproduction)、[Apache](xref:publishing/apache-proxy)、または [IIS](xref:publishing/iis) を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8e782-131">If your app uses the [Kestrel](xref:fundamentals/servers/kestrel) web server and will be exposed to the Internet, you must use [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy), or [IIS](xref:publishing/iis) as a reverse proxy server.</span></span> <span data-ttu-id="8e782-132">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="8e782-132">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span> <span data-ttu-id="8e782-133">リバース プロキシを使用する主な理由はセキュリティです。</span><span class="sxs-lookup"><span data-stu-id="8e782-133">The main reason for using a reverse proxy is security.</span></span> <span data-ttu-id="8e782-134">詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8e782-134">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

---

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a><span data-ttu-id="8e782-135">Visual Studio と MSBuild を使用して展開を自動化する</span><span class="sxs-lookup"><span data-stu-id="8e782-135">Using Visual Studio and MSBuild to automate deployment</span></span>

<span data-ttu-id="8e782-136">多くの場合、展開には、`dotnet publish` からサーバーへの出力のコピー以外の追加の作業が必要になります。</span><span class="sxs-lookup"><span data-stu-id="8e782-136">Deployment often requires additional tasks besides copying the output from `dotnet publish` to a server.</span></span> <span data-ttu-id="8e782-137">たとえば、*publish* フォルダーに対するファイルの追加や除外が必要になります。</span><span class="sxs-lookup"><span data-stu-id="8e782-137">For example, you might want to include extra files in the *publish* folder, or exclude files from it.</span></span> <span data-ttu-id="8e782-138">Visual Studio では Web 展開で MSBuild を使用します。この MSBuild は、展開時に他の多くの作業を行うためにカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="8e782-138">Visual Studio uses MSBuild for web deployment, and you can customize MSBuild to do many other tasks during deployment.</span></span> <span data-ttu-id="8e782-139">詳細については、[Visual Studio の発行プロファイル](xref:publishing/web-publishing-vs)に関するページと、『[Using MSBuild and Team Foundation Build](http://msbuildbook.com/)』という書籍を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8e782-139">For more information, see [Publish profiles in Visual Studio](xref:publishing/web-publishing-vs) and the [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) book.</span></span>

<span data-ttu-id="8e782-140">[Web の発行機能](xref:tutorials/publish-to-azure-webapp-using-vs)または[組み込みの Git サポート](xref:publishing/azure-continuous-deployment)を使用して、Visual Studio から Azure App Service に直接展開することができます。</span><span class="sxs-lookup"><span data-stu-id="8e782-140">You can deploy directly from Visual Studio to Azure App Service by using [the Publish Web feature](xref:tutorials/publish-to-azure-webapp-using-vs) or by using [built-in Git support](xref:publishing/azure-continuous-deployment).</span></span> <span data-ttu-id="8e782-141">Visual Studio Team Services では、[Azure App Service への継続的な展開](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="8e782-141">Visual Studio Team Services supports [continuous deployment to Azure App Service](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure).</span></span>

## <a name="publishing-to-azure"></a><span data-ttu-id="8e782-142">Azure への発行</span><span class="sxs-lookup"><span data-stu-id="8e782-142">Publishing to Azure</span></span>

<span data-ttu-id="8e782-143">Visual Studio を使用してこのアプリを Azure に発行する方法については、「[Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する](xref:tutorials/publish-to-azure-webapp-using-vs)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="8e782-143">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish this app to Azure using Visual Studio.</span></span>  <span data-ttu-id="8e782-144">Azure へのアプリの発行は、[コマンド ライン](xref:tutorials/publish-to-azure-webapp-using-cli)で行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="8e782-144">The app can also be published to Azure from the [command line](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8e782-145">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="8e782-145">Additional resources</span></span>

<span data-ttu-id="8e782-146">ホスティング環境として Docker を使用する方法については、[Docker での ASP.NET Core アプリのホスティング](xref:publishing/docker)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8e782-146">For information about using Docker as a hosting environment, see [Host ASP.NET Core apps in Docker](xref:publishing/docker).</span></span>