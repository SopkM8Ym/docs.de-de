---
title: Beispielszenarien für Geschäftsszenarien und Anwendungsfälle für Server Lose apps
description: Erfahren Sie, wie Sie sich mit einem praktischen Ansatz vertraut machen, indem Sie auf Beispiele zugreifen, die von der Bildverarbeitung zu mobilen Back-Ends und ETL-Pipelines reichen.
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 06/26/2018
ms.openlocfilehash: adc4e1f3249cd72c423430ad4cb5dbb8eea8baf9
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "69577283"
---
# <a name="serverless-business-scenarios-and-use-cases"></a><span data-ttu-id="3e5c0-103">Geschäftsszenarios für serverlose Architekuren und Anwendungsfälle</span><span class="sxs-lookup"><span data-stu-id="3e5c0-103">Serverless business scenarios and use cases</span></span>

<span data-ttu-id="3e5c0-104">Es gibt viele Anwendungsfälle und Szenarien für Server lose Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-104">There are many use cases and scenarios for serverless applications.</span></span> <span data-ttu-id="3e5c0-105">Dieses Kapitel enthält Beispiele, die die verschiedenen Szenarien veranschaulichen.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-105">This chapter includes samples that illustrate the different scenarios.</span></span> <span data-ttu-id="3e5c0-106">Zu den Szenarien gehören Links zu verwandten Dokumentationen und öffentlichen Quellcode-Repository.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-106">The scenarios include links to related documentation and public source code repositories.</span></span> <span data-ttu-id="3e5c0-107">Die Beispiele in diesem Kapitel ermöglichen Ihnen den Einstieg in das entwickeln und Implementieren von Server losen Lösungen.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-107">The samples in this chapter enable you to get started on your own building and implementing serverless solutions.</span></span>

## <a name="analyze-and-archive-images"></a><span data-ttu-id="3e5c0-108">Analysieren und Archivieren von Bildern</span><span class="sxs-lookup"><span data-stu-id="3e5c0-108">Analyze and archive images</span></span>

<span data-ttu-id="3e5c0-109">In diesem Beispiel werden Server lose Ereignisse (Event Grid), Workflows (Logik-APP) und Code (Azure Functions) veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-109">This sample demonstrates serverless events (Event Grid), workflows (Logic App), and code (Azure Functions).</span></span> <span data-ttu-id="3e5c0-110">Außerdem wird gezeigt, wie Sie in eine andere Ressource integrieren, in diesem Fall Cognitive Services für die Bildanalyse.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-110">It also shows how to integrate with another resource, in this case Cognitive Services for image analysis.</span></span>

<span data-ttu-id="3e5c0-111">Eine Konsolenanwendung ermöglicht Ihnen das Übergeben eines Links an eine URL im Web.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-111">A console application allows you to pass a link to a URL on the web.</span></span> <span data-ttu-id="3e5c0-112">Die App veröffentlicht die URL als Event Grid-Nachricht.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-112">The app publishes the URL as an event grid message.</span></span> <span data-ttu-id="3e5c0-113">Parallel dazu wird die Nachricht von einer Server losen Funktions-APP und einer Logik-App abonniert.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-113">In parallel, a serverless function app and a logic app subscribe to the message.</span></span> <span data-ttu-id="3e5c0-114">Die Server lose Funktions-APP serialisiert das Image in BLOB Storage.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-114">The serverless function app serializes the image to blob storage.</span></span> <span data-ttu-id="3e5c0-115">Außerdem werden Informationen in Azure Table Storage gespeichert.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-115">It also stores information in Azure Table Storage.</span></span> <span data-ttu-id="3e5c0-116">Die Metadaten speichern die ursprüngliche Bild-URL und den Namen des BLOB-Bilds.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-116">The metadata stores the original image URL and the name of the blob image.</span></span> <span data-ttu-id="3e5c0-117">Die Logik-App interagiert mit der Custom Vision-API, um das Image zu analysieren und eine vom computergenerierte Beschriftung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-117">The logic app interacts with the Custom Vision API to analyze the image and create a machine-generated caption.</span></span> <span data-ttu-id="3e5c0-118">Die Beschriftung wird in der Metadatentabelle gespeichert.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-118">The caption is stored in the metadata table.</span></span>

