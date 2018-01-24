---
title: "ASP.NET Core アプリケーションで開いているリダイレクト攻撃の防止 |Microsoft ドキュメント"
author: ardalis
description: "ASP.NET Core アプリケーションに対して開くリダイレクト攻撃を防止する方法を示します"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/preventing-open-redirects
ms.openlocfilehash: e57ae429e9af54ade74485361ba591cb75c16752
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="preventing-open-redirect-attacks-in-an-aspnet-core-app"></a><span data-ttu-id="a4b93-103">ASP.NET Core アプリケーションで開いているリダイレクト攻撃の防止</span><span class="sxs-lookup"><span data-stu-id="a4b93-103">Preventing Open Redirect Attacks in an ASP.NET Core app</span></span>

<span data-ttu-id="a4b93-104">クエリ文字列またはフォームのデータなどの要求で指定されている URL にリダイレクトする web アプリ可能性のある改ざんされるおそれがユーザーをリダイレクトする、悪意のある外部の url。</span><span class="sxs-lookup"><span data-stu-id="a4b93-104">A web app that redirects to a URL that is specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="a4b93-105">このによって改ざんされると、開いているリダイレクト攻撃が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a4b93-105">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="a4b93-106">アプリケーション ロジックは、指定された URL へリダイレクトされるたびに、リダイレクト URL 改ざんされていないことを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a4b93-106">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="a4b93-107">ASP.NET Core では、アプリを開いているリダイレクト (開いているとも呼ばれるリダイレクト) 攻撃から保護するための組み込みの機能を持ちます。</span><span class="sxs-lookup"><span data-stu-id="a4b93-107">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="a4b93-108">開いているリダイレクト攻撃とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="a4b93-108">What is an open redirect attack?</span></span>

<span data-ttu-id="a4b93-109">Web アプリケーションは、認証を必要とするリソースにアクセスするとき頻繁にユーザーとログイン ページにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="a4b93-109">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="a4b93-110">リダイレクト typlically が含まれています、 `returnUrl` querystring パラメーターが正常にログインした後、ユーザーが最初に要求された URL に返されるようにします。</span><span class="sxs-lookup"><span data-stu-id="a4b93-110">The redirection typlically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="a4b93-111">ユーザーが認証されると、最初に要求した URL にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="a4b93-111">After the user authenticates, they are redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="a4b93-112">リンク先の URL が指定されて、要求のクエリ文字列の悪意のあるユーザーが、クエリ文字列を改ざん可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a4b93-112">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="a4b93-113">改ざんされたクエリ文字列には、外部、悪意のあるサイトにユーザーをリダイレクトするサイトを許可できます。</span><span class="sxs-lookup"><span data-stu-id="a4b93-113">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="a4b93-114">この手法は、開いているリダイレクト (またはリダイレクト) 攻撃と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a4b93-114">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="a4b93-115">例攻撃</span><span class="sxs-lookup"><span data-stu-id="a4b93-115">An example attack</span></span>

