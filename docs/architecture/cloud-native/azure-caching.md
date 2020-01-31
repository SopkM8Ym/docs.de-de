---
title: Zwischenspeichern in einer Cloud-native Anwendung
description: Erfahren Sie mehr über das Zwischenspeichern von Strategien in einer Cloud-native Anwendung.
author: robvet
ms.date: 01/22/2020
ms.openlocfilehash: 2da61a01fc53233d1934df813fcba3b91a495c43
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2020
ms.locfileid: "76795116"
---
# <a name="caching-in-a-cloud-native-app"></a><span data-ttu-id="be066-103">Caching in einer cloudbasierten App</span><span class="sxs-lookup"><span data-stu-id="be066-103">Caching in a cloud-native app</span></span>

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

<span data-ttu-id="be066-104">Die Vorteile der Zwischenspeicherung sind gut verständlich.</span><span class="sxs-lookup"><span data-stu-id="be066-104">The benefits of caching are well understood.</span></span> <span data-ttu-id="be066-105">Das Verfahren funktioniert durch temporäres kopieren häufig aufgerufene Daten aus einem Back-End-Datenspeicher in einen *schnellen Speicher* , der sich näher an der Anwendung befindet.</span><span class="sxs-lookup"><span data-stu-id="be066-105">The technique works by temporarily copying frequently accessed data from a backend data store to *fast storage* that's located closer to the application.</span></span> <span data-ttu-id="be066-106">Caching wird häufig implementiert, wenn...</span><span class="sxs-lookup"><span data-stu-id="be066-106">Caching is often implemented where...</span></span>

- <span data-ttu-id="be066-107">Die Daten bleiben relativ statisch.</span><span class="sxs-lookup"><span data-stu-id="be066-107">Data remains relatively static.</span></span>
- <span data-ttu-id="be066-108">Der Datenzugriff ist langsam, insbesondere im Vergleich zur Geschwindigkeit des Caches.</span><span class="sxs-lookup"><span data-stu-id="be066-108">Data access is slow, especially compared to the speed of the cache.</span></span>
- <span data-ttu-id="be066-109">Daten unterliegen hohen Konflikten.</span><span class="sxs-lookup"><span data-stu-id="be066-109">Data is subject to high levels of contention.</span></span>

## <a name="why"></a><span data-ttu-id="be066-110">Warum?</span><span class="sxs-lookup"><span data-stu-id="be066-110">Why?</span></span>

