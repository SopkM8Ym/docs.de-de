---
title: 'Installieren von .NET Core unter Alpine: .NET Core'
description: Hier werden verschiedene Möglichkeiten veranschaulicht, das.NET Core SDK und die NET Core-Runtime unter Alpine zu installieren.
author: thraka
ms.author: adegeo
ms.date: 06/04/2020
ms.openlocfilehash: bbaf4ee9020dcd33c894b5376bf04ad65db8d378
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/15/2020
ms.locfileid: "84771488"
---
# <a name="install-net-core-sdk-or-net-core-runtime-on-alpine"></a><span data-ttu-id="88af6-103">Installieren des .NET Core SDK oder der .NET Core-Runtime unter Alpine</span><span class="sxs-lookup"><span data-stu-id="88af6-103">Install .NET Core SDK or .NET Core Runtime on Alpine</span></span>

<span data-ttu-id="88af6-104">In diesem Artikel wird beschrieben, wie Sie .NET Core unter Alpine installieren.</span><span class="sxs-lookup"><span data-stu-id="88af6-104">This article describes how to install .NET Core on Alpine.</span></span> <span data-ttu-id="88af6-105">Wenn für eine Alpine-Version kein Support mehr geboten wird, wird .NET Core mit dieser Version nicht mehr unterstützt.</span><span class="sxs-lookup"><span data-stu-id="88af6-105">When a Alpine version falls out of support, .NET Core is no longer supported with that version.</span></span> <span data-ttu-id="88af6-106">Diese Anweisungen können Ihnen jedoch helfen, .NET Core in diesen Versionen auszuführen, auch wenn kein Support dafür geboten wird.</span><span class="sxs-lookup"><span data-stu-id="88af6-106">However, these instructions may help you to get .NET Core running on those versions, even though it isn't supported.</span></span>

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