<span data-ttu-id="a4b93-116">悪意のあるユーザーは、悪意のあるユーザー、ユーザーの資格情報やアプリで機密情報へのアクセスを許可するためのもので、攻撃を開発できます。</span><span class="sxs-lookup"><span data-stu-id="a4b93-116">A malicious user could develop an attack intended to allow the malicious user access to a user's credentials or sensitive information on your app.</span></span> <span data-ttu-id="a4b93-117">サイトのログイン ページへのリンクをクリックして、ユーザーを誘導する攻撃を開始する、 `returnUrl` querystring 値の URL に追加します。</span><span class="sxs-lookup"><span data-stu-id="a4b93-117">To begin the attack, they convince the user to click a link to your site's login page, with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="a4b93-118">たとえば、 [NerdDinner.com](http://nerddinner.com) (ASP.NET MVC の記述) サンプル アプリケーションには、このようなログイン ページは、ここが含まれています:``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``です。</span><span class="sxs-lookup"><span data-stu-id="a4b93-118">For example, the [NerdDinner.com](http://nerddinner.com) sample application (written for ASP.NET MVC) includes such a login page here: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span></span> <span data-ttu-id="a4b93-119">攻撃には、これらの手順を実行し、次に示します。</span><span class="sxs-lookup"><span data-stu-id="a4b93-119">The attack then follows these steps:</span></span>

1. <span data-ttu-id="a4b93-120">ユーザーへのリンクをクリックした``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn``(に注意してください、2 番目の URL は nerddi**n**er、いない nerddi**nn**er)。</span><span class="sxs-lookup"><span data-stu-id="a4b93-120">User clicks a link to ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (note, second URL is nerddi**n**er, not nerddi**nn**er).</span></span>
2. <span data-ttu-id="a4b93-121">ユーザーが正常にログインします。</span><span class="sxs-lookup"><span data-stu-id="a4b93-121">The user logs in successfully.</span></span>
3. <span data-ttu-id="a4b93-122">ユーザーがリダイレクト (サイト) によって``http://nerddiner.com/Account/LogOn``(実際のサイトのような悪意のあるサイト)。</span><span class="sxs-lookup"><span data-stu-id="a4b93-122">The user is redirected (by the site) to ``http://nerddiner.com/Account/LogOn`` (malicious site that looks like real site).</span></span>
4. <span data-ttu-id="a4b93-123">ユーザーがもう一度ログイン (自分の資格情報をサイト悪意のあること) と実際のサイトにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="a4b93-123">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="a4b93-124">ユーザーは可能性がありますと考えるログインに、最初の試行が失敗したし、その 2 つ目が成功しました。</span><span class="sxs-lookup"><span data-stu-id="a4b93-124">The user will likely believe their first attempt to log in failed, and their second one was successful.</span></span> <span data-ttu-id="a4b93-125">ほとんどの場合、対応していない残るでしょう自分の資格情報が侵害されていることができます。</span><span class="sxs-lookup"><span data-stu-id="a4b93-125">They'll most likely remain unaware their credentials have been compromised.</span></span>

![開いているリダイレクト攻撃プロセス](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="a4b93-127">ログイン ページだけでなく、一部のサイトはリダイレクト ページまたはエンドポイントを提供します。</span><span class="sxs-lookup"><span data-stu-id="a4b93-127">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="a4b93-128">アプリが開いているリダイレクトでは、含まれているページを想像``/Home/Redirect``です。</span><span class="sxs-lookup"><span data-stu-id="a4b93-128">Imagine your app has a page with an open redirect, ``/Home/Redirect``.</span></span> <span data-ttu-id="a4b93-129">攻撃者を作成して、たとえばに移動する電子メールのリンク``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``です。</span><span class="sxs-lookup"><span data-stu-id="a4b93-129">An attacker could create, for example, a link in an email that goes to ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span></span> <span data-ttu-id="a4b93-130">一般的なユーザーは、URL を見るし、サイト名で始まるを参照してください。</span><span class="sxs-lookup"><span data-stu-id="a4b93-130">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="a4b93-131">信頼する、リンクがクリックしてされます。</span><span class="sxs-lookup"><span data-stu-id="a4b93-131">Trusting that, they will click the link.</span></span> <span data-ttu-id="a4b93-132">開いているリダイレクトは自分のものと同じでは、フィッシング詐欺サイトにユーザーに送信し、およびユーザーがそうなと考える内容へのログインは、サイトです。</span><span class="sxs-lookup"><span data-stu-id="a4b93-132">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="a4b93-133">開いているリダイレクト攻撃から保護します。</span><span class="sxs-lookup"><span data-stu-id="a4b93-133">Protecting against open redirect attacks</span></span>

<span data-ttu-id="a4b93-134">Web アプリケーションを開発する場合は、すべてのユーザーが入力したデータを信頼できないとして扱います。</span><span class="sxs-lookup"><span data-stu-id="a4b93-134">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="a4b93-135">アプリケーションに、URL の内容に基づく、ユーザーをリダイレクトする機能がある場合は、このようなリダイレクトはのみ行うことローカルで、アプリ内で (または、既知の url、クエリ文字列で指定することがありますいない任意の URL) を確認します。</span><span class="sxs-lookup"><span data-stu-id="a4b93-135">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="a4b93-136">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="a4b93-136">LocalRedirect</span></span>

<span data-ttu-id="a4b93-137">使用して、``LocalRedirect``ベースからのヘルパー メソッド`Controller`クラス。</span><span class="sxs-lookup"><span data-stu-id="a4b93-137">Use the ``LocalRedirect`` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="a4b93-138">``LocalRedirect``非ローカル URL が指定されている場合、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a4b93-138">``LocalRedirect`` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="a4b93-139">それ以外の場合、動作と同じように、``Redirect``メソッドです。</span><span class="sxs-lookup"><span data-stu-id="a4b93-139">Otherwise, it behaves just like the ``Redirect`` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="a4b93-140">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="a4b93-140">IsLocalUrl</span></span>

<span data-ttu-id="a4b93-141">使用して、 [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_)をリダイレクトする前に Url をテストするメソッド。</span><span class="sxs-lookup"><span data-stu-id="a4b93-141">Use the [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="a4b93-142">次の例では、リダイレクトする前に、URL がローカルかどうかをチェックする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a4b93-142">The following example shows how to check whether a URL is local before redirecting.</span></span>

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

<span data-ttu-id="a4b93-143">`IsLocalUrl`メソッドは、悪意のあるサイトにリダイレクトされない誤ってユーザーを保護します。</span><span class="sxs-lookup"><span data-stu-id="a4b93-143">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="a4b93-144">ローカルの URL を予期したような状況で非ローカル URL が指定されている場合に指定された URL の詳細を記録できます。</span><span class="sxs-lookup"><span data-stu-id="a4b93-144">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="a4b93-145">ログイン リダイレクト Url ではリダイレクト攻撃を診断する際に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="a4b93-145">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>