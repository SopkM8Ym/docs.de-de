---
title: Installieren von .NET Core unter Debian (.NET Core)
description: Veranschaulicht verschiedene Möglichkeiten, das .NET Core SDK und die NET Core-Runtime unter Debian zu installieren.
author: thraka
ms.author: adegeo
ms.date: 06/04/2020
ms.openlocfilehash: c66d8e1daad4e59a766781b7117600352879b724
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602807"
---
# <a name="install-net-core-sdk-or-net-core-runtime-on-debian"></a><span data-ttu-id="72bce-103">Installieren des .NET Core SDK oder der .NET Core-Runtime unter Debian</span><span class="sxs-lookup"><span data-stu-id="72bce-103">Install .NET Core SDK or .NET Core Runtime on Debian</span></span>

<span data-ttu-id="72bce-104">In diesem Artikel wird beschrieben, wie Sie .NET Core unter Debian installieren.</span><span class="sxs-lookup"><span data-stu-id="72bce-104">This article describes how to install .NET Core on Debian.</span></span> <span data-ttu-id="72bce-105">Wenn für eine Debian-Version kein Support mehr geboten wird, wird .NET Core mit dieser Version nicht mehr unterstützt.</span><span class="sxs-lookup"><span data-stu-id="72bce-105">When a Debian version falls out of support, .NET Core is no longer supported with that version.</span></span> <span data-ttu-id="72bce-106">Diese Anweisungen können Ihnen jedoch helfen, .NET Core in diesen Versionen auszuführen, auch wenn kein Support dafür geboten wird.</span><span class="sxs-lookup"><span data-stu-id="72bce-106">However, these instructions may help you to get .NET Core running on those versions, even though it isn't supported.</span></span>

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

[!INCLUDE [linux-install-package-manager-x64-vs-arm](includes/linux-install-package-manager-x64-vs-arm.md)]

## <a name="supported-distributions"></a><span data-ttu-id="72bce-107">Unterstützte Distributionen</span><span class="sxs-lookup"><span data-stu-id="72bce-107">Supported distributions</span></span>

<span data-ttu-id="72bce-108">Die folgende Tabelle ist eine Liste der derzeit unterstützten .NET Core-Versionen und der Versionen von Debian, unter denen sie unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="72bce-108">The following table is a list of currently supported .NET Core releases and the versions of Debian they're supported on.</span></span> <span data-ttu-id="72bce-109">Diese Versionen werden weiterhin unterstützt, bis entweder die Version von [.NET Core das Ende des Supports](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) oder die Version von [Debian das Ende ihrer Lebensdauer erreicht](https://wiki.debian.org/DebianReleases).</span><span class="sxs-lookup"><span data-stu-id="72bce-109">These versions remain supported until either the version of [.NET Core reaches end-of-support](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) or the version of [Debian reaches end-of-life](https://wiki.debian.org/DebianReleases).</span></span>

- <span data-ttu-id="72bce-110">✔️ gibt an, dass die Version von Debian oder .NET Core weiterhin unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="72bce-110">A ✔️ indicates that the version of Debian or .NET Core is still supported.</span></span>
- <span data-ttu-id="72bce-111">❌ gibt an, dass die Version von Debian oder .NET Core in dieser Debian-Version nicht unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="72bce-111">A ❌ indicates that the version of Debian or .NET Core isn't supported on that Debian release.</span></span>
- <span data-ttu-id="72bce-112">Wenn sowohl eine Version von Debian als auch eine Version von .NET Core über ✔️ verfügen, wird diese Kombination aus Betriebssystem und .NET unterstützt.</span><span class="sxs-lookup"><span data-stu-id="72bce-112">When both a version of Debian and a version of .NET Core have ✔️, that OS and .NET combination are supported.</span></span>

