---
title: Einführung in die eshoponcontainers-Referenz-App
description: Einführung in die systemeigene cloudbasierte Webdienst-Referenz-App für ASP.net Core und Azure.
ms.date: 06/30/2019
ms.openlocfilehash: 0d55f248acbc34bcc76d38987d7e1d537cf6065a
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73841774"
---
# <a name="introducing-eshoponcontainers-reference-app"></a><span data-ttu-id="2d0fa-103">Einführung in die eshoponcontainers-Referenz-App</span><span class="sxs-lookup"><span data-stu-id="2d0fa-103">Introducing eShopOnContainers reference app</span></span>

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

<span data-ttu-id="2d0fa-104">In Zusammenarbeit mit führenden Community-Experten hat Microsoft in Zusammenarbeit mit einer voll funktionsfähigen Cloud-Native-Referenz Anwendung (eshoponcontainers) erstellt.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-104">Microsoft, in partnership with leading community experts, has produced a full-featured cloud-native microservices reference application, eShopOnContainers.</span></span> <span data-ttu-id="2d0fa-105">Diese Anwendung ist für die Veranschaulichung der Verwendung von .net Core und docker und optional von Azure, Kubernetes und Visual Studio zum Erstellen einer Online Store Front konzipiert.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-105">This application is built to showcase using .NET Core and Docker, and optionally Azure, Kubernetes, and Visual Studio, to build an online storefront.</span></span>

![Bildschirm Abbildung der eshoponcontainers-Beispiel-app.](./media/eshoponcontainers-sample-app-screenshot.png)

<span data-ttu-id="2d0fa-107">**Abbildung 2-1**.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-107">**Figure 2-1**.</span></span> <span data-ttu-id="2d0fa-108">Bildschirm Abbildung der eshoponcontainers-Beispiel-app.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-108">eShopOnContainers Sample App Screenshot.</span></span>