![Analysieren und Archivieren von Images](./media/image-processing-example.png)

<span data-ttu-id="3e5c0-120">Eine separate Single-Page-Anwendung (Spa) Ruft eine Server lose Funktion auf, um eine Liste von Bildern und Metadaten zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-120">A separate single page application (SPA) calls a serverless function to get a list of images and metadata.</span></span> <span data-ttu-id="3e5c0-121">Für jedes Image wird eine andere Funktion aufgerufen, die die Bilddaten aus dem Archiv übermittelt.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-121">For each image, it calls another function that delivers the image data from the archive.</span></span> <span data-ttu-id="3e5c0-122">Das Endergebnis ist ein Katalog mit automatischen Beschriftungen.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-122">The final result is a gallery with automatic captions.</span></span>

![Automatisierter Image Katalog](./media/automated-image-gallery.png)

<span data-ttu-id="3e5c0-124">Das vollständige Repository und Anweisungen zum Erstellen der Logik-App finden Sie hier: [Event Grid-Kleber](https://github.com/JeremyLikness/Event-Grid-Glue).</span><span class="sxs-lookup"><span data-stu-id="3e5c0-124">The full repository and instructions to build the logic app are available here: [Event grid glue](https://github.com/JeremyLikness/Event-Grid-Glue).</span></span>

## <a name="cross-platform-mobile-client-using-xamarinforms-and-functions"></a><span data-ttu-id="3e5c0-125">Plattformübergreifender mobiler Client mit xamarin. Forms und Functions</span><span class="sxs-lookup"><span data-stu-id="3e5c0-125">Cross-platform mobile client using Xamarin.Forms and functions</span></span>

<span data-ttu-id="3e5c0-126">Erfahren Sie, wie Sie eine einfache Server lose Azure-Funktion im Azure-Webportal oder in Visual Studio implementieren.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-126">See how to implement a simple serverless Azure Function in the Azure Web Portal or in Visual Studio.</span></span> <span data-ttu-id="3e5c0-127">Erstellen Sie einen Client mit xamarin. Forms, der unter Android, IOS und Windows ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-127">Build a client with Xamarin.Forms that runs on Android, iOS, and Windows.</span></span> <span data-ttu-id="3e5c0-128">Die Anwendung wird dann für die Verwendung von JavaScript Object Notation (JSON) als Kommunikationsmedium zwischen dem Server und den mobilen Clients mit einem Server losen Back-End verfeinert.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-128">The application is then refined to use JavaScript Object Notation (JSON) as a communication medium between the server and the mobile clients with a serverless back end.</span></span>

<span data-ttu-id="3e5c0-129">Weitere Informationen finden Sie unter [Implementieren einer einfachen Azure-Funktion mit einem xamarin. Forms-Client.](https://azure.microsoft.com/resources/samples/functions-xamarin-getting-started/)</span><span class="sxs-lookup"><span data-stu-id="3e5c0-129">For more information, see [Implementing a simple Azure Function with a Xamarin.Forms client](https://azure.microsoft.com/resources/samples/functions-xamarin-getting-started/)</span></span>

## <a name="generate-a-photo-mosaic-with-serverless-image-recognition"></a><span data-ttu-id="3e5c0-130">Foto-Mosaik mit Server loser Bild Erkennung generieren</span><span class="sxs-lookup"><span data-stu-id="3e5c0-130">Generate a photo mosaic with serverless image recognition</span></span>

<span data-ttu-id="3e5c0-131">Das Beispiel verwendet Azure Functions und Microsoft Cognitive Services Custom Vision Service, um ein Photo-Mosaik aus einem Eingabebild zu generieren.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-131">The sample uses Azure Functions and Microsoft Cognitive Services Custom Vision Service to generate a photo mosaic from an input image.</span></span> <span data-ttu-id="3e5c0-132">Das Modell wird trainiert, um Bilder zu erkennen.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-132">The model is trained to recognize images.</span></span> <span data-ttu-id="3e5c0-133">Wenn ein Bild hochgeladen wird, wird das Bild erkannt und mit dem Suchvorgang gesucht.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-133">When an image is uploaded, it recognizes the image and searches with Bing.</span></span> <span data-ttu-id="3e5c0-134">Das ursprüngliche Bild wird mithilfe der Suchergebnisse neu zusammengesetzt.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-134">The original image is recomposed using the search results.</span></span>

![Orlando Eye Photo und Mosaic](./media/orlando-eye-both.png)

<span data-ttu-id="3e5c0-136">Beispielsweise können Sie Ihr Modell mit Orlando-, z. b. dem Orlando Eye, Schulen.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-136">For example, you can train your model with Orlando landmarks, such as the Orlando Eye.</span></span> <span data-ttu-id="3e5c0-137">Custom Vision erkennt ein Bild von Orlando Eye, und die Funktion erstellt ein Foto-Mosaik, das aus den Suchergebnissen von Suchergebnissen für "Orlando Eye" besteht.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-137">Custom Vision will recognize an image of the Orlando Eye, and the function will create a photo mosaic composed of Bing image search results for "Orlando Eye."</span></span>

<span data-ttu-id="3e5c0-138">Weitere Informationen finden Sie unter [Azure Functions Photo Mosaic Generator](https://azure.microsoft.com/resources/samples/functions-dotnet-photo-mosaic/).</span><span class="sxs-lookup"><span data-stu-id="3e5c0-138">For more information, see [Azure Functions photo mosaic generator](https://azure.microsoft.com/resources/samples/functions-dotnet-photo-mosaic/).</span></span>

## <a name="migrate-an-existing-application-to-the-cloud"></a><span data-ttu-id="3e5c0-139">Migrieren einer vorhandenen Anwendung in die Cloud</span><span class="sxs-lookup"><span data-stu-id="3e5c0-139">Migrate an existing application to the cloud</span></span>

<span data-ttu-id="3e5c0-140">Wie bereits in den vorherigen Kapiteln erläutert, ist es üblich, eine N-Tier-Architektur zu nutzen, um Ihre Anwendung lokal zu hosten.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-140">As discussed in previous chapters, it's common to embrace an N-Tier architecture to host your application on-premises.</span></span> <span data-ttu-id="3e5c0-141">Obwohl das Migrieren von Ressourcen "wie immer" mit virtuellen Computern der am wenigsten riskante Weg zur Cloud ist, entscheiden sich viele Unternehmen für die Umgestaltung Ihrer Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-141">Although migrating resources "as is" using virtual machines is the least risky path to the cloud, many companies choose to use the opportunity to refactor their applications.</span></span> <span data-ttu-id="3e5c0-142">Glücklicherweise muss das Refactoring keinen "All-or-Nothing"-Aufwand haben.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-142">Fortunately, refactoring doesn't have to be an "all-or-nothing" effort.</span></span> <span data-ttu-id="3e5c0-143">Tatsächlich ist es möglich, Ihre APP zu migrieren, und dann Komponenten durch die native Cloud-Entsprechung zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-143">In fact, it's possible to migrate your app then piecemeal replace components with cloud native counterparts.</span></span>

<span data-ttu-id="3e5c0-144">Die Anwendung verwendet die Proxys von Azure Functions, um das Refactoring eines Endpunkts aus dem Legacy lokalen Code zu einem Server losen Endpunkt zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-144">The application uses the proxies feature of Azure Functions to enable refactoring an endpoint from legacy on-premises code to a serverless endpoint.</span></span>

![Migrations Architektur](./media/migration-architecture.png)

<span data-ttu-id="3e5c0-146">Der Proxy stellt einen einzelnen API-Endpunkt bereit, der aktualisiert wird, um einzelne Anforderungen umzuleiten, wenn Sie in Server lose Funktionen verschoben werden.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-146">The proxy provides a single API endpoint that is updated to reroute individual requests as they're moved into serverless functions.</span></span>

<span data-ttu-id="3e5c0-147">Sie können sich ein Video ansehen, das die gesamte Migration durchläuft: [Lift & Shift mit Server losen Azure Functions](https://channel9.msdn.com/Events/Connect/2017/E102).</span><span class="sxs-lookup"><span data-stu-id="3e5c0-147">You can view a video that walks through the entire migration: [Lift and shift with serverless Azure functions](https://channel9.msdn.com/Events/Connect/2017/E102).</span></span> <span data-ttu-id="3e5c0-148">Greifen Sie auf den Beispielcode zu: [Bringen Sie Ihre eigene APP](https://github.com/JeremyLikness/bring-own-app-connect-17)ein.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-148">Access the sample code: [Bring your own app](https://github.com/JeremyLikness/bring-own-app-connect-17).</span></span>

## <a name="parse-a-csv-file-and-insert-into-a-database"></a><span data-ttu-id="3e5c0-149">Eine CSV-Datei analysieren und in eine Datenbank einfügen</span><span class="sxs-lookup"><span data-stu-id="3e5c0-149">Parse a CSV file and insert into a database</span></span>

<span data-ttu-id="3e5c0-150">Extrahieren, Transformieren und laden (ETL) ist eine gängige Geschäftsfunktion, die verschiedene Systeme integriert.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-150">Extract, Transform, and Load (ETL) is a common business function that integrates different systems.</span></span> <span data-ttu-id="3e5c0-151">Herkömmliche Ansätze umfassen häufig das Einrichten dedizierter FTP-Server und die anschließende Bereitstellung geplanter Aufträge, um Dateien zu analysieren und für die geschäftliche Verwendung zu übersetzen.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-151">Traditional approaches often involve setting up dedicated FTP servers then deploying scheduled jobs to parse files and translate them for business use.</span></span> <span data-ttu-id="3e5c0-152">Durch die Server lose Architektur wird die Aufgabe vereinfacht, da ein Trigger ausgelöst werden kann, wenn die Datei hochgeladen wird.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-152">Serverless architecture makes the job easier because a trigger can fire when the file is uploaded.</span></span> <span data-ttu-id="3e5c0-153">Azure Functions behandelt Aufgaben wie ETL durch die ideale Komposition von kleinen Code teilen, die sich auf ein bestimmtes Problem konzentrieren.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-153">Azure Functions tackles tasks like ETL through its ideal composition of small pieces of code that focus on a specific problem.</span></span>

![Screenshot, der den CSV-Prozess für die CSV-Verarbeitung anzeigt.](./media/serverless-business-scenarios/csv-parse-database-import.png)

<span data-ttu-id="3e5c0-155">Informationen zu Quellcode und praktischen Übungseinheiten finden Sie unter [CSV-Import-Lab](https://github.com/JeremyLikness/azure-fn-file-process-hol).</span><span class="sxs-lookup"><span data-stu-id="3e5c0-155">For source code and a hands-on lab, see [CSV import lab](https://github.com/JeremyLikness/azure-fn-file-process-hol).</span></span>

## <a name="shorten-links-and-track-metrics"></a><span data-ttu-id="3e5c0-156">Kürzen von Links und verfolgen von Metriken</span><span class="sxs-lookup"><span data-stu-id="3e5c0-156">Shorten links and track metrics</span></span>

<span data-ttu-id="3e5c0-157">Die Tools zum Verkürzen von Links trugen ursprünglich dazu bei, URLs in kurzen Twitter-Beiträgen zu codieren, um den Grenzwert von 140 Zeichen</span><span class="sxs-lookup"><span data-stu-id="3e5c0-157">Link shortening tools originally helped encode URLs in short twitter posts to accommodate the 140 character limit.</span></span> <span data-ttu-id="3e5c0-158">Sie wurden zu mehreren Verwendungsmöglichkeiten erweitert, meistens zum Nachverfolgen von Click-through-Vorgängen für Analysezwecke.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-158">They've grown to encompass several uses, most commonly to track click-throughs for analytics.</span></span> <span data-ttu-id="3e5c0-159">Das linkshortener-Szenario ist eine vollständig Server lose Anwendung zum Verwalten von Verknüpfungen und Berichts Metriken.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-159">The link shortener scenario is an entirely serverless application that manages links and reports metrics.</span></span>

<span data-ttu-id="3e5c0-160">Azure Functions wird für eine Single-Page-Anwendung (Spa) verwendet, mit der Sie die Long-URL einfügen und kurze URLs generieren können.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-160">Azure Functions is used to serve a Single Page Application (SPA) that allows you to paste the long URL and generate short URLs.</span></span> <span data-ttu-id="3e5c0-161">Die URLs werden gekennzeichnet, um Dinge wie Kampagnen (Themen) und Medien (z. b. soziale Netzwerke, an die die Links gesendet werden) nachverfolgen zu können.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-161">The URLs are tagged to track things like campaigns (topics) and mediums (such as social networks that the links are posted to).</span></span> <span data-ttu-id="3e5c0-162">Der kurze Code wird in Azure Table Storage als Schlüssel gespeichert, wobei die Long-URL den Wert hat.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-162">The short code is stored in Azure Table Storage as the key, with the long URL as the value.</span></span> <span data-ttu-id="3e5c0-163">Wenn Sie auf den Kurzlink klicken, wird von einer anderen Funktion die Long-URL nachgeschlagen, eine Umleitung gesendet und Informationen über das Ereignis in einer Warteschlange abgelegt.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-163">When you click on the short link, another function looks up the long URL, sends a redirect, and places information about the event on a queue.</span></span> <span data-ttu-id="3e5c0-164">Eine andere Azure-Funktion verarbeitet die Warteschlange und platziert die Informationen in Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-164">Another Azure Function processes the queue and places the information into Azure Cosmos DB.</span></span>

![Link Kürzel-Architektur](./media/link-shortener-architecture.png)

<span data-ttu-id="3e5c0-166">Sie können dann ein Power BI Dashboard erstellen, um Einblicke in die gesammelten Daten zu sammeln.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-166">You can then create a Power BI dashboard to gather insights about the data collected.</span></span> <span data-ttu-id="3e5c0-167">Im Back-End stellt Application Insights wichtige Metriken bereit.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-167">On the back end, Application Insights provides important metrics.</span></span> <span data-ttu-id="3e5c0-168">Die Telemetrie umfasst, wie lange es dauert, bis der durchschnittliche Benutzer umgeleitet wird und wie lange der Zugriff auf Azure Table Storage dauert.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-168">Telemetry includes how long it takes for the average user to redirect and how long it takes to access Azure Table Storage.</span></span>

![Power BI Beispiel](./media/power-bi-example.png)

<span data-ttu-id="3e5c0-170">Das vollständige Link-verkürzer-Repository mit Anweisungen finden Sie hier: [Server loser URL-Kürzel](https://github.com/jeremylikness/serverless-url-shortener).</span><span class="sxs-lookup"><span data-stu-id="3e5c0-170">The full link shortener repository with instructions is available here: [Serverless URL shortener](https://github.com/jeremylikness/serverless-url-shortener).</span></span> <span data-ttu-id="3e5c0-171">Informationen zu einer vereinfachten Version finden Sie hier: [Azure Storage für Server lose .net-apps in wenigen Minuten](https://blogs.msdn.microsoft.com/webdev/2018/01/25/azure-storage-for-serverless-net-apps-in-minutes/).</span><span class="sxs-lookup"><span data-stu-id="3e5c0-171">You can read about a simplified version here: [Azure Storage for serverless .NET apps in minutes](https://blogs.msdn.microsoft.com/webdev/2018/01/25/azure-storage-for-serverless-net-apps-in-minutes/).</span></span>

## <a name="verify-device-connectivity-using-a-ping"></a><span data-ttu-id="3e5c0-172">Überprüfen der Geräte Konnektivität per Ping</span><span class="sxs-lookup"><span data-stu-id="3e5c0-172">Verify device connectivity using a ping</span></span>

<span data-ttu-id="3e5c0-173">Das Beispiel besteht aus einer Azure IOT Hub und einer Azure-Funktion.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-173">The sample consists of an Azure IoT Hub and an Azure Function.</span></span> <span data-ttu-id="3e5c0-174">Eine neue Nachricht im IOT Hub löst die Azure-Funktion aus.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-174">A new message on the IoT Hub triggers the Azure Function.</span></span> <span data-ttu-id="3e5c0-175">Der Server lose Code sendet denselben Nachrichten Inhalt zurück an das Gerät, von dem das Gerät gesendet wurde.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-175">The serverless code sends the same message content back to the device that sent it.</span></span> <span data-ttu-id="3e5c0-176">Das Projekt verfügt über den gesamten Code und die Bereitstellungs Konfiguration, die für die Lösung erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="3e5c0-176">The project has all the code and deployment configuration needed for the solution.</span></span>

<span data-ttu-id="3e5c0-177">Weitere Informationen finden Sie unter [Azure IOT Hub Ping](https://azure.microsoft.com/resources/samples/iot-hub-node-ping/).</span><span class="sxs-lookup"><span data-stu-id="3e5c0-177">For more information, see [Azure IoT Hub ping](https://azure.microsoft.com/resources/samples/iot-hub-node-ping/).</span></span>

## <a name="recommended-resources"></a><span data-ttu-id="3e5c0-178">Empfohlene Ressourcen</span><span class="sxs-lookup"><span data-stu-id="3e5c0-178">Recommended resources</span></span>

* [<span data-ttu-id="3e5c0-179">Azure Functions Photo-Mosaik Generator</span><span class="sxs-lookup"><span data-stu-id="3e5c0-179">Azure Functions photo mosaic generator</span></span>](https://azure.microsoft.com/resources/samples/functions-dotnet-photo-mosaic/)
* [<span data-ttu-id="3e5c0-180">Azure IOT Hub Ping</span><span class="sxs-lookup"><span data-stu-id="3e5c0-180">Azure IoT Hub ping</span></span>](https://azure.microsoft.com/resources/samples/iot-hub-node-ping/)
* [<span data-ttu-id="3e5c0-181">Azure Storage für Server lose .net-apps in wenigen Minuten</span><span class="sxs-lookup"><span data-stu-id="3e5c0-181">Azure Storage for serverless .NET apps in minutes</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/01/25/azure-storage-for-serverless-net-apps-in-minutes/)
* [<span data-ttu-id="3e5c0-182">Bring your own App</span><span class="sxs-lookup"><span data-stu-id="3e5c0-182">Bring your own app</span></span>](https://github.com/JeremyLikness/bring-own-app-connect-17)
* [<span data-ttu-id="3e5c0-183">CSV-Import-Lab</span><span class="sxs-lookup"><span data-stu-id="3e5c0-183">CSV import lab</span></span>](https://github.com/JeremyLikness/azure-fn-file-process-hol)
* [<span data-ttu-id="3e5c0-184">Event Grid-Kleber</span><span class="sxs-lookup"><span data-stu-id="3e5c0-184">Event grid glue</span></span>](https://github.com/JeremyLikness/Event-Grid-Glue)
* [<span data-ttu-id="3e5c0-185">Implementieren einer einfachen Azure-Funktion mit einem xamarin. Forms-Client</span><span class="sxs-lookup"><span data-stu-id="3e5c0-185">Implementing a simple Azure Function with a Xamarin.Forms client</span></span>](https://azure.microsoft.com/resources/samples/functions-xamarin-getting-started/)
* [<span data-ttu-id="3e5c0-186">Lift & Shift mit Server losen Azure Functions</span><span class="sxs-lookup"><span data-stu-id="3e5c0-186">Lift and shift with serverless Azure functions</span></span>](https://channel9.msdn.com/Events/Connect/2017/E102)
* [<span data-ttu-id="3e5c0-187">Server loser URL-Kürzel</span><span class="sxs-lookup"><span data-stu-id="3e5c0-187">Serverless URL shortener</span></span>](https://github.com/jeremylikness/serverless-url-shortener)

>[!div class="step-by-step"]
><span data-ttu-id="3e5c0-188">[Zurück](orchestration-patterns.md)
>[Weiter](serverless-conclusion.md)</span><span class="sxs-lookup"><span data-stu-id="3e5c0-188">[Previous](orchestration-patterns.md)
[Next](serverless-conclusion.md)</span></span>