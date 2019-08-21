---
title: Umstände, unter denen eine Bereitstellung von Windows-Containern auf virtuellen Azure-Computern (IaaS-Cloud) erfolgen sollte
description: Modernisieren vorhandener .NET-Anwendungen mit Azure Cloud und Windows-Containern | Zeitpunkt der Bereitstellung von Windows-Containern in Azure-VMS (IaaS-Cloud)
ms.date: 04/28/2018
ms.openlocfilehash: e9a2903662306b607977a7751018e24161ab80ab
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "69577903"
---
# <a name="when-to-deploy-windows-containers-to-azure-vms-iaas-cloud"></a><span data-ttu-id="30366-103">Umstände, unter denen eine Bereitstellung von Windows-Containern auf virtuellen Azure-Computern (IaaS-Cloud) erfolgen sollte</span><span class="sxs-lookup"><span data-stu-id="30366-103">When to deploy Windows Containers to Azure VMs (IaaS cloud)</span></span>

<span data-ttu-id="30366-104">Wenn Ihre Organisation Azure-VMS verwendet, auch wenn Sie auch Windows-Container verwenden, sind Sie immer noch mit IAAS beschäftigt.</span><span class="sxs-lookup"><span data-stu-id="30366-104">If your organization is using Azure VMs, even if you are also using Windows Containers, you are still dealing with IaaS.</span></span> <span data-ttu-id="30366-105">Dies bedeutet, dass Sie sich mit Infrastruktur Vorgängen, VM-Betriebs Systempatches und Infrastruktur Komplexität für hochgradig skalierbare Anwendungen beschäftigen müssen, wenn Sie die Bereitstellung auf mehreren VMs in einer Infrastruktur mit Lastenausgleich durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="30366-105">That means that dealing with infrastructure operations, VM OS patches, and infrastructure complexity for highly scalable applications when you need to deploy to multiple VMs in a load balanced infrastructure.</span></span> <span data-ttu-id="30366-106">Die wichtigsten Szenarien für die Verwendung von Windows-Containern auf einem virtuellen Azure-Computer sind:</span><span class="sxs-lookup"><span data-stu-id="30366-106">The main scenarios for using Windows Containers in an Azure VM are:</span></span>

- <span data-ttu-id="30366-107">**Dev/Test-Umgebung**: Ein virtueller Computer in der Cloud eignet sich ideal für Entwicklung und Tests in der Cloud.</span><span class="sxs-lookup"><span data-stu-id="30366-107">**Dev/test environment**: A VM in the cloud is perfect for development and testing in the cloud.</span></span> <span data-ttu-id="30366-108">Die Umgebung kann je nach Ihren Anforderungen schnell erstellt oder angehalten werden.</span><span class="sxs-lookup"><span data-stu-id="30366-108">You can rapidly create or stop the environment depending on your needs.</span></span>

- <span data-ttu-id="30366-109">**Kleine und mittlere Skalierbarkeits Anforderungen**: In Szenarien, in denen Sie möglicherweise nur eine Reihe von virtuellen Computern für Ihre Produktionsumgebung benötigen, kann das Verwalten einer kleinen Anzahl von virtuellen Computern so lange kostengünstig sein, bis Sie zu erweiterten Umgebungen wie orchestratoren wechseln können.</span><span class="sxs-lookup"><span data-stu-id="30366-109">**Small and medium scalability needs**: In scenarios where you might need just a couple of VMs for your production environment, managing a small number of VMs might be affordable until you can move to more advanced PaaS environments, like orchestrators.</span></span>

- <span data-ttu-id="30366-110">**Produktionsumgebung mit vorhandenen Bereitstellungs Tools**: Möglicherweise wechseln Sie von einer lokalen Umgebung, in der Sie in Tools investiert haben, um komplexe bereit Stellungen für VMS oder Bare-Metal-Server zu erstellen (z.b. Puppet oder ähnliche Tools).</span><span class="sxs-lookup"><span data-stu-id="30366-110">**Production environment with existing deployment tools**: You might be moving from an on-premises environment in which you have invested in tools to make complex deployments to VMs or bare-metal servers (like Puppet or similar tools).</span></span> <span data-ttu-id="30366-111">Um mit minimalen Änderungen an den Bereitstellungs Verfahren für die Produktionsumgebung in die Cloud zu wechseln, können Sie diese Tools für die Bereitstellung auf Azure-VMS verwenden.</span><span class="sxs-lookup"><span data-stu-id="30366-111">To move to the cloud with minimal changes to production environment deployment procedures, you might continue to use those tools to deploy to Azure VMs.</span></span> <span data-ttu-id="30366-112">Allerdings möchten Sie Windows-Container als Bereitstellungs Einheit verwenden, um die Bereitstellung zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="30366-112">However, you'll want to use Windows Containers as the unit of deployment to improve the deployment experience.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="30366-113">[Zurück](when-to-deploy-windows-containers-in-your-on-premises-iaas-vm-infrastructure.md)
>[Weiter](when-to-deploy-windows-containers-to-azure-container-instances-ACI.md)</span><span class="sxs-lookup"><span data-stu-id="30366-113">[Previous](when-to-deploy-windows-containers-in-your-on-premises-iaas-vm-infrastructure.md)
[Next](when-to-deploy-windows-containers-to-azure-container-instances-ACI.md)</span></span>