<span data-ttu-id="2d0fa-109">Bevor Sie mit diesem Kapitel beginnen, sollten Sie die [eshoponcontainers-Referenz Anwendung](https://github.com/dotnet-architecture/eShopOnContainers)herunterladen.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-109">Before starting this chapter, we recommend that you download the [eShopOnContainers reference application](https://github.com/dotnet-architecture/eShopOnContainers).</span></span> <span data-ttu-id="2d0fa-110">Wenn Sie dies tun, sollten Sie die nachfolgenden Informationen leichter befolgen.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-110">If you do so, it should be easier for you to follow along with the information presented.</span></span>

## <a name="features-and-requirements"></a><span data-ttu-id="2d0fa-111">Features und Anforderungen</span><span class="sxs-lookup"><span data-stu-id="2d0fa-111">Features and requirements</span></span>

<span data-ttu-id="2d0fa-112">Beginnen wir mit einer Prüfung der Features und Anforderungen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-112">Let's start with a review of the application's features and requirements.</span></span> <span data-ttu-id="2d0fa-113">Die eshoponcontainers-Anwendung stellt einen Online Shop dar, der verschiedene physische Produkte wie t-Shirts und Kaffee-Mugs verkauft.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-113">The eShopOnContainers application represents an online store that sells various physical products like t-shirts and coffee mugs.</span></span> <span data-ttu-id="2d0fa-114">Wenn Sie zuvor etwas online gekauft haben, sollte die Erfahrung bei der Verwendung des Stores relativ vertraut sein.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-114">If you've bought anything online before, the experience of using the store should be relatively familiar.</span></span> <span data-ttu-id="2d0fa-115">Im folgenden finden Sie einige der grundlegenden Features, die der Store implementiert:</span><span class="sxs-lookup"><span data-stu-id="2d0fa-115">Here are some of the basic features the store implements:</span></span>

- <span data-ttu-id="2d0fa-116">Katalog Elemente auflisten</span><span class="sxs-lookup"><span data-stu-id="2d0fa-116">List catalog items</span></span>
- <span data-ttu-id="2d0fa-117">Elemente nach Typ filtern</span><span class="sxs-lookup"><span data-stu-id="2d0fa-117">Filter items by type</span></span>
- <span data-ttu-id="2d0fa-118">Elemente nach Marke Filtern</span><span class="sxs-lookup"><span data-stu-id="2d0fa-118">Filter items by brand</span></span>
- <span data-ttu-id="2d0fa-119">Elemente zum Warenkorb hinzufügen</span><span class="sxs-lookup"><span data-stu-id="2d0fa-119">Add items to the shopping basket</span></span>
- <span data-ttu-id="2d0fa-120">Bearbeiten oder Entfernen von Elementen aus dem Warenkorb</span><span class="sxs-lookup"><span data-stu-id="2d0fa-120">Edit or remove items from the basket</span></span>
- <span data-ttu-id="2d0fa-121">Zur Kasse</span><span class="sxs-lookup"><span data-stu-id="2d0fa-121">Checkout</span></span>
- <span data-ttu-id="2d0fa-122">Konto registrieren</span><span class="sxs-lookup"><span data-stu-id="2d0fa-122">Register an account</span></span>
- <span data-ttu-id="2d0fa-123">Anmelden</span><span class="sxs-lookup"><span data-stu-id="2d0fa-123">Sign in</span></span>
- <span data-ttu-id="2d0fa-124">Abmelden</span><span class="sxs-lookup"><span data-stu-id="2d0fa-124">Sign out</span></span>
- <span data-ttu-id="2d0fa-125">Aufträge überprüfen</span><span class="sxs-lookup"><span data-stu-id="2d0fa-125">Review orders</span></span>

<span data-ttu-id="2d0fa-126">Die Anwendung verfügt auch über die folgenden nicht funktionalen Anforderungen:</span><span class="sxs-lookup"><span data-stu-id="2d0fa-126">The application also has the following non-functional requirements:</span></span>

- <span data-ttu-id="2d0fa-127">Der Dienst muss hoch verfügbar sein, und er muss automatisch skaliert werden, um mehr Datenverkehr zu erreichen (und die Skalierung nach dem Datenverkehr zu erhöhen).</span><span class="sxs-lookup"><span data-stu-id="2d0fa-127">It needs to be highly available and it must scale automatically to meet increased traffic (and scale back down once traffic subsides).</span></span>
- <span data-ttu-id="2d0fa-128">Es sollte eine einfach zu verwendende Überwachung der Integritäts-und Diagnoseprotokolle bieten, um Probleme bei der Problembehandlung zu beheben.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-128">It should provide easy-to-use monitoring of its health and diagnostic logs to help troubleshoot any issues it encounters.</span></span>
- <span data-ttu-id="2d0fa-129">Es sollte einen Agile Entwicklungsprozess unterstützen, einschließlich der Unterstützung für Continuous Integration und Bereitstellung (CI/CD).</span><span class="sxs-lookup"><span data-stu-id="2d0fa-129">It should support an agile development process, including support for continuous integration and deployment (CI/CD).</span></span>
- <span data-ttu-id="2d0fa-130">Zusätzlich zu den beiden Web-Front-Ends (herkömmliche und Single-Page-Anwendung) muss die Anwendung auch Mobile Client-apps unterstützen, die unterschiedliche Arten von Betriebssystemen ausführen.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-130">In addition to the two web front ends (traditional and Single Page Application), the application must also support mobile client apps running different kinds of operating systems.</span></span>
- <span data-ttu-id="2d0fa-131">Es sollte plattformübergreifendes Hosting und plattformübergreifende Entwicklung unterstützen.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-131">It should support cross-platform hosting and cross-platform development.</span></span>

![eshoponcontainers Referenz zur Anwendungs Entwicklungs Architektur.](./media/eshoponcontainers-development-architecture.png)

<span data-ttu-id="2d0fa-133">**Abbildung 2-2:**</span><span class="sxs-lookup"><span data-stu-id="2d0fa-133">**Figure 2-2**.</span></span> <span data-ttu-id="2d0fa-134">eshoponcontainers Referenz zur Anwendungs Entwicklungs Architektur.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-134">eShopOnContainers reference application development architecture.</span></span>

<span data-ttu-id="2d0fa-135">Der Zugriff auf die Anwendung eshoponcontainers ist über Web-oder Mobile Clients möglich, die über HTTPS auf die Anwendung zugreifen, die entweder auf die ASP.net Core MVC-Serveranwendung oder ein entsprechendes API-Gateway abzielt.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-135">The eShopOnContainers application is accessible from web or mobile clients that access the application over HTTPS targeting either the ASP.NET Core MVC server application or an appropriate API Gateway.</span></span> <span data-ttu-id="2d0fa-136">API-Gateways bieten mehrere Vorteile, wie z. b. das Entkoppeln von Back-End-Diensten von einzelnen Front-End-Clients und die Bereitstellung einer</span><span class="sxs-lookup"><span data-stu-id="2d0fa-136">API Gateways offer several advantages, such as decoupling back-end services from individual front-end clients and providing better security.</span></span> <span data-ttu-id="2d0fa-137">Die Anwendung nutzt auch ein verknüpftes Muster, das als Backends-for-Frontend (BFF) bezeichnet wird, das die Erstellung separater API-Gateways für jeden Front-End-Client empfiehlt.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-137">The application also makes use of a related pattern known as Backends-for-Frontends (BFF), which recommends creating separate API gateways for each front-end client.</span></span> <span data-ttu-id="2d0fa-138">Die Referenzarchitektur veranschaulicht das Aufteilen der API-Gateways, je nachdem, ob die Anforderung von einem Web-oder mobilen Client stammt.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-138">The reference architecture demonstrates breaking up the API gateways based on whether the request is coming from a web or mobile client.</span></span>

<span data-ttu-id="2d0fa-139">Die Funktionalität der Anwendung wird in eine Reihe unterschiedlicher-Dienste aufgeteilt.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-139">The application's functionality is broken up into a number of distinct microservices.</span></span> <span data-ttu-id="2d0fa-140">Dienste sind für die Authentifizierung und Identität, das Auflisten von Elementen aus dem Produktkatalog, das Verwalten der Einkaufskörbe von Benutzern und das Platzieren von Aufträgen zuständig.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-140">There are services responsible for authentication and identity, listing items from the product catalog, managing users' shopping baskets, and  placing orders.</span></span> <span data-ttu-id="2d0fa-141">Jeder dieser separaten Dienste verfügt über einen eigenen persistenten Speicher.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-141">Each of these separate services has its own persistent storage.</span></span> <span data-ttu-id="2d0fa-142">Beachten Sie, dass es keinen einzelnen Master Datenspeicher gibt, mit dem alle Dienste interagieren.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-142">Note that there's no single master data store with which all services interact.</span></span> <span data-ttu-id="2d0fa-143">Stattdessen erfolgt die Koordination und Kommunikation zwischen den Diensten Bedarfs abhängig und durch die Verwendung eines Nachrichtenbus.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-143">Instead, coordination and communication between the services is done on an as-needed basis and through the use of a message bus.</span></span>

<span data-ttu-id="2d0fa-144">Alle unterschiedlichen-Dienste sind basierend auf Ihren individuellen Anforderungen anders konzipiert.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-144">Each of the different microservices is designed differently, based on their individual requirements.</span></span> <span data-ttu-id="2d0fa-145">Dies bedeutet, dass sich der technologische Stapel unterscheiden kann, obwohl alle mit .net Core erstellt und für die Cloud entwickelt wurden.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-145">This means their technology stack may differ, although they're all built using .NET Core and designed for the cloud.</span></span> <span data-ttu-id="2d0fa-146">Einfachere Dienste bieten einen einfachen CRUD-Zugriff (Create-Read-Update-Delete) auf die zugrunde liegenden Datenspeicher, während erweiterte Dienste Domänen gesteuerte Entwurfs Ansätze und-Muster verwenden, um die Geschäfts Komplexität zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-146">Simpler services provide basic Create-Read-Update-Delete (CRUD) access to the underlying data stores, while more advanced services use Domain-Driven Design approaches and patterns to manage business complexity.</span></span>

![Verschiedene Arten von mikrodiensten](./media/different-kinds-of-microservices.png)

<span data-ttu-id="2d0fa-148">**Abbildung 2-3:**</span><span class="sxs-lookup"><span data-stu-id="2d0fa-148">**Figure 2-3**.</span></span> <span data-ttu-id="2d0fa-149">Verschiedene Arten von-Diensten.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-149">Different kinds of microservices.</span></span>

## <a name="overview-of-the-code"></a><span data-ttu-id="2d0fa-150">Übersicht über den Code</span><span class="sxs-lookup"><span data-stu-id="2d0fa-150">Overview of the code</span></span>

<span data-ttu-id="2d0fa-151">Da es sich um die Nutzung von mikrodiensten handelt, enthält die eshoponcontainers-App im GitHub-Repository einige separate Projekte und Lösungen.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-151">Because it leverages microservices, the eShopOnContainers app includes quite a few separate projects and solutions in its GitHub repository.</span></span> <span data-ttu-id="2d0fa-152">Zusätzlich zu separaten Lösungen und ausführbaren Dateien sind die verschiedenen Dienste so konzipiert, dass Sie sowohl während der lokalen Entwicklung als auch zur Laufzeit in der Produktion in ihren eigenen Containern ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-152">In addition to separate solutions and executable files, the various services are designed to run inside their own containers, both during local development and at runtime in production.</span></span> <span data-ttu-id="2d0fa-153">In Abbildung 2-4 wird die vollständige Visual Studio-Projekt Mappe gezeigt, in der die verschiedenen Projekte organisiert sind.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-153">Figure 2-4 shows the full Visual Studio solution, in which the various different projects are organized.</span></span>

![Projekte in der Visual Studio-Projekt Mappe.](./media/projects-in-visual-studio-solution.png)

<span data-ttu-id="2d0fa-155">**Abbildung 2-4**.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-155">**Figure 2-4**.</span></span> <span data-ttu-id="2d0fa-156">Projekte in der Visual Studio-Projekt Mappe.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-156">Projects in Visual Studio solution.</span></span>

<span data-ttu-id="2d0fa-157">Der Code ist für die Unterstützung der verschiedenen-mikrodienste organisiert, und innerhalb der einzelnen mikrodienste wird der Code in Domänen Logik, Infrastrukturprobleme sowie eine Benutzeroberfläche oder ein Dienst Endpunkt aufgeteilt.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-157">The code is organized to support the different microservices, and within each microservice, the code is broken up into domain logic, infrastructure concerns, and user interface or service endpoint.</span></span> <span data-ttu-id="2d0fa-158">In vielen Fällen können die Abhängigkeiten jedes Diensts durch Azure-Dienste in der Produktionsumgebung und alternative Optionen für die lokale Entwicklung erfüllt werden.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-158">In many cases, each service's dependencies can be fulfilled by Azure services in production, as well as alternative options for local development.</span></span> <span data-ttu-id="2d0fa-159">Sehen wir uns an, wie die Anforderungen der Anwendung den Azure-Diensten zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-159">Let's examine how the application's requirements map to Azure services.</span></span>

## <a name="understanding-microservices"></a><span data-ttu-id="2d0fa-160">Grundlegendes zu mikroservices</span><span class="sxs-lookup"><span data-stu-id="2d0fa-160">Understanding microservices</span></span>

<span data-ttu-id="2d0fa-161">Dieses Buch konzentriert sich auf cloudbasierte Anwendungen, die mithilfe der Azure-Technologie erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-161">This book focuses on cloud-native applications built using Azure technology.</span></span> <span data-ttu-id="2d0fa-162">Weitere Informationen zu bewährten Methoden für microservices und zum Entwerfen von auf microservices basierenden Anwendungen finden Sie im Begleitbuch [.net-microservices: Architektur für .NET-Container Anwendungen](https://dotnet.microsoft.com/learn/aspnet/microservices-architecture).</span><span class="sxs-lookup"><span data-stu-id="2d0fa-162">To learn more about microservices best practices and how to architect microservice-based applications, read the companion book, [.NET Microservices: Architecture for Containerized .NET Applications](https://dotnet.microsoft.com/learn/aspnet/microservices-architecture).</span></span> <span data-ttu-id="2d0fa-163">Das Buch ist online, im PDF-oder im eReader-Format verfügbar.</span><span class="sxs-lookup"><span data-stu-id="2d0fa-163">The book is available online, in PDF, or eReader formats.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="2d0fa-164">[Zurück](candidate-apps.md)
>[Weiter](map-eshoponcontainers-azure-services.md)</span><span class="sxs-lookup"><span data-stu-id="2d0fa-164">[Previous](candidate-apps.md)
[Next](map-eshoponcontainers-azure-services.md)</span></span>