---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: "创建自定义路由约束 (VB) |Microsoft 文档"
author: StephenWalther
description: "Stephen Walther 演示如何创建自定义的路由约束。 我们实现一个简单阻止路由的自定义约束匹配 w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 41b04c1fea267e7ee9f8a0b1c2f0d4fe4bb96d15
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-route-constraint-vb"></a><span data-ttu-id="f4f57-104">创建自定义路由约束 (VB)</span><span class="sxs-lookup"><span data-stu-id="f4f57-104">Creating a Custom Route Constraint (VB)</span></span>
====================
<span data-ttu-id="f4f57-105">通过[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="f4f57-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="f4f57-106">Stephen Walther 演示如何创建自定义的路由约束。</span><span class="sxs-lookup"><span data-stu-id="f4f57-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="f4f57-107">我们实现简单的自定义约束，以防止从远程计算机进行浏览器请求时要匹配的路由。</span><span class="sxs-lookup"><span data-stu-id="f4f57-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="f4f57-108">本教程的目标是演示如何创建自定义的路由约束。</span><span class="sxs-lookup"><span data-stu-id="f4f57-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="f4f57-109">自定义的路由约束，可防止一个路由，除非匹配某些自定义的条件匹配。</span><span class="sxs-lookup"><span data-stu-id="f4f57-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="f4f57-110">在本教程中，我们创建 Localhost 路由约束。</span><span class="sxs-lookup"><span data-stu-id="f4f57-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="f4f57-111">Localhost 路由约束仅匹配来自本地计算机发出的请求。</span><span class="sxs-lookup"><span data-stu-id="f4f57-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="f4f57-112">通过 Internet 从远程请求都不匹配。</span><span class="sxs-lookup"><span data-stu-id="f4f57-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="f4f57-113">通过实现 IRouteConstraint 接口实现一个自定义的路由约束。</span><span class="sxs-lookup"><span data-stu-id="f4f57-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="f4f57-114">这是一个极简单的接口，其中介绍了一种方法：</span><span class="sxs-lookup"><span data-stu-id="f4f57-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="f4f57-115">该方法返回一个布尔值。</span><span class="sxs-lookup"><span data-stu-id="f4f57-115">The method returns a Boolean value.</span></span> <span data-ttu-id="f4f57-116">如果返回 False，则与约束关联的路由不会匹配浏览器请求。</span><span class="sxs-lookup"><span data-stu-id="f4f57-116">If you return False, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="f4f57-117">列表 1 中包含的 Localhost 约束。</span><span class="sxs-lookup"><span data-stu-id="f4f57-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="f4f57-118">**列表 1-LocalhostConstraint.vb**</span><span class="sxs-lookup"><span data-stu-id="f4f57-118">**Listing 1 - LocalhostConstraint.vb**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="f4f57-119">列表 1 中的约束充分利用由 HttpRequest 类公开 IsLocal 属性。</span><span class="sxs-lookup"><span data-stu-id="f4f57-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="f4f57-120">该请求的 IP 地址是任一 127.0.0.1 或请求的 IP 等同于服务器的 IP 地址时，此属性返回 true。</span><span class="sxs-lookup"><span data-stu-id="f4f57-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="f4f57-121">使用自定义在 Global.asax 文件中定义的路由约束。</span><span class="sxs-lookup"><span data-stu-id="f4f57-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="f4f57-122">列出 2 中的 Global.asax 文件使用 Localhost 约束以防止任何人都要求管理员页，除非它们从本地服务器发出请求。</span><span class="sxs-lookup"><span data-stu-id="f4f57-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="f4f57-123">例如，/Admin/DeleteAll 的请求将失败时从远程服务器进行。</span><span class="sxs-lookup"><span data-stu-id="f4f57-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="f4f57-124">**列出 2-Global.asax**</span><span class="sxs-lookup"><span data-stu-id="f4f57-124">**Listing 2 - Global.asax**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="f4f57-125">管理员路由定义中使用 Localhost 约束。</span><span class="sxs-lookup"><span data-stu-id="f4f57-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="f4f57-126">此路由将不匹配的远程浏览器请求。</span><span class="sxs-lookup"><span data-stu-id="f4f57-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="f4f57-127">但是，请注意，在 Global.asax 中定义的其他路由可能匹配同一请求。</span><span class="sxs-lookup"><span data-stu-id="f4f57-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="f4f57-128">请务必了解约束可以防止特定路由匹配请求以及在 Global.asax 文件中定义并不是所有路由。</span><span class="sxs-lookup"><span data-stu-id="f4f57-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="f4f57-129">请注意，默认路由已被注释掉列出 2 中的 Global.asax 文件。</span><span class="sxs-lookup"><span data-stu-id="f4f57-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="f4f57-130">如果包括默认路由，默认路由将匹配的管理控制器的请求。</span><span class="sxs-lookup"><span data-stu-id="f4f57-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="f4f57-131">在这种情况下，远程用户仍可以调用的管理控制器的操作，即使它们的请求不匹配的管理路由。</span><span class="sxs-lookup"><span data-stu-id="f4f57-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="f4f57-132">上一篇</span><span class="sxs-lookup"><span data-stu-id="f4f57-132">Previous</span></span>](creating-a-route-constraint-vb.md)