<span data-ttu-id="88af6-107">Es sind keine Installationsprogramme für Alpine vorhanden.</span><span class="sxs-lookup"><span data-stu-id="88af6-107">There are no installers for Alpine.</span></span> <span data-ttu-id="88af6-108">Sie müssen entweder das [Installationsskript](#scripted-install) verwenden oder die Anweisungen zur [manuellen Installation](#manual-install) befolgen.</span><span class="sxs-lookup"><span data-stu-id="88af6-108">You must either use the [install script](#scripted-install) or follow the [manual install](#manual-install) instructions.</span></span>

## <a name="supported-distributions"></a><span data-ttu-id="88af6-109">Unterstützte Distributionen</span><span class="sxs-lookup"><span data-stu-id="88af6-109">Supported distributions</span></span>

<span data-ttu-id="88af6-110">Die folgende Tabelle enthält die derzeit unterstützten .NET Core-Releases und die Alpine-Versionen, die diese unterstützen.</span><span class="sxs-lookup"><span data-stu-id="88af6-110">The following table is a list of currently supported .NET Core releases and the versions of Alpine they're supported on.</span></span> <span data-ttu-id="88af6-111">Diese Versionen werden weiterhin unterstützt, bis entweder die Version von [.NET Core das Ende des Supports](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) oder die Version von [Alpine das Ende ihrer Lebensdauer erreicht](https://wiki.alpinelinux.org/wiki/Alpine_Linux:Releases).</span><span class="sxs-lookup"><span data-stu-id="88af6-111">These versions remain supported until either the version of [.NET Core reaches end-of-support](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) or the version of [Alpine reaches end-of-life](https://wiki.alpinelinux.org/wiki/Alpine_Linux:Releases).</span></span>

- <span data-ttu-id="88af6-112">✔️ gibt an, dass die Version von Alpine oder .NET Core weiterhin unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="88af6-112">A ✔️ indicates that the version of Alpine or .NET Core is still supported.</span></span>
- <span data-ttu-id="88af6-113">❌ gibt an, dass die Version von Alpine oder .NET Core in diesem Alpine-Release nicht unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="88af6-113">A ❌ indicates that the version of Alpine or .NET Core isn't supported on that Alpine release.</span></span>
- <span data-ttu-id="88af6-114">Wenn sowohl eine Version von Alpine als auch eine Version von .NET Core mit ✔️ versehen sind, wird diese Kombination aus Betriebssystem und .NET unterstützt.</span><span class="sxs-lookup"><span data-stu-id="88af6-114">When both a version of Alpine and a version of .NET Core have ✔️, that OS and .NET combination are supported.</span></span>

| <span data-ttu-id="88af6-115">Alpine</span><span class="sxs-lookup"><span data-stu-id="88af6-115">Alpine</span></span>                   | <span data-ttu-id="88af6-116">.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="88af6-116">.NET Core 2.1</span></span> | <span data-ttu-id="88af6-117">.NET Core 3.1</span><span class="sxs-lookup"><span data-stu-id="88af6-117">.NET Core 3.1</span></span> | <span data-ttu-id="88af6-118">.NET 5 Preview</span><span class="sxs-lookup"><span data-stu-id="88af6-118">.NET 5 Preview</span></span> |
|--------------------------|---------------|---------------|----------------|
| <span data-ttu-id="88af6-119">✔️ 3.12</span><span class="sxs-lookup"><span data-stu-id="88af6-119">✔️ 3.12</span></span>  | <span data-ttu-id="88af6-120">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="88af6-120">✔️ 2.1</span></span>        | <span data-ttu-id="88af6-121">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="88af6-121">✔️ 3.1</span></span>        | <span data-ttu-id="88af6-122">✔️ 5.0 Preview</span><span class="sxs-lookup"><span data-stu-id="88af6-122">✔️ 5.0 Preview</span></span> |
| <span data-ttu-id="88af6-123">✔️ 3.11</span><span class="sxs-lookup"><span data-stu-id="88af6-123">✔️ 3.11</span></span>  | <span data-ttu-id="88af6-124">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="88af6-124">✔️ 2.1</span></span>        | <span data-ttu-id="88af6-125">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="88af6-125">✔️ 3.1</span></span>        | <span data-ttu-id="88af6-126">✔️ 5.0 Preview</span><span class="sxs-lookup"><span data-stu-id="88af6-126">✔️ 5.0 Preview</span></span> |
| <span data-ttu-id="88af6-127">✔️ 3.10</span><span class="sxs-lookup"><span data-stu-id="88af6-127">✔️ 3.10</span></span>  | <span data-ttu-id="88af6-128">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="88af6-128">✔️ 2.1</span></span>        | <span data-ttu-id="88af6-129">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="88af6-129">✔️ 3.1</span></span>        | <span data-ttu-id="88af6-130">✔️ 5.0 Preview</span><span class="sxs-lookup"><span data-stu-id="88af6-130">✔️ 5.0 Preview</span></span> |
| <span data-ttu-id="88af6-131">✔️ 3.9</span><span class="sxs-lookup"><span data-stu-id="88af6-131">✔️ 3.9</span></span>   | <span data-ttu-id="88af6-132">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="88af6-132">✔️ 2.1</span></span>        | <span data-ttu-id="88af6-133">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="88af6-133">✔️ 3.1</span></span>        | <span data-ttu-id="88af6-134">✔️ 5.0 Preview</span><span class="sxs-lookup"><span data-stu-id="88af6-134">✔️ 5.0 Preview</span></span> |
| <span data-ttu-id="88af6-135">❌ 3.8</span><span class="sxs-lookup"><span data-stu-id="88af6-135">❌ 3.8</span></span>   | <span data-ttu-id="88af6-136">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="88af6-136">✔️ 2.1</span></span>        | <span data-ttu-id="88af6-137">❌ 3.1</span><span class="sxs-lookup"><span data-stu-id="88af6-137">❌ 3.1</span></span>        | <span data-ttu-id="88af6-138">❌ 5.0 Preview</span><span class="sxs-lookup"><span data-stu-id="88af6-138">❌ 5.0 Preview</span></span> |

<span data-ttu-id="88af6-139">Die folgenden Versionen von .NET Core werden nicht mehr unterstützt.</span><span class="sxs-lookup"><span data-stu-id="88af6-139">The following versions of .NET Core are no longer supported.</span></span> <span data-ttu-id="88af6-140">Die Downloads dafür bleiben weiterhin veröffentlicht:</span><span class="sxs-lookup"><span data-stu-id="88af6-140">The downloads for these still remain published:</span></span>

- <span data-ttu-id="88af6-141">3.0</span><span class="sxs-lookup"><span data-stu-id="88af6-141">3.0</span></span>
- <span data-ttu-id="88af6-142">2.2</span><span class="sxs-lookup"><span data-stu-id="88af6-142">2.2</span></span>
- <span data-ttu-id="88af6-143">2.0</span><span class="sxs-lookup"><span data-stu-id="88af6-143">2.0</span></span>

## <a name="dependencies"></a><span data-ttu-id="88af6-144">Abhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="88af6-144">Dependencies</span></span>

<span data-ttu-id="88af6-145">.NET Core unter Alpine Linux erfordert die Installation der folgenden Abhängigkeiten:</span><span class="sxs-lookup"><span data-stu-id="88af6-145">.NET Core on Alpine Linux requires the following dependencies installed:</span></span>

- <span data-ttu-id="88af6-146">icu-libs</span><span class="sxs-lookup"><span data-stu-id="88af6-146">icu-libs</span></span>
- <span data-ttu-id="88af6-147">krb5-libs</span><span class="sxs-lookup"><span data-stu-id="88af6-147">krb5-libs</span></span>
- <span data-ttu-id="88af6-148">libintl</span><span class="sxs-lookup"><span data-stu-id="88af6-148">libintl</span></span>
- <span data-ttu-id="88af6-149">libssl1.1 (Alpine 3.9 oder höher)</span><span class="sxs-lookup"><span data-stu-id="88af6-149">libssl1.1 (Alpine v3.9 or greater)</span></span>
- <span data-ttu-id="88af6-150">libssl1.0 (Alpine 3.8)</span><span class="sxs-lookup"><span data-stu-id="88af6-150">libssl1.0 (Alpine v3.8)</span></span>
- <span data-ttu-id="88af6-151">libstdc++</span><span class="sxs-lookup"><span data-stu-id="88af6-151">libstdc++</span></span>
- <span data-ttu-id="88af6-152">lttng-ust</span><span class="sxs-lookup"><span data-stu-id="88af6-152">lttng-ust</span></span>
- <span data-ttu-id="88af6-153">numactl (optional)</span><span class="sxs-lookup"><span data-stu-id="88af6-153">numactl (optional)</span></span>
- <span data-ttu-id="88af6-154">zlib</span><span class="sxs-lookup"><span data-stu-id="88af6-154">zlib</span></span>

## <a name="scripted-install"></a><span data-ttu-id="88af6-155">Per Skript gesteuerte Installation</span><span class="sxs-lookup"><span data-stu-id="88af6-155">Scripted install</span></span>

[!INCLUDE [linux-install-scripted](includes/linux-install-scripted.md)]

## <a name="manual-install"></a><span data-ttu-id="88af6-156">Manuelle Installation</span><span class="sxs-lookup"><span data-stu-id="88af6-156">Manual install</span></span>

[!INCLUDE [linux-install-manual](includes/linux-install-manual.md)]

## <a name="next-steps"></a><span data-ttu-id="88af6-157">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="88af6-157">Next steps</span></span>

- [<span data-ttu-id="88af6-158">Tutorial: Erstellen einer Konsolenanwendung mit dem .NET Core SDK in Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="88af6-158">Tutorial: Create a console application with .NET Core SDK using Visual Studio Code</span></span>](../tutorials/with-visual-studio-code.md)