| <span data-ttu-id="72bce-113">Debian</span><span class="sxs-lookup"><span data-stu-id="72bce-113">Debian</span></span>                   | <span data-ttu-id="72bce-114">.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="72bce-114">.NET Core 2.1</span></span> | <span data-ttu-id="72bce-115">.NET Core 3.1</span><span class="sxs-lookup"><span data-stu-id="72bce-115">.NET Core 3.1</span></span> | <span data-ttu-id="72bce-116">.NET 5 Preview (nur manuelle Installation)</span><span class="sxs-lookup"><span data-stu-id="72bce-116">.NET 5 Preview (manual install only)</span></span> |
|--------------------------|---------------|---------------|----------------|
| <span data-ttu-id="72bce-117">✔️ [10](#debian-10-)</span><span class="sxs-lookup"><span data-stu-id="72bce-117">✔️ [10](#debian-10-)</span></span>     | <span data-ttu-id="72bce-118">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="72bce-118">✔️ 2.1</span></span>        | <span data-ttu-id="72bce-119">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="72bce-119">✔️ 3.1</span></span>        | <span data-ttu-id="72bce-120">✔️ 5.0 Preview</span><span class="sxs-lookup"><span data-stu-id="72bce-120">✔️ 5.0 Preview</span></span> |
| <span data-ttu-id="72bce-121">✔️ [9](#debian-9-)</span><span class="sxs-lookup"><span data-stu-id="72bce-121">✔️ [9](#debian-9-)</span></span>       | <span data-ttu-id="72bce-122">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="72bce-122">✔️ 2.1</span></span>        | <span data-ttu-id="72bce-123">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="72bce-123">✔️ 3.1</span></span>        | <span data-ttu-id="72bce-124">✔️ 5.0 Preview</span><span class="sxs-lookup"><span data-stu-id="72bce-124">✔️ 5.0 Preview</span></span> |
| <span data-ttu-id="72bce-125">❌ [8](#debian-8-)</span><span class="sxs-lookup"><span data-stu-id="72bce-125">❌ [8](#debian-8-)</span></span>       | <span data-ttu-id="72bce-126">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="72bce-126">✔️ 2.1</span></span>        | <span data-ttu-id="72bce-127">❌ 3.1</span><span class="sxs-lookup"><span data-stu-id="72bce-127">❌ 3.1</span></span>        | <span data-ttu-id="72bce-128">❌ 5.0 Preview</span><span class="sxs-lookup"><span data-stu-id="72bce-128">❌ 5.0 Preview</span></span> |

<span data-ttu-id="72bce-129">Die folgenden Versionen von .NET Core werden nicht mehr unterstützt.</span><span class="sxs-lookup"><span data-stu-id="72bce-129">The following versions of .NET Core are no longer supported.</span></span> <span data-ttu-id="72bce-130">Die Downloads dafür bleiben weiterhin veröffentlicht:</span><span class="sxs-lookup"><span data-stu-id="72bce-130">The downloads for these still remain published:</span></span>

- <span data-ttu-id="72bce-131">3.0</span><span class="sxs-lookup"><span data-stu-id="72bce-131">3.0</span></span>
- <span data-ttu-id="72bce-132">2.2</span><span class="sxs-lookup"><span data-stu-id="72bce-132">2.2</span></span>
- <span data-ttu-id="72bce-133">2.0</span><span class="sxs-lookup"><span data-stu-id="72bce-133">2.0</span></span>

## <a name="how-to-install-other-versions"></a><span data-ttu-id="72bce-134">Installieren anderer Versionen</span><span class="sxs-lookup"><span data-stu-id="72bce-134">How to install other versions</span></span>

[!INCLUDE [hack-pkgname](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="debian-10-"></a><span data-ttu-id="72bce-135">Debian 10 ✔️</span><span class="sxs-lookup"><span data-stu-id="72bce-135">Debian 10 ✔️</span></span>

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/debian/10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-31](includes/linux-install-31-apt.md)]

## <a name="debian-9-"></a><span data-ttu-id="72bce-136">Debian 9 ✔️</span><span class="sxs-lookup"><span data-stu-id="72bce-136">Debian 9 ✔️</span></span>

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget -O - https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.asc.gpg
sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
wget https://packages.microsoft.com/config/debian/9/prod.list
sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
```

[!INCLUDE [linux-apt-install-31](includes/linux-install-31-apt.md)]

## <a name="debian-8-"></a><span data-ttu-id="72bce-137">Debian 8 ❌</span><span class="sxs-lookup"><span data-stu-id="72bce-137">Debian 8 ❌</span></span>

[!INCLUDE [linux-not-supported](includes/linux-not-supported-debian.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget -O - https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.asc.gpg
sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
wget https://packages.microsoft.com/config/debian/8/prod.list
sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
```

[!INCLUDE [linux-apt-install-21](includes/linux-install-21-apt.md)]

## <a name="apt-update-sdk-or-runtime"></a><span data-ttu-id="72bce-138">APT Update SDK oder Runtime</span><span class="sxs-lookup"><span data-stu-id="72bce-138">APT update SDK or runtime</span></span>

<span data-ttu-id="72bce-139">Wenn eine neue Patchversion für .NET Core verfügbar ist, können Sie diese einfach über APT mit den folgenden Befehlen aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="72bce-139">When a new patch release is available for .NET Core, you can simply upgrade it through APT with the following commands:</span></span>

```bash
sudo apt-get update
sudo apt-get upgrade
```

## <a name="apt-troubleshooting"></a><span data-ttu-id="72bce-140">Problembehandlung für APT</span><span class="sxs-lookup"><span data-stu-id="72bce-140">APT troubleshooting</span></span>

<span data-ttu-id="72bce-141">Dieser Abschnitt enthält Informationen zu häufigen Fehlern, die bei der Installation von .NET Core mit APT auftreten können.</span><span class="sxs-lookup"><span data-stu-id="72bce-141">This section provides information on common errors you may get while using APT to install .NET Core.</span></span>

### <a name="unable-to-locate"></a><span data-ttu-id="72bce-142">Auffinden nicht möglich</span><span class="sxs-lookup"><span data-stu-id="72bce-142">Unable to locate</span></span>

[!INCLUDE [package-manager-failed-to-find-deb](includes/package-manager-failed-to-find-deb.md)]

```bash
sudo apt-get install -y gpg
wget -O - https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor -o microsoft.asc.gpg
sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
wget https://packages.microsoft.com/config/debian/{os-version}/prod.list
sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
sudo apt-get update; \
  sudo apt-get install -y apt-transport-https && \
  sudo apt-get update && \
  sudo apt-get install -y {dotnet-package}
```

### <a name="failed-to-fetch"></a><span data-ttu-id="72bce-143">Fehler beim Abrufen</span><span class="sxs-lookup"><span data-stu-id="72bce-143">Failed to fetch</span></span>

[!INCLUDE [package-manager-failed-to-fetch-deb](includes/package-manager-failed-to-fetch-deb.md)]

## <a name="snap"></a><span data-ttu-id="72bce-144">Snap</span><span class="sxs-lookup"><span data-stu-id="72bce-144">Snap</span></span>

[!INCLUDE [linux-install-snap](includes/linux-install-snap.md)]

## <a name="dependencies"></a><span data-ttu-id="72bce-145">Abhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="72bce-145">Dependencies</span></span>

[!INCLUDE [linux-install-dependencies](includes/linux-install-dependencies.md)]

## <a name="scripted-install"></a><span data-ttu-id="72bce-146">Per Skript gesteuerte Installation</span><span class="sxs-lookup"><span data-stu-id="72bce-146">Scripted install</span></span>

[!INCLUDE [linux-install-scripted](includes/linux-install-scripted.md)]

## <a name="manual-install"></a><span data-ttu-id="72bce-147">Manuelle Installation</span><span class="sxs-lookup"><span data-stu-id="72bce-147">Manual install</span></span>

[!INCLUDE [linux-install-manual](includes/linux-install-manual.md)]

## <a name="next-steps"></a><span data-ttu-id="72bce-148">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="72bce-148">Next steps</span></span>

- [<span data-ttu-id="72bce-149">Tutorial: Erstellen einer Konsolenanwendung mit dem .NET Core SDK in Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="72bce-149">Tutorial: Create a console application with .NET Core SDK using Visual Studio Code</span></span>](../tutorials/with-visual-studio-code.md)