---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: "ビルドおよび Web アプリケーション プロジェクトをパッケージ化 |Microsoft ドキュメント"
author: jrjlee
description: "リモート サーバー環境に web アプリケーション プロジェクトを配置する場合は、最初のタスクが、プロジェクトをビルドし、web 配置 packa を生成しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: c05f725c9e6b493a6af8f5b5d20dbc9ff73a1ef8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="building-and-packaging-web-application-projects"></a><span data-ttu-id="a0b2e-103">ビルドおよび Web アプリケーション プロジェクトをパッケージ化</span><span class="sxs-lookup"><span data-stu-id="a0b2e-103">Building and Packaging Web Application Projects</span></span>
====================
<span data-ttu-id="a0b2e-104">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="a0b2e-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="a0b2e-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="a0b2e-106">リモート サーバー環境に web アプリケーション プロジェクトを配置する場合は、最初のタスクは、プロジェクトをビルドし、web 配置パッケージの生成です。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-106">When you want to deploy a web application project to a remote server environment, your first task is to build the project and generate a web deployment package.</span></span> <span data-ttu-id="a0b2e-107">このトピックでは、web アプリケーション プロジェクトのビルド プロセスのしくみについて説明します。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-107">This topic describes how the build process works for web application projects.</span></span> <span data-ttu-id="a0b2e-108">具体的には、それについて説明します。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-108">In particular, it explains:</span></span>
> 
> - <span data-ttu-id="a0b2e-109">どの Web 発行パイプライン (WPP) は、展開の機能が含まれてに、ビルド プロセスを拡張します。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-109">How the Web Publishing Pipeline (WPP) extends the build process to include deployment functionality.</span></span>
> - <span data-ttu-id="a0b2e-110">どのように (Web 配置) インターネット インフォメーション サービス (IIS) Web 配置ツールには、展開パッケージに、web アプリケーションがオンにします。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-110">How the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) turns your web application into a deployment package.</span></span>
> - <span data-ttu-id="a0b2e-111">ビルドおよびパッケージ化プロセスの動作と、どのようなファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-111">How the build and packaging process works and what files are created.</span></span>


<span data-ttu-id="a0b2e-112">Visual Studio 2010 web アプリケーション プロジェクトのビルドおよび配置プロセスでは、WPP です。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-112">In Visual Studio 2010, the build and deployment process for web application projects is supported by the WPP.</span></span> <span data-ttu-id="a0b2e-113">WPP は、MSBuild の機能を拡張して、Web 配置と統合できるようにする Microsoft Build Engine (MSBuild) ターゲットのセットを提供します。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-113">The WPP provides a set of Microsoft Build Engine (MSBuild) targets that extend the functionality of MSBuild and enable it to integrate with Web Deploy.</span></span> <span data-ttu-id="a0b2e-114">Visual Studio 内で、web アプリケーション プロジェクトに、プロパティ ページでこの拡張機能を確認できます。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-114">Within Visual Studio, you can see this extended functionality on the property pages for your web application project.</span></span> <span data-ttu-id="a0b2e-115">**パッケージ化/発行 Web**  ページと連携して、**パッケージ化/発行 SQL**  ページで、web アプリケーション プロジェクトは展開のパッケージ化ビルド プロセスが完了する方法を構成できます。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-115">The **Package/Publish Web** page, together with the **Package/Publish SQL** page, lets you configure how your web application project is packaged for deployment when the build process is complete.</span></span>

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a><span data-ttu-id="a0b2e-116">WPP はどのように機能するのですか。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-116">How Does the WPP Work?</span></span>

<span data-ttu-id="a0b2e-117">C# プロジェクト ファイルを見てを行うかどうか、ベースの web アプリケーション プロジェクトでは、2 つの .targets ファイルをインポートすることが確認できます。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-117">If you take a look at the project file for a C#-based web application project, you can see that it imports two .targets files.</span></span>


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]