<span data-ttu-id="be066-111">Wie im Leitfaden zum [Microsoft Caching](https://docs.microsoft.com/azure/architecture/best-practices/caching)erläutert, kann die Zwischenspeicherung die Leistung, Skalierbarkeit und Verfügbarkeit für einzelne und das System als Ganzes verbessern.</span><span class="sxs-lookup"><span data-stu-id="be066-111">As discussed in the [Microsoft caching guidance](https://docs.microsoft.com/azure/architecture/best-practices/caching), caching can increase performance, scalability, and availability for individual microservices and the system as a whole.</span></span> <span data-ttu-id="be066-112">Es verringert die Latenz und die Konflikte bei der Verarbeitung großer Mengen gleichzeitiger Anforderungen an einen Datenspeicher.</span><span class="sxs-lookup"><span data-stu-id="be066-112">It reduces the latency and contention of handling large volumes of concurrent requests to a data store.</span></span> <span data-ttu-id="be066-113">Wenn sich das Datenvolumen und die Anzahl der Benutzer erhöhen, sind die Vorteile der Zwischenspeicherung umso größer.</span><span class="sxs-lookup"><span data-stu-id="be066-113">As data volume and the number of users increase, the greater the benefits of caching become.</span></span>

<span data-ttu-id="be066-114">Das Zwischenspeichern ist am effektivsten, wenn ein Client wiederholt Daten liest, die unveränderlich sind oder sich nur selten ändern.</span><span class="sxs-lookup"><span data-stu-id="be066-114">Caching is most effective when a client repeatedly reads data that is immutable or that changes infrequently.</span></span> <span data-ttu-id="be066-115">Beispiele hierfür sind Referenzinformationen, z. b. Produkt-und Preisinformationen, oder freigegebene statische Ressourcen, die teuer zu erstellen sind.</span><span class="sxs-lookup"><span data-stu-id="be066-115">Examples include reference information such as product and pricing information, or shared static resources that are costly to construct.</span></span>

<span data-ttu-id="be066-116">Während die Zustands losen Dienste zustandslos sein sollten, kann ein verteilter Cache den gleichzeitigen Zugriff auf Sitzungszustandsdaten bei Bedarf unterstützen.</span><span class="sxs-lookup"><span data-stu-id="be066-116">While microservices should be stateless, a distributed cache can support concurrent access to session state data when absolutely required.</span></span>

<span data-ttu-id="be066-117">Beachten Sie auch Caching, um wiederkehrende Berechnungen zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="be066-117">Also consider caching to avoid repetitive computations.</span></span> <span data-ttu-id="be066-118">Wenn ein Vorgang Daten transformiert oder eine komplizierte Berechnung durchführt, wird das Ergebnis für nachfolgende Anforderungen zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="be066-118">If an operation transforms data or performs a complicated calculation, cache the result for subsequent requests.</span></span>

## <a name="caching-architecture"></a><span data-ttu-id="be066-119">Zwischen Speicherungs Architektur</span><span class="sxs-lookup"><span data-stu-id="be066-119">Caching architecture</span></span>

<span data-ttu-id="be066-120">Native Cloud-Anwendungen implementieren in der Regel eine verteilte zwischen Speicherungs Architektur.</span><span class="sxs-lookup"><span data-stu-id="be066-120">Cloud native applications typically implement a distributed caching architecture.</span></span> <span data-ttu-id="be066-121">Der Cache wird als cloudbasierter [Sicherungsdienst](./definition.md#backing-services)(getrennt von den-Diensten) gehostet.</span><span class="sxs-lookup"><span data-stu-id="be066-121">The cache is hosted as a cloud-based [backing service](./definition.md#backing-services), separate from the microservices.</span></span> <span data-ttu-id="be066-122">In Abbildung 5-20 wird die Architektur dargestellt.</span><span class="sxs-lookup"><span data-stu-id="be066-122">Figure 5-20 shows the architecture.</span></span>

![Zwischenspeichern in einer nativen Cloud-App](media/caching-in-a-cloud-native-app.png)

<span data-ttu-id="be066-124">**Abbildung 5-19**: Zwischenspeichern in einer nativen Cloud-App</span><span class="sxs-lookup"><span data-stu-id="be066-124">**Figure 5-19**: Caching in a cloud native app</span></span>

<span data-ttu-id="be066-125">Beachten Sie in der obigen Abbildung, wie der Cache von den-und-Diensten unabhängig ist und von diesen gemeinsam genutzt wird.</span><span class="sxs-lookup"><span data-stu-id="be066-125">In the previous figure, note how the cache is independent of and shared by the microservices.</span></span> <span data-ttu-id="be066-126">In diesem Szenario wird der Cache vom [API-Gateway](./front-end-communication.md)aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="be066-126">In this scenario, the cache is invoked by the [API Gateway](./front-end-communication.md).</span></span> <span data-ttu-id="be066-127">Wie in Kapitel 4 erläutert, fungiert das Gateway als Front-End für alle eingehenden Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="be066-127">As discussed in chapter 4, the gateway serves as a front end for all incoming requests.</span></span> <span data-ttu-id="be066-128">Der verteilte Cache erhöht die Reaktionsfähigkeit des Systems durch die Rückgabe von zwischengespeicherten Daten nach Möglichkeit.</span><span class="sxs-lookup"><span data-stu-id="be066-128">The distributed cache increases system responsiveness by returning cached data whenever possible.</span></span> <span data-ttu-id="be066-129">Wenn Sie den Cache von den Diensten trennen, kann der Cache außerdem unabhängig voneinander zentral hoch-oder herunterskaliert werden, um größere Datenverkehrs Anforderungen zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="be066-129">Additionally, separating the cache from the services allows the cache to scale up or out independently to meet increased traffic demands.</span></span>

<span data-ttu-id="be066-130">Die Abbildung zeigt ein gängiges zwischen Speicherungs Muster, das als [Cache Abbild Muster](https://docs.microsoft.com/azure/architecture/patterns/cache-aside)bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="be066-130">The figure presents a common caching pattern known as the [cache-aside pattern](https://docs.microsoft.com/azure/architecture/patterns/cache-aside).</span></span> <span data-ttu-id="be066-131">Bei einer eingehenden Anforderung Fragen Sie zuerst den Cache (Schritt \#1) nach einer Antwort ab.</span><span class="sxs-lookup"><span data-stu-id="be066-131">For an incoming request, you first query the cache (step \#1) for a response.</span></span> <span data-ttu-id="be066-132">Wenn Sie gefunden werden, werden die Daten sofort zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="be066-132">If found, the data is returned immediately.</span></span> <span data-ttu-id="be066-133">Wenn die Daten im Cache (als [Cache](https://www.techopedia.com/definition/6308/cache-miss)Fehler bezeichnet) nicht vorhanden sind, werden Sie aus einer lokalen Datenbank in einem Downstreamdienst abgerufen (Schritt \#2).</span><span class="sxs-lookup"><span data-stu-id="be066-133">If the data doesn't exist in the cache (known as a [cache miss](https://www.techopedia.com/definition/6308/cache-miss)), it's retrieved from a local database in a downstream service (step \#2).</span></span> <span data-ttu-id="be066-134">Sie wird dann für zukünftige Anforderungen (Schritt \#3) in den Cache geschrieben und an den Aufrufer zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="be066-134">It's then written to the cache for future requests (step \#3), and returned to the caller.</span></span> <span data-ttu-id="be066-135">Es muss darauf geachtet werden, dass zwischengespeicherte Daten regelmäßig entfernt werden, damit das System rechtzeitig und konsistent bleibt.</span><span class="sxs-lookup"><span data-stu-id="be066-135">Care must be taken to periodically evict cached data so that the system remains timely and consistent.</span></span>

<span data-ttu-id="be066-136">Wenn ein frei gegebener Cache wächst, kann es sich als vorteilhaft erweisen, seine Daten über mehrere Knoten hinweg zu partitionieren.</span><span class="sxs-lookup"><span data-stu-id="be066-136">As a shared cache grows, it might prove beneficial to partition its data across multiple nodes.</span></span> <span data-ttu-id="be066-137">Dies kann helfen, Konflikte zu minimieren und die Skalierbarkeit zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="be066-137">Doing so can help minimize contention and improve scalability.</span></span> <span data-ttu-id="be066-138">Viele Caching-Dienste unterstützen die Fähigkeit, Knoten dynamisch hinzuzufügen und zu entfernen und Daten über mehrere Partitionen hinweg neu auszugleichen.</span><span class="sxs-lookup"><span data-stu-id="be066-138">Many Caching services support the ability to dynamically add and remove nodes and rebalance data across partitions.</span></span> <span data-ttu-id="be066-139">Diese Vorgehensweise umfasst in der Regel Clustering.</span><span class="sxs-lookup"><span data-stu-id="be066-139">This approach typically involves clustering.</span></span> <span data-ttu-id="be066-140">Clustering macht eine Auflistung von Verbund Knoten als nahtlosen, einzelnen Cache verfügbar.</span><span class="sxs-lookup"><span data-stu-id="be066-140">Clustering exposes a collection of federated nodes as a seamless, single cache.</span></span> <span data-ttu-id="be066-141">Intern werden die Daten jedoch auf die Knoten verteilt, die auf eine vordefinierte Verteilungs Strategie folgen, die die Last gleichmäßig ausgleicht.</span><span class="sxs-lookup"><span data-stu-id="be066-141">Internally, however, the data is dispersed across the nodes following a predefined distribution strategy that balances the load evenly.</span></span>

## <a name="azure-cache-for-redis"></a><span data-ttu-id="be066-142">Azure-Cache für redis</span><span class="sxs-lookup"><span data-stu-id="be066-142">Azure Cache for Redis</span></span>

<span data-ttu-id="be066-143">[Azure Cache für redis](https://azure.microsoft.com/services/cache/) ist ein sicherer Daten Cache-und Messaging Broker Dienst, der vollständig von Microsoft verwaltet wird.</span><span class="sxs-lookup"><span data-stu-id="be066-143">[Azure Cache for Redis](https://azure.microsoft.com/services/cache/) is a secure data caching and messaging broker service, fully managed by Microsoft.</span></span> <span data-ttu-id="be066-144">Dieses Angebot wird als Platform-as-a-Service-Angebot (Platform-as-a-Service, PAS) genutzt und bietet einen hohen Durchsatz und geringe Latenz</span><span class="sxs-lookup"><span data-stu-id="be066-144">Consumed as a Platform as a Service (PaaS) offering, it provides high throughput and low-latency access to data.</span></span> <span data-ttu-id="be066-145">Der Dienst ist für jede Anwendung innerhalb oder außerhalb von Azure zugänglich.</span><span class="sxs-lookup"><span data-stu-id="be066-145">The service is accessible to any application within or outside of Azure.</span></span>

<span data-ttu-id="be066-146">Der Azure Cache for redis-Dienst verwaltet den Zugriff auf Open Source-redis-Server, die in Azure-Rechenzentren gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="be066-146">The Azure Cache for Redis service manages access to open-source Redis servers hosted across Azure data centers.</span></span> <span data-ttu-id="be066-147">Der-Dienst fungiert als Fassade, die Verwaltung, Zugriffs Steuerung und Sicherheit bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="be066-147">The service acts as a facade providing management, access control, and security.</span></span> <span data-ttu-id="be066-148">Der Dienst unterstützt nativ einen umfangreichen Satz von Datenstrukturen, einschließlich Zeichen folgen, Hashes, Listen und Sets.</span><span class="sxs-lookup"><span data-stu-id="be066-148">The service natively supports a rich set of data structures, including strings, hashes, lists, and sets.</span></span> <span data-ttu-id="be066-149">Wenn Ihre Anwendung bereits redis verwendet, funktioniert Sie mit Azure Cache for redis unverändert.</span><span class="sxs-lookup"><span data-stu-id="be066-149">If your application already uses Redis, it will work as-is with Azure Cache for Redis.</span></span>

<span data-ttu-id="be066-150">Azure Cache für redis ist mehr als ein einfacher Cache Server.</span><span class="sxs-lookup"><span data-stu-id="be066-150">Azure Cache for Redis is more than a simple cache server.</span></span> <span data-ttu-id="be066-151">Es kann eine Reihe von Szenarien unterstützen, um eine microservices-Architektur zu verbessern:</span><span class="sxs-lookup"><span data-stu-id="be066-151">It can support a number of scenarios to enhance a microservices architecture:</span></span>

- <span data-ttu-id="be066-152">Ein in-Memory-Datenspeicher</span><span class="sxs-lookup"><span data-stu-id="be066-152">An in-memory data store</span></span>
- <span data-ttu-id="be066-153">Eine verteilte nicht relationale Datenbank</span><span class="sxs-lookup"><span data-stu-id="be066-153">A distributed non-relational database</span></span>
- <span data-ttu-id="be066-154">Ein Nachrichten Broker</span><span class="sxs-lookup"><span data-stu-id="be066-154">A message broker</span></span>
- <span data-ttu-id="be066-155">Einen Konfigurations-oder Ermittlungs Server</span><span class="sxs-lookup"><span data-stu-id="be066-155">A configuration or discovery server</span></span>
  
<span data-ttu-id="be066-156">Für erweiterte Szenarien kann eine Kopie der zwischengespeicherten Daten [auf dem](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-persistence)Datenträger persistent gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="be066-156">For advanced scenarios, a copy of the cached data can be [persisted to disk](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-persistence).</span></span> <span data-ttu-id="be066-157">Wenn ein schwerwiegender Ereignis sowohl den primären als auch den Replikat Cache deaktiviert, wird der Cache aus der neuesten Momentaufnahme rekonstruiert.</span><span class="sxs-lookup"><span data-stu-id="be066-157">If a catastrophic event disables both the primary and replica caches, the cache is reconstructed from the most recent snapshot.</span></span>

<span data-ttu-id="be066-158">Azure redis Cache ist für eine Reihe vordefinierter Konfigurationen und Tarife verfügbar.</span><span class="sxs-lookup"><span data-stu-id="be066-158">Azure Redis Cache is available across a number of predefined configurations and pricing tiers.</span></span>  <span data-ttu-id="be066-159">Der [Premium](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-premium-tier-intro) -Tarif bietet viele Features auf Unternehmensebene, z. b. Clustering, Daten Persistenz, georeplikation und Virtual-Network-Isolation.</span><span class="sxs-lookup"><span data-stu-id="be066-159">The [Premium tier](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-premium-tier-intro) features many enterprise-level features such as clustering, data persistence, geo-replication, and virtual-network isolation.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="be066-160">[Zurück](relational-vs-nosql-data.md)
>[Weiter](elastic-search-in-azure.md)</span><span class="sxs-lookup"><span data-stu-id="be066-160">[Previous](relational-vs-nosql-data.md)
[Next](elastic-search-in-azure.md)</span></span>