<span data-ttu-id="a0b2e-118">最初の**インポート**ステートメントは、すべての Visual c# プロジェクトに共通します。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-118">The first **Import** statement is common to all Visual C# projects.</span></span> <span data-ttu-id="a0b2e-119">このファイル*Microsoft.CSharp.targets*、ターゲットおよび Visual c# に固有のタスクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-119">This file, *Microsoft.CSharp.targets*, contains targets and tasks that are specific to Visual C#.</span></span> <span data-ttu-id="a0b2e-120">たとえば、c# コンパイラ (**Csc**) ここではタスクが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-120">For example, the C# compiler (**Csc**) task is invoked here.</span></span> <span data-ttu-id="a0b2e-121">*Microsoft.CSharp.targets*ファイルのインポートではさらに、 *Microsoft.Common.targets*ファイル。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-121">The *Microsoft.CSharp.targets* file in turn imports the *Microsoft.Common.targets* file.</span></span> <span data-ttu-id="a0b2e-122">同様に、すべてのプロジェクトに共通のターゲットを定義**ビルド**、**リビルド**、**実行**、**コンパイル**、および**クリーン**.</span><span class="sxs-lookup"><span data-stu-id="a0b2e-122">This defines targets that are common to all projects, like **Build**, **Rebuild**, **Run**, **Compile**, and **Clean**.</span></span> <span data-ttu-id="a0b2e-123">2 番目**インポート**ステートメントは、web アプリケーション プロジェクトに固有です。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-123">The second **Import** statement is specific to web application projects.</span></span> <span data-ttu-id="a0b2e-124">*ある Microsoft.WebApplication.targets*ファイルのインポートではさらに、 *Microsoft.Web.Publishing.targets*ファイル。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-124">The *Microsoft.WebApplication.targets* file in turn imports the *Microsoft.Web.Publishing.targets* file.</span></span> <span data-ttu-id="a0b2e-125">*Microsoft.Web.Publishing.targets*本質的にファイル*は*WPP です。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-125">The *Microsoft.Web.Publishing.targets* file essentially *is* the WPP.</span></span> <span data-ttu-id="a0b2e-126">同様に、ターゲットを定義する**パッケージ**と**MSDeployPublish**、Web デプロイのさまざまな展開タスクの完了を呼び出すことです。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-126">It defines targets, like **Package** and **MSDeployPublish**, that invoke Web Deploy to complete various deployment tasks.</span></span>

<span data-ttu-id="a0b2e-127">連絡先のマネージャーのサンプル ソリューションで、これらの追加のターゲットの使用方法について理解を開き、 *Publish.proj*ファイルし、を見て、 **BuildProjects**ターゲットです。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-127">To understand how these additional targets are used, in the Contact Manager sample solution, open the *Publish.proj* file and take a look at the **BuildProjects** target.</span></span>


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]


<span data-ttu-id="a0b2e-128">このターゲットを使用して、 **MSBuild**をさまざまなプロジェクトをビルドするタスク。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-128">This target uses the **MSBuild** task to build various projects.</span></span> <span data-ttu-id="a0b2e-129">通知、 **DeployOnBuild**と**DeployTarget**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-129">Notice the **DeployOnBuild** and **DeployTarget** properties:</span></span>

- <span data-ttu-id="a0b2e-130">**DeployOnBuild = true**プロパティは本来「するビルドが正常に完了したときに、追加のターゲットを実行します」という意味。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-130">The **DeployOnBuild=true** property essentially means "I want to execute an additional target when build completes successfully."</span></span>
- <span data-ttu-id="a0b2e-131">**DeployTarget**プロパティを識別するときに実行するターゲットの名前、 **DeployOnBuild**プロパティと等しい**true**です。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-131">The **DeployTarget** property identifies the name of the target you want to execute when the **DeployOnBuild** property is equal to **true**.</span></span> <span data-ttu-id="a0b2e-132">この場合、MSBuild を実行することを指定している、**パッケージ**プロジェクトのビルド後のターゲットです。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-132">In this case, you're specifying that you want MSBuild to execute the **Package** target after building the project.</span></span>

<span data-ttu-id="a0b2e-133">**パッケージ**でターゲットが定義されている、 *Microsoft.Web.Publishing.targets*ファイル。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-133">The **Package** target is defined in the *Microsoft.Web.Publishing.targets* file.</span></span> <span data-ttu-id="a0b2e-134">基本的には、このターゲットは、web アプリケーション プロジェクトのビルド出力を受け取るし、IIS web サーバーにパブリッシュできる web 配置パッケージに変換します。 します。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-134">Essentially, this target takes the build output of your web application project and turns it into a web deployment package that can be published to an IIS web server.</span></span>

> [!NOTE]
> <span data-ttu-id="a0b2e-135">プロジェクト ファイルを表示する (たとえば、 *ContactManager.Mvc.csproj*) Visual Studio 2010 では、まずソリューションからプロジェクトをアンロードします。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-135">To view a project file (for example, *ContactManager.Mvc.csproj*) in Visual Studio 2010, you first need to unload the project from your solution.</span></span> <span data-ttu-id="a0b2e-136">**ソリューション エクスプ ローラー**ウィンドウで、プロジェクト ノードを右クリックし、をクリックして**プロジェクトのアンロード**です。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-136">In the **Solution Explorer** window, right-click the project node, and then click **Unload Project**.</span></span> <span data-ttu-id="a0b2e-137">もう一度、プロジェクト ノードを右クリックし、をクリックして**編集***[プロジェクト ファイル]*)。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-137">Right-click the project node again, and then click **Edit***[project file]*).</span></span> <span data-ttu-id="a0b2e-138">プロジェクト ファイルは、その生の XML 形式で開かれます。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-138">The project file will open in its raw XML form.</span></span> <span data-ttu-id="a0b2e-139">完了したら、プロジェクトの再読み込みしてください。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-139">Remember to reload the project when you're done.</span></span>  
> <span data-ttu-id="a0b2e-140">詳細については、MSBuild のターゲット、タスク、および**インポート**ステートメントを参照してください[プロジェクト ファイルを理解する](understanding-the-project-file.md)です。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-140">For more information on MSBuild targets, tasks, and **Import** statements, see [Understanding the Project File](understanding-the-project-file.md).</span></span> <span data-ttu-id="a0b2e-141">プロジェクト ファイルと、WPP をより詳細な概要については、次を参照してください。[内の Microsoft Build Engine: MSBuild を使用して、Team Foundation ビルド](http://amzn.com/0735645248)Sayed Ibrahim Hashimi して William Bartholomew、ISBN: 978-0-7356-4524-0 です。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-141">For a more in-depth introduction to project files and the WPP, see [Inside the Microsoft Build Engine: Using MSBuild and Team Foundation Build](http://amzn.com/0735645248) by Sayed Ibrahim Hashimi and William Bartholomew, ISBN: 978-0-7356-4524-0.</span></span>


## <a name="what-is-a-web-deployment-package"></a><span data-ttu-id="a0b2e-142">Web 配置パッケージは何ですか。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-142">What Is a Web Deployment Package?</span></span>

<span data-ttu-id="a0b2e-143">ビルドし、Visual Studio 2010 を使用するか、直接 MSBuild を使用して、web アプリケーション プロジェクトを展開すると、最終的な結果は、通常、 *web 配置パッケージ*です。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-143">When you build and deploy a web application project, either by using Visual Studio 2010 or by using MSBuild directly, the end result is typically a *web deployment package*.</span></span> <span data-ttu-id="a0b2e-144">Web 配置パッケージは、.zip ファイルです。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-144">The web deployment package is a .zip file.</span></span> <span data-ttu-id="a0b2e-145">IIS を含むすべてのものと、web アプリケーションを再作成するために必要な Web 配置を含みます。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-145">It contains everything that IIS and Web Deploy need in order to recreate your web application, including:</span></span>

- <span data-ttu-id="a0b2e-146">コンテンツ、リソース ファイル、構成ファイル、JavaScript およびカスケード スタイル シート (CSS) のリソース、およびなどを含む web アプリケーションのコンパイル済みの出力。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-146">The compiled output of your web application, including content, resource files, configuration files, JavaScript and cascading style sheets (CSS) resources, and so on.</span></span>
- <span data-ttu-id="a0b2e-147">Web アプリケーション プロジェクトおよびすべてのアセンブリは、ソリューション内のプロジェクトを参照します。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-147">Assemblies for your web application project and for any referenced projects within your solution.</span></span>
- <span data-ttu-id="a0b2e-148">Web アプリケーションを配置する任意のデータベースを生成する SQL スクリプト。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-148">SQL scripts to generate any databases that you're deploying with your web application.</span></span>

<span data-ttu-id="a0b2e-149">Web 配置パッケージが生成されると、さまざまな方法で IIS web サーバーに発行することができます。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-149">Once the web deployment package has been generated, you can publish it to an IIS web server in various ways.</span></span> <span data-ttu-id="a0b2e-150">たとえば、Web デプロイのリモート エージェント サービスまたは Web 配置ハンドラーが、ターゲット web サーバーで、対象としてリモートで展開することができます。 または IIS マネージャーを使用して、ターゲット web サーバー上のパッケージを手動でインポートすることができます。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-150">For example, you can deploy it remotely by targeting the Web Deploy Remote Agent service or the Web Deploy Handler on the destination web server, or you can use IIS Manager to manually import the package on the destination web server.</span></span> <span data-ttu-id="a0b2e-151">展開に対するこれらの方法の詳細については、次を参照してください。 [Web 配置を右側の方法を選択する](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-151">For more information on these approaches to deployment, see [Choosing the Right Approach to Web Deployment](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).</span></span>

## <a name="how-does-the-build-process-work"></a><span data-ttu-id="a0b2e-152">ビルド処理のしくみ</span><span class="sxs-lookup"><span data-stu-id="a0b2e-152">How Does the Build Process Work?</span></span>

<span data-ttu-id="a0b2e-153">これは、ビルド、および web アプリケーション プロジェクトをパッケージ化するときの動作を示しています。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-153">This shows what happens when you build and package a web application project:</span></span>

![](building-and-packaging-web-application-projects/_static/image2.png)

<span data-ttu-id="a0b2e-154">ビルド プロセスがという名前のファイルを生成する web アプリケーション プロジェクトをビルドするときに*[プロジェクト名]。SourceManifest.xml*です。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-154">When you build a web application project, the build process generates a file named *[project name].SourceManifest.xml*.</span></span> <span data-ttu-id="a0b2e-155">プロジェクト ファイルとビルド出力と一緒にこの*です。SourceManifest.xml*ファイル指示 Web Deploy web 展開パッケージに含める必要があること。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-155">Along with the project file and the build output, this *.SourceManifest.xml* file tells Web Deploy what it needs to include in the web deployment package.</span></span> <span data-ttu-id="a0b2e-156">これらの入力を使用して、という名前の web 配置パッケージを生成 Web Deploy *[プロジェクト名] .zip*です。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-156">Using these inputs, Web Deploy generates a web deployment package named *[project name].zip*.</span></span>

<span data-ttu-id="a0b2e-157">Web 配置パッケージと共にビルド プロセスには、パッケージを使用するのに役立つ 2 つのファイルが生成されます。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-157">Alongside the web deployment package, the build process generates two files that can help you to use the package:</span></span>

- <span data-ttu-id="a0b2e-158">*. Deploy.cmd*ファイルには、リモートの IIS web サーバーに web 配置パッケージを発行する Web Deploy (MSDeploy.exe) のパラメーター化コマンドのセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-158">The *.deploy.cmd* file includes a set of parameterized Web Deploy (MSDeploy.exe) commands that publish your web deployment package to a remote IIS web server.</span></span> <span data-ttu-id="a0b2e-159">実行している、 *. deploy.cmd*ファイルに適切なパラメーターは、通常、早く簡単および代替が容易になります、MSDeploy.exe を手動で構築するコマンド示します自分でします。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-159">Running the *.deploy.cmd* file, with appropriate parameters, typically provides a quicker and easier alternative to manually constructing the MSDeploy.exe commands yourself.</span></span>
- <span data-ttu-id="a0b2e-160">*SetParameters.xml*ファイルは、MSDeploy.exe コマンドにパラメーター値のセットを提供します。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-160">The *SetParameters.xml* file provides a set of parameter values to the MSDeploy.exe command.</span></span> <span data-ttu-id="a0b2e-161">これらの値をサービス エンドポイントの値は、パッケージを展開して、接続文字列で定義されて、IIS の web アプリケーションの名前などのプロパティを含める、 *web.config*ファイル、および任意の展開プロパティプロジェクト プロパティ ページで定義されている値。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-161">These values include properties like the name of the IIS web application to which you want to deploy the package, the values of any service endpoints and connection strings defined in the *web.config* file, and any deployment property values defined on the project properties pages.</span></span>

<span data-ttu-id="a0b2e-162">*SetParameters.xml*ファイルが配置プロセスを管理するキー。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-162">The *SetParameters.xml* file is key to managing the deployment process.</span></span> <span data-ttu-id="a0b2e-163">このファイルは、web アプリケーション プロジェクトの内容に応じて動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-163">This file is generated dynamically according to the contents of your web application project.</span></span> <span data-ttu-id="a0b2e-164">たとえばへの接続文字列を追加する場合、 *web.config*ファイル、ビルド プロセスは自動的に接続文字列を検出、展開を同様に、パラメーター化および内のエントリを作成、 *SetParameters.xml*展開プロセスの一部として、接続文字列を変更することを許可するファイル。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-164">For example, if you add a connection string to your *web.config* file, the build process will automatically detect the connection string, parameterize the deployment accordingly, and create an entry in the *SetParameters.xml* file to allow you to modify the connection string as part of the deployment process.</span></span> <span data-ttu-id="a0b2e-165">次のトピックでは、 [Web パッケージの展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)さらに詳しくは、このファイルの役割について説明、および変更には、することができます、ビルドおよび配置中にさまざまな方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-165">The next topic, [Configuring Parameters for Web Package Deployment](configuring-parameters-for-web-package-deployment.md), explains the role of this file in more detail and describes the different ways in which you can modify it during build and deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="a0b2e-166">Visual Studio 2010 で、WPP はサポートされませんパッケージ化する前に web アプリケーションのページをプリコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-166">In Visual Studio 2010, the WPP does not support precompiling the pages in a web application prior to packaging.</span></span> <span data-ttu-id="a0b2e-167">次のバージョンの Visual Studio と WPP パッケージング オプションとして、web アプリケーションをプリコンパイルする機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-167">The next version of Visual Studio and the WPP will include the ability to precompile a web application as a packaging option.</span></span>


## <a name="conclusion"></a><span data-ttu-id="a0b2e-168">まとめ</span><span class="sxs-lookup"><span data-stu-id="a0b2e-168">Conclusion</span></span>

<span data-ttu-id="a0b2e-169">このトピックでは、Visual Studio 2010 で web アプリケーション プロジェクトのビルドとパッケージ化プロセスの概要が用意されています。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-169">This topic provided an overview of the build and packaging process for web application projects in Visual Studio 2010.</span></span> <span data-ttu-id="a0b2e-170">WPP を使用する MSBuild から、Web Deploy コマンドを呼び出す方法が説明されているし、ビルドとパッケージ化プロセスの動作に説明します。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-170">It described how the WPP lets you invoke Web Deploy commands from MSBuild, and it explained how the build and packaging process works.</span></span>

<span data-ttu-id="a0b2e-171">Web 配置パッケージを作成したら、次の手順では、展開を開始します。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-171">Once you've created a web deployment package, your next step is to deploy it.</span></span> <span data-ttu-id="a0b2e-172">詳細については、次を参照してください。 [Web パッケージの展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)と[Web パッケージを展開する](deploying-web-packages.md)です。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-172">For more information on this, see [Configuring Parameters for Web Package Deployment](configuring-parameters-for-web-package-deployment.md) and [Deploying Web Packages](deploying-web-packages.md).</span></span>

## <a name="further-reading"></a><span data-ttu-id="a0b2e-173">関連項目</span><span class="sxs-lookup"><span data-stu-id="a0b2e-173">Further Reading</span></span>

<span data-ttu-id="a0b2e-174">このチュートリアルでは、次のトピック[Web パッケージの展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)と[Web パッケージを展開する](deploying-web-packages.md)、作成した web パッケージを使用する方法についてガイダンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-174">The next topics in this tutorial, [Configuring Parameters for Web Package Deployment](configuring-parameters-for-web-package-deployment.md) and [Deploying Web Packages](deploying-web-packages.md), provide guidance on how to use the web package you've created.</span></span> <span data-ttu-id="a0b2e-175">このシリーズの最終的なチュートリアル[高度なエンタープライズ Web 展開](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)、カスタマイズおよびパッケージ化プロセスのトラブルシューティングを行う方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-175">The final tutorial in this series, [Advanced Enterprise Web Deployment](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), provides guidance on how to customize and troubleshoot the packaging process.</span></span>

<span data-ttu-id="a0b2e-176">プロジェクト ファイルと、WPP をより詳細な概要については、次を参照してください。[内の Microsoft Build Engine: MSBuild を使用して、Team Foundation ビルド](http://amzn.com/0735645248)Sayed Ibrahim Hashimi して William Bartholomew、ISBN: 978-0-7356-4524-0 です。</span><span class="sxs-lookup"><span data-stu-id="a0b2e-176">For a more in-depth introduction to project files and the WPP, see [Inside the Microsoft Build Engine: Using MSBuild and Team Foundation Build](http://amzn.com/0735645248) by Sayed Ibrahim Hashimi and William Bartholomew, ISBN: 978-0-7356-4524-0.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a0b2e-177">[前へ](understanding-the-build-process.md)
[次へ](configuring-parameters-for-web-package-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="a0b2e-177">[Previous](understanding-the-build-process.md)
[Next](configuring-parameters-for-web-package-deployment.md)</span></span>