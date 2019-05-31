---
title: Verwenden von ML.NET in einem Szenario für die Erkennung von Vertriebsanomalien
description: Erfahren Sie, wie Sie ML.NET in einem Szenario zur Erkennung von Vertriebsanomalien einsetzen können, um zu verstehen, wie Sie die Daten im Hinblick auf Anomaliespitzen und Änderungspunkte analysieren können, um die entsprechenden Maßnahmen zu ergreifen.
ms.date: 05/06/2019
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: b0dbd8793e2be3973c37f0f78bc0ddd61b6bfea7
ms.sourcegitcommit: ca2ca60e6f5ea327f164be7ce26d9599e0f85fe4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65065653"
---
# <a name="tutorial-use-mlnet-for-product-sales-anomaly-detection"></a><span data-ttu-id="13e4e-103">Tutorial: Verwenden von ML.NET für die Erkennung von Vertriebsanomalien</span><span class="sxs-lookup"><span data-stu-id="13e4e-103">Tutorial: Use ML.NET for product sales anomaly detection</span></span> 

<span data-ttu-id="13e4e-104">Dieses Beispieltutorial veranschaulicht die Verwendung von ML.NET zur Erkennung von Anomalien in Produktvertriebsdaten, um über eine .NET Core-Konsolenanwendung mit C# in Visual Studio 2019 die entsprechenden Maßnahmen zu ergreifen.</span><span class="sxs-lookup"><span data-stu-id="13e4e-104">This sample tutorial illustrates using ML.NET to detect anomalies in product sales data to take the appropriate action via a .NET Core console application using C# in Visual Studio 2019.</span></span> 

<span data-ttu-id="13e4e-105">In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="13e4e-105">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="13e4e-106">Laden der Daten</span><span class="sxs-lookup"><span data-stu-id="13e4e-106">Load the data</span></span>
> * <span data-ttu-id="13e4e-107">Trainieren des Modells zur Erkennung von Anomaliespitzen</span><span class="sxs-lookup"><span data-stu-id="13e4e-107">Train the model for spike anomaly detection</span></span>
> * <span data-ttu-id="13e4e-108">Erkennen von Anomaliespitzen mit dem trainierten Modell</span><span class="sxs-lookup"><span data-stu-id="13e4e-108">Detect spike anomalies with the trained model</span></span>
> * <span data-ttu-id="13e4e-109">Trainieren des Modells zur Erkennung einer Änderungspunktanomalie</span><span class="sxs-lookup"><span data-stu-id="13e4e-109">Train the model for change point anomaly detection</span></span>
> * <span data-ttu-id="13e4e-110">Erkennen von Änderungspunktanomalien mit dem trainierten Modell</span><span class="sxs-lookup"><span data-stu-id="13e4e-110">Detect change point anomalies with the trained model</span></span>

<span data-ttu-id="13e4e-111">Sie finden den Quellcode für dieses Tutorial im Repository [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/ProductSalesAnomalyDetection).</span><span class="sxs-lookup"><span data-stu-id="13e4e-111">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/ProductSalesAnomalyDetection) repository.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13e4e-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="13e4e-112">Prerequisites</span></span>

* <span data-ttu-id="13e4e-113">[Visual Studio 2017 15.6 oder höher](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) mit installierter Workload „Plattformübergreifende .NET Core-Entwicklung“.</span><span class="sxs-lookup"><span data-stu-id="13e4e-113">[Visual Studio 2017 15.6 or later](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the ".NET Core cross-platform development" workload installed.</span></span>

* [<span data-ttu-id="13e4e-114">Das Dataset „product-sales.csv“</span><span class="sxs-lookup"><span data-stu-id="13e4e-114">The product-sales.csv dataset</span></span>](https://raw.githubusercontent.com/dotnet/machinelearning-samples/master/samples/csharp/getting-started/AnomalyDetection_Sales/SpikeDetection/Data/product-sales.csv)

>[!NOTE]
> <span data-ttu-id="13e4e-115">Das Datenformat in `product-sales.csv` basiert auf dem Dataset „Shampoo Sales Over a Three Year Period“ von Rob Hyndman, ursprünglich aus dem DataMarket und von der Time Series Data Library (TSDL) bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="13e4e-115">The data format in `product-sales.csv` is based on the dataset “Shampoo Sales Over a Three Year Period” originally sourced from DataMarket and provided by Time Series Data Library (TSDL), created by Rob Hyndman.</span></span> <span data-ttu-id="13e4e-116">Das Dataset „Shampoo Sales Over a Three Year Period“ ist im Rahmen der DataMarket Default Open License lizenziert.</span><span class="sxs-lookup"><span data-stu-id="13e4e-116">“Shampoo Sales Over a Three Year Period” Dataset Licensed Under the DataMarket Default Open License.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="13e4e-117">Erstellen einer Konsolenanwendung</span><span class="sxs-lookup"><span data-stu-id="13e4e-117">Create a console application</span></span>

1. <span data-ttu-id="13e4e-118">Erstellen Sie eine **.NET Core-Konsolenanwendung** mit dem Namen „ProductSalesAnomalyDetection“.</span><span class="sxs-lookup"><span data-stu-id="13e4e-118">Create a **.NET Core Console Application** called "ProductSalesAnomalyDetection".</span></span>

2. <span data-ttu-id="13e4e-119">Erstellen Sie ein Verzeichnis mit dem Namen *Data* in Ihrem Projekt, um die Datasetdateien zu speichern.</span><span class="sxs-lookup"><span data-stu-id="13e4e-119">Create a directory named *Data* in your project to save your data set files.</span></span>

3. <span data-ttu-id="13e4e-120">Installieren des **Microsoft.ML NuGet-Pakets**:</span><span class="sxs-lookup"><span data-stu-id="13e4e-120">Install the **Microsoft.ML NuGet Package**:</span></span>

    <span data-ttu-id="13e4e-121">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.</span><span class="sxs-lookup"><span data-stu-id="13e4e-121">In Solution Explorer, right-click on your project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="13e4e-122">Wählen Sie als Paketquelle „nuget.org“ aus. Wählen Sie anschließend die Registerkarte Durchsuchen aus, suchen Sie nach **Microsoft.ML**, und wählen Sie das **v1.0.0**-Paket in der Liste und anschließend die Schaltfläche **Installieren** aus.</span><span class="sxs-lookup"><span data-stu-id="13e4e-122">Choose "nuget.org" as the Package source, select the Browse tab, search for **Microsoft.ML**, select the **v1.0.0** package in the list, and select the **Install** button.</span></span> <span data-ttu-id="13e4e-123">Wählen Sie die Schaltfläche **OK** im Dialogfeld **Vorschau der Änderungen** und dann die Schaltfläche **Ich stimme zu** im Dialogfeld **Zustimmung zur Lizenz** aus, wenn Sie den Lizenzbedingungen für die aufgelisteten Pakete zustimmen.</span><span class="sxs-lookup"><span data-stu-id="13e4e-123">Select the **OK** button on the **Preview Changes** dialog and then select the **I Accept** button on the **License Acceptance** dialog if you agree with the license terms for the packages listed.</span></span> <span data-ttu-id="13e4e-124">Wiederholen Sie diese Schritte für **Microsoft.ML.TimeSeries v0.12.0**.</span><span class="sxs-lookup"><span data-stu-id="13e4e-124">Repeat these steps for **Microsoft.ML.TimeSeries v0.12.0**.</span></span>

4. <span data-ttu-id="13e4e-125">Fügen Sie am Anfang der Datei *Program.cs* die folgenden `using`-Anweisungen hinzu:</span><span class="sxs-lookup"><span data-stu-id="13e4e-125">Add the following `using` statements at the top of your *Program.cs* file:</span></span>

    [!code-csharp[AddUsings](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#AddUsings "Add necessary usings")]

### <a name="download-your-data"></a><span data-ttu-id="13e4e-126">Herunterladen der Daten</span><span class="sxs-lookup"><span data-stu-id="13e4e-126">Download your data</span></span>

1. <span data-ttu-id="13e4e-127">Laden Sie die das Dataset herunter, und speichern Sie es im Ordner *Data*, den Sie vorher erstellt haben:</span><span class="sxs-lookup"><span data-stu-id="13e4e-127">Download the dataset and save it to the *Data* folder you previously created:</span></span>

   * <span data-ttu-id="13e4e-128">Klicken Sie mit der rechten Maustaste auf [*product-sales.csv*](https://raw.githubusercontent.com/dotnet/machinelearning-samples/master/samples/csharp/getting-started/AnomalyDetection_Sales/SpikeDetection/Data/product-sales.csv) und anschließend auf „Link speichern unter“ oder „Ziel speichern unter“.</span><span class="sxs-lookup"><span data-stu-id="13e4e-128">Right click on [*product-sales.csv*](https://raw.githubusercontent.com/dotnet/machinelearning-samples/master/samples/csharp/getting-started/AnomalyDetection_Sales/SpikeDetection/Data/product-sales.csv) and select "Save Link (or Target) As..."</span></span>

     <span data-ttu-id="13e4e-129">Achten Sie darauf, die \*CSV-Datei entweder im Ordner *Data* zu speichern oder sie in den Ordner *Data* zu verschieben, nachdem Sie die \*CSV-Datei an anderer Stelle gespeichert haben.</span><span class="sxs-lookup"><span data-stu-id="13e4e-129">Make sure you either save the \*.csv file to the *Data* folder, or after you save it elsewhere, move the \*.csv file to the *Data* folder.</span></span>

2. <span data-ttu-id="13e4e-130">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die \*CSV Datei, und wählen Sie **Eigenschaften** aus.</span><span class="sxs-lookup"><span data-stu-id="13e4e-130">In Solution Explorer, right-click the \*.csv file and select **Properties**.</span></span> <span data-ttu-id="13e4e-131">Ändern Sie unter **Erweitert** den Wert von **In Ausgabeverzeichnis kopieren** in **Kopieren, wenn neuer**.</span><span class="sxs-lookup"><span data-stu-id="13e4e-131">Under **Advanced**, change the value of **Copy to Output Directory** to **Copy if newer**.</span></span>

<span data-ttu-id="13e4e-132">Die folgende Tabelle enthält eine Datenvorschau aus Ihrem \*CSV-Datei:</span><span class="sxs-lookup"><span data-stu-id="13e4e-132">The following table is a data preview from your \*.csv file:</span></span>

|<span data-ttu-id="13e4e-133">Monat</span><span class="sxs-lookup"><span data-stu-id="13e4e-133">Month</span></span>  |<span data-ttu-id="13e4e-134">ProductSales</span><span class="sxs-lookup"><span data-stu-id="13e4e-134">ProductSales</span></span> |
|-------|-------------|
|<span data-ttu-id="13e4e-135">1-Jan</span><span class="sxs-lookup"><span data-stu-id="13e4e-135">1-Jan</span></span>  |    <span data-ttu-id="13e4e-136">271</span><span class="sxs-lookup"><span data-stu-id="13e4e-136">271</span></span>      |
|<span data-ttu-id="13e4e-137">2-Jan</span><span class="sxs-lookup"><span data-stu-id="13e4e-137">2-Jan</span></span>  |    <span data-ttu-id="13e4e-138">150,9</span><span class="sxs-lookup"><span data-stu-id="13e4e-138">150.9</span></span>    |
|<span data-ttu-id="13e4e-139">.....</span><span class="sxs-lookup"><span data-stu-id="13e4e-139">.....</span></span>  |    <span data-ttu-id="13e4e-140">.....</span><span class="sxs-lookup"><span data-stu-id="13e4e-140">.....</span></span>    |
|<span data-ttu-id="13e4e-141">1-Feb</span><span class="sxs-lookup"><span data-stu-id="13e4e-141">1-Feb</span></span>  |    <span data-ttu-id="13e4e-142">199,3</span><span class="sxs-lookup"><span data-stu-id="13e4e-142">199.3</span></span>    |
|<span data-ttu-id="13e4e-143">.....</span><span class="sxs-lookup"><span data-stu-id="13e4e-143">.....</span></span>  |    <span data-ttu-id="13e4e-144">.....</span><span class="sxs-lookup"><span data-stu-id="13e4e-144">.....</span></span>    |

### <a name="create-classes-and-define-paths"></a><span data-ttu-id="13e4e-145">Erstellen von Klassen und Definieren von Pfaden</span><span class="sxs-lookup"><span data-stu-id="13e4e-145">Create classes and define paths</span></span>

<span data-ttu-id="13e4e-146">Als nächstes definieren Sie Ihre Datenstruktur der Eingabeklasse.</span><span class="sxs-lookup"><span data-stu-id="13e4e-146">Next, define your input class data structure.</span></span>

<span data-ttu-id="13e4e-147">Fügen Sie dem Projekt eine neue Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="13e4e-147">Add a new class to your project:</span></span>

1. <span data-ttu-id="13e4e-148">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und klicken Sie auf **Hinzufügen > Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="13e4e-148">In **Solution Explorer**, right-click the project, and then select **Add > New Item**.</span></span>

2. <span data-ttu-id="13e4e-149">Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option **Klasse** aus, und ändern Sie das Feld **Name** in*ProductSalesData.cs*.</span><span class="sxs-lookup"><span data-stu-id="13e4e-149">In the **Add New Item dialog box**, select **Class** and change the **Name** field to *ProductSalesData.cs*.</span></span> <span data-ttu-id="13e4e-150">Wählen Sie dann die Schaltfläche **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="13e4e-150">Then, select the **Add** button.</span></span>

<span data-ttu-id="13e4e-151">Die Datei *ProductSalesData.cs* wird im Code-Editor geöffnet.</span><span class="sxs-lookup"><span data-stu-id="13e4e-151">The *ProductSalesData.cs* file opens in the code editor.</span></span> <span data-ttu-id="13e4e-152">Fügen Sie die folgende `using`-Anweisung am Anfang der *ProductSalesData.cs*-Datei hinzu:</span><span class="sxs-lookup"><span data-stu-id="13e4e-152">Add the following `using` statement to the top of *ProductSalesData.cs*:</span></span>

```csharp
using Microsoft.ML.Data;
```

<span data-ttu-id="13e4e-153">Entfernen Sie die vorhandene Klassendefinition, und fügen Sie der Datei *ProductSalesData.cs* den folgenden Code mit den beiden Klassen `ProductSalesData` und `ProductSalesPrediction` hinzu:</span><span class="sxs-lookup"><span data-stu-id="13e4e-153">Remove the existing class definition and add the following code, which has two classes `ProductSalesData` and `ProductSalesPrediction`, to the *ProductSalesData.cs* file:</span></span>

[!code-csharp[DeclareTypes](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/ProductSalesData.cs#DeclareTypes "Declare data record types")]

<span data-ttu-id="13e4e-154">`ProductSalesData` ist eine Klasse für Eingabedaten.</span><span class="sxs-lookup"><span data-stu-id="13e4e-154">`ProductSalesData` specifies an input data class.</span></span> <span data-ttu-id="13e4e-155">Das [LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute.%23ctor%28System.Int32%29)-Attribut legt fest, welche Spalten (durch Angabe des Spaltenindex) im Dataset geladen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="13e4e-155">The [LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute.%23ctor%28System.Int32%29) attribute specifies which columns (by column index) in the dataset should be loaded.</span></span> 

<span data-ttu-id="13e4e-156">Fügen Sie am Anfang der Datei *Program.cs* folgende zusätzliche `using`-Anweisungen hinzu:</span><span class="sxs-lookup"><span data-stu-id="13e4e-156">Add the following additional `using` statements to the top of the *Program.cs* file:</span></span>

[!code-csharp[AddUsings](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#AddUsings "Add necessary usings")]

<span data-ttu-id="13e4e-157">Sie müssen zwei globale Felder zum Speichern des gerade heruntergeladenen Datasetdateipfads und des gespeicherten Modelldateipfads erstellen:</span><span class="sxs-lookup"><span data-stu-id="13e4e-157">You need to create two global fields to hold the recently downloaded dataset file path and the saved model file path:</span></span>

* <span data-ttu-id="13e4e-158">`_dataPath` enthält den Pfad zu dem Dataset, das zum Trainieren des Modells verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="13e4e-158">`_dataPath` has the path to the dataset used to train the model.</span></span>
* <span data-ttu-id="13e4e-159">`_docsize` enthält die Anzahl der Datensätze in der Datendatei.</span><span class="sxs-lookup"><span data-stu-id="13e4e-159">`_docsize` has the number of records in dataset file.</span></span> <span data-ttu-id="13e4e-160">Sie verwenden diese Informationen zum Berechnen von `pvalueHistoryLength`.</span><span class="sxs-lookup"><span data-stu-id="13e4e-160">You'll use this to calculate `pvalueHistoryLength`.</span></span>

<span data-ttu-id="13e4e-161">Fügen Sie den folgenden Code der Zeile direkt über der `Main`-Methode hinzu, um diese Pfade anzugeben:</span><span class="sxs-lookup"><span data-stu-id="13e4e-161">Add the following code to the line right above the `Main` method to specify those paths:</span></span>

[!code-csharp[Declare global variables](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#DeclareGlobalVariables "Declare global variables")]

<span data-ttu-id="13e4e-162">Die [MLContext-Klasse](xref:Microsoft.ML.MLContext) ist der Startpunkt für alle ML.NET-Vorgänge. Durch das Initialisieren von `mlContext` wird eine neue ML.NET-Umgebung erstellt, die für mehrere Objekte des Modellerstellungsworkflows verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="13e4e-162">The [MLContext class](xref:Microsoft.ML.MLContext) is a starting point for all ML.NET operations, and initializing `mlContext` creates a new ML.NET environment that can be shared across the model creation workflow objects.</span></span> <span data-ttu-id="13e4e-163">Die Klasse ähnelt dem Konzept von `DBContext` in Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="13e4e-163">It's similar, conceptually, to `DBContext` in Entity Framework.</span></span>

### <a name="initialize-variables-in-main"></a><span data-ttu-id="13e4e-164">Initialisieren von Variablen in Main</span><span class="sxs-lookup"><span data-stu-id="13e4e-164">Initialize variables in Main</span></span>

<span data-ttu-id="13e4e-165">Ersetzen Sie die Zeile `Console.WriteLine("Hello World!")` in der `Main`-Methode durch den folgenden Code, um die Variable `mlContext` zu deklarieren und zu initialisieren:</span><span class="sxs-lookup"><span data-stu-id="13e4e-165">Replace the `Console.WriteLine("Hello World!")` line in the `Main` method with the following code to declare and initialize the `mlContext` variable:</span></span>

[!code-csharp[CreateMLContext](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#CreateMLContext "Create the ML Context")]

### <a name="load-the-data"></a><span data-ttu-id="13e4e-166">Laden der Daten</span><span class="sxs-lookup"><span data-stu-id="13e4e-166">Load the data</span></span>

<span data-ttu-id="13e4e-167">Daten werden in ML.NET als [IDataView-Klasse](xref:Microsoft.ML.IDataView) dargestellt.</span><span class="sxs-lookup"><span data-stu-id="13e4e-167">Data in ML.NET is represented as an [IDataView class](xref:Microsoft.ML.IDataView).</span></span> <span data-ttu-id="13e4e-168">Mit `IDataView` können Tabellendaten (Zahlen und Text) flexibel und effizient beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="13e4e-168">`IDataView` is a flexible, efficient way of describing tabular data (numeric and text).</span></span> <span data-ttu-id="13e4e-169">Daten können aus einer Textdatei oder in Echtzeit (z. B. aus einer SQL-Datenbank oder aus Protokolldateien) in ein `IDataView`-Objekt geladen werden.</span><span class="sxs-lookup"><span data-stu-id="13e4e-169">Data can be loaded from a text file or in real time (for example, SQL database or log files) to an `IDataView` object.</span></span> <span data-ttu-id="13e4e-170">Fügen Sie der `Main()`-Methode folgenden Code als nächste Zeile hinzu:</span><span class="sxs-lookup"><span data-stu-id="13e4e-170">Add the following code as the next line of the `Main()` method:</span></span>

[!code-csharp[LoadData](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#LoadData "loading dataset")]

<span data-ttu-id="13e4e-171">Die [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29)-Methode definiert das Datenschema und liest die Datei ein.</span><span class="sxs-lookup"><span data-stu-id="13e4e-171">The [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) defines the data schema and reads in the file.</span></span> <span data-ttu-id="13e4e-172">Diese Methode akzeptiert Datenpfadvariablen und gibt ein `IDataView`-Objekt zurück.</span><span class="sxs-lookup"><span data-stu-id="13e4e-172">It takes in the data path variables and returns an `IDataView`.</span></span>

## <a name="ml-task---time-series-anomaly-detection"></a><span data-ttu-id="13e4e-173">ML-Aufgabe – Anomalieerkennung in einer Zeitreihe</span><span class="sxs-lookup"><span data-stu-id="13e4e-173">ML task - time series anomaly detection</span></span> 

<span data-ttu-id="13e4e-174">Die Anomalieerkennung zeigt unerwartete oder ungewöhnliche Ereignisse oder Verhaltensweisen an.</span><span class="sxs-lookup"><span data-stu-id="13e4e-174">Anomaly detection flags unexpected or unusual events or behaviors.</span></span> <span data-ttu-id="13e4e-175">Sie erhalten Hinweise darauf, wo Sie nach Problemen schauen müssen, und können die Frage beantworten, ob das Ereignis oder Verhalten ungewöhnlich ist.</span><span class="sxs-lookup"><span data-stu-id="13e4e-175">It gives clues where to look for problems and helps you answer the question "Is this weird?".</span></span>

![Ist das ungewöhnlich](./media/sales-anomaly-detection/anomalydetection.png)

<span data-ttu-id="13e4e-177">Die Anomalieerkennung ist ein Prozess, bei dem Ausreißer in Zeitreihendaten ermittelt. Damit wird auf Eingabezeitreihen hingewiesen, bei denen nicht das erwartete oder ein ungewöhnliches Verhalten aufgetreten ist.</span><span class="sxs-lookup"><span data-stu-id="13e4e-177">Anomaly detection is the process of detecting time-series data outliers; points on a given input time-series where the behavior isn't what was expected, or "weird".</span></span>

<span data-ttu-id="13e4e-178">Dies kann in vielerlei Hinsicht hilfreich sein.</span><span class="sxs-lookup"><span data-stu-id="13e4e-178">This can be useful in lots of ways.</span></span> <span data-ttu-id="13e4e-179">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="13e4e-179">For instance:</span></span>

<span data-ttu-id="13e4e-180">Wenn Sie ein Auto besitzen, möchten Sie vielleicht wissen: Ist diese Ölstandsanzeige normal oder habe ich ein Leck?</span><span class="sxs-lookup"><span data-stu-id="13e4e-180">If you have a car, you might want to know: Is this oil gauge reading normal, or do I have a leak?</span></span>
<span data-ttu-id="13e4e-181">Wenn Sie den Energieverbrauch überwachen, möchten Sie vielleicht wissen: Gibt es einen Stromausfall?</span><span class="sxs-lookup"><span data-stu-id="13e4e-181">If you're monitoring power consumption, you’d want to know: Is there an outage?</span></span>

<span data-ttu-id="13e4e-182">Es können zwei Arten von Zeitreihenanomalien erkannt werden:</span><span class="sxs-lookup"><span data-stu-id="13e4e-182">There are two types of time series anomalies that can be detected:</span></span> 

* <span data-ttu-id="13e4e-183">**Spitzen** zeigen temporäre Häufungen von anomalem Verhalten im System an.</span><span class="sxs-lookup"><span data-stu-id="13e4e-183">**Spikes** indicate temporary bursts of anomalous behavior in the system.</span></span> 

* <span data-ttu-id="13e4e-184">**Änderungspunkte** zeigen den Beginn anhaltender Änderungen im Zeitverlauf im System an.</span><span class="sxs-lookup"><span data-stu-id="13e4e-184">**Change points** indicate the beginning of persistent changes over time in the system.</span></span> 

<span data-ttu-id="13e4e-185">In ML.NET eignen sich die Algorithmen „IID Spike Detection“ oder „IID Change point Detection“ für [unabhängige und identisch verteilte Datasets](https://en.wikipedia.org/wiki/Independent_and_identically_distributed_random_variables).</span><span class="sxs-lookup"><span data-stu-id="13e4e-185">In ML.NET, The IID Spike Detection or IID Change point Detection algorithms are suited for [independent and identically distributed datasets](https://en.wikipedia.org/wiki/Independent_and_identically_distributed_random_variables).</span></span> 

<span data-ttu-id="13e4e-186">Sie analysieren die gleichen Produktvertriebsdaten, um Spitzen und Änderungspunkte zu erkennen.</span><span class="sxs-lookup"><span data-stu-id="13e4e-186">You'll analyze the same product sales data to detect spikes and change points.</span></span> <span data-ttu-id="13e4e-187">Der Prozess zum Erstellen und Trainieren eines Modells ist für die Erkennung von Spitzen und Änderungspunkten gleich; der Hauptunterschied besteht in dem verwendeten spezifischen Erkennungsalgorithmus.</span><span class="sxs-lookup"><span data-stu-id="13e4e-187">The building and training model process is the same for spike detection and change point detection; the main difference is the specific detection algorithm used.</span></span>

## <a name="spike-detection"></a><span data-ttu-id="13e4e-188">Spitzenerkennung</span><span class="sxs-lookup"><span data-stu-id="13e4e-188">Spike detection</span></span> 

<span data-ttu-id="13e4e-189">Das Ziel der Spitzenerkennung ist es, plötzliche, aber temporäre Häufungen zu identifizieren, die sich signifikant von der Mehrzahl der Zeitreihen-Datenwerte unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="13e4e-189">The goal of spike detection is to identify sudden yet temporary bursts that significantly differ from the majority of the time series data values.</span></span> <span data-ttu-id="13e4e-190">Es ist wichtig, diese verdächtigen seltenen Elemente, Ereignisse oder Beobachtungen rechtzeitig zu erkennen, um sie zu minimieren.</span><span class="sxs-lookup"><span data-stu-id="13e4e-190">It's important to detect these suspicious rare items, events or observations in a timely manner to be minimized.</span></span> <span data-ttu-id="13e4e-191">Der folgende Ansatz kann zur Erkennung einer Vielzahl von Anomalien verwendet werden: z.B. Ausfälle, Cyberangriffe oder virale Webinhalte.</span><span class="sxs-lookup"><span data-stu-id="13e4e-191">The following approach can be used to detect a variety of anomalies such as: outages, cyber-attacks, or viral web content.</span></span> <span data-ttu-id="13e4e-192">Das folgende Bild zeigt ein Beispiel für Spitzen in einem Zeitreihen-Dataset:</span><span class="sxs-lookup"><span data-stu-id="13e4e-192">The following image is an example of spikes in a time series dataset:</span></span>

![SpikeDetection](./media/sales-anomaly-detection/SpikeDetection.png)

### <a name="create-the-detectspike-method"></a><span data-ttu-id="13e4e-194">Erstellen der DetectSpike()-Methode</span><span class="sxs-lookup"><span data-stu-id="13e4e-194">Create the DetectSpike() method</span></span>

<span data-ttu-id="13e4e-195">Fügen Sie der `DetectSpike()`-Methode den folgenden Aufruf als nächste Codezeile der `Main()`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="13e4e-195">Add the following call to the `DetectSpike()`method as the next line of code in the `Main()` method:</span></span>

[!code-csharp[CallDetectSpike](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#CallDetectSpike)]

<span data-ttu-id="13e4e-196">Die `DetectSpike()`-Methode führt die folgenden Aufgaben aus:</span><span class="sxs-lookup"><span data-stu-id="13e4e-196">The `DetectSpike()` method executes the following tasks:</span></span>

* <span data-ttu-id="13e4e-197">Trainieren des Modells.</span><span class="sxs-lookup"><span data-stu-id="13e4e-197">Trains the model.</span></span>
* <span data-ttu-id="13e4e-198">Erkennen von Spitzen basierend auf historischen Vertriebsdaten.</span><span class="sxs-lookup"><span data-stu-id="13e4e-198">Detects spikes based on on historical sales data.</span></span>
* <span data-ttu-id="13e4e-199">Anzeigen der Ergebnisse.</span><span class="sxs-lookup"><span data-stu-id="13e4e-199">Displays the results.</span></span>

<span data-ttu-id="13e4e-200">Erstellen Sie die `DetectSpike()`-Methode mit dem folgenden Code direkt nach der `Main()`-Methode:</span><span class="sxs-lookup"><span data-stu-id="13e4e-200">Create the `DetectSpike()` method, just after the `Main()` method, using the following code:</span></span>

```csharp
static void DetectSpike(MLContext mlContext, int docSize, IDataView productSales)
{

}
```

<span data-ttu-id="13e4e-201">Verwenden Sie [IidSpikeEstimator](xref:Microsoft.ML.Transforms.TimeSeries.IidSpikeEstimator), um das Modell zum Erkennen von Spitzen zu trainieren.</span><span class="sxs-lookup"><span data-stu-id="13e4e-201">Use the [IidSpikeEstimator](xref:Microsoft.ML.Transforms.TimeSeries.IidSpikeEstimator) to train the model for spike detection.</span></span> <span data-ttu-id="13e4e-202">Fügen Sie ihn mit dem folgenden Code zur `DetectChangepoint()`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="13e4e-202">Add it to the `DetectChangepoint()` method with the following code:</span></span>

[!code-csharp[AddSpikeTrainer](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#AddSpikeTrainer)]

<span data-ttu-id="13e4e-203">Fügen Sie den unten aufgeführten Code als nächste Codezeilen in die `DetectSpike()`-Methode ein, um das Modell an die `productSales`-Daten anzupassen und das trainierte Modell zurückzugeben:</span><span class="sxs-lookup"><span data-stu-id="13e4e-203">Fit the model to the `productSales` data and return the trained model by adding the following as the next line of code in the `DetectSpike()` method:</span></span>

[!code-csharp[TrainModel1](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#TrainModel1)]

<span data-ttu-id="13e4e-204">Mit der [Fit()](xref:Microsoft.ML.Data.TrivialEstimator%601.Fit%2A)-Methode wird Ihr Modell trainiert, indem das Dataset transformiert und das Training angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="13e4e-204">The [Fit()](xref:Microsoft.ML.Data.TrivialEstimator%601.Fit%2A) method trains your model by transforming the dataset and applying the training.</span></span>

<span data-ttu-id="13e4e-205">Fügen Sie die folgende Codezeile hinzu, um die `productSales`-Daten als nächste Zeile in der `DetectSpike()`-Methode zu transformieren:</span><span class="sxs-lookup"><span data-stu-id="13e4e-205">Add the following line of code to transform the `productSales` data as the next line in the `DetectSpike()` method:</span></span>

[!code-csharp[TransformData1](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#TransformData1)]

<span data-ttu-id="13e4e-206">Der vorherige Code verwendet die [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A)-Methode trifft Vorhersagen für mehrere bereitgestellte Eingabezeilen eines Testdatasets.</span><span class="sxs-lookup"><span data-stu-id="13e4e-206">The previous code uses the [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) method to make predictions for multiple provided input rows of a test dataset.</span></span>

<span data-ttu-id="13e4e-207">Konvertieren Sie Ihre `transformedData` mit der [CreateEnumerable()](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A)-Methode mit dem folgenden Code in ein stark typisiertes `IEnumerable` zur einfacheren Anzeige:</span><span class="sxs-lookup"><span data-stu-id="13e4e-207">Convert your `transformedData` into a strongly-typed `IEnumerable` for easier display using the [CreateEnumerable()](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) method with the following code:</span></span>

[!code-csharp[CreateEnumerable1](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#CreateEnumerable1)]

<span data-ttu-id="13e4e-208">Erstellen Sie mit dem folgenden <xref:System.Console.WriteLine?displayProperty=nameWithType>-Code eine Anzeige für die Kopfzeile:</span><span class="sxs-lookup"><span data-stu-id="13e4e-208">Create a display header line using the following <xref:System.Console.WriteLine?displayProperty=nameWithType> code:</span></span>

[!code-csharp[DisplayHeader1](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#DisplayHeader1)]

<span data-ttu-id="13e4e-209">In den Ergebnissen der Spitzenerkennung werden folgende Informationen angezeigt:</span><span class="sxs-lookup"><span data-stu-id="13e4e-209">You'll display the following information in your spike detection results:</span></span>

* <span data-ttu-id="13e4e-210">`Alert` gibt eine Spitzenwarnung für einen bestimmten Datenpunkt an.</span><span class="sxs-lookup"><span data-stu-id="13e4e-210">`Alert` indicates a spike alert for a given data point.</span></span>

* <span data-ttu-id="13e4e-211">`Score` ist der `ProductSales`-Wert für einen bestimmten Datenpunkt im Dataset.</span><span class="sxs-lookup"><span data-stu-id="13e4e-211">`Score` is the `ProductSales` value for a given data point in the dataset.</span></span>

* <span data-ttu-id="13e4e-212">`P-Value` Das „P“ steht für „Probability“ (Wahrscheinlichkeit).</span><span class="sxs-lookup"><span data-stu-id="13e4e-212">`P-Value` The "P" stands for probability.</span></span> <span data-ttu-id="13e4e-213">Damit wird angegeben, wie wahrscheinlich es ist, dass dieser Datenpunkt eine Anomalie ist.</span><span class="sxs-lookup"><span data-stu-id="13e4e-213">This indicates how likely this data point is an anomaly.</span></span> 

<span data-ttu-id="13e4e-214">Verwenden Sie den folgenden Code, um `predictions` `IEnumerable` zu durchlaufen und die Ergebnisse anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="13e4e-214">Use the following code to iterate through the `predictions` `IEnumerable` and display the results:</span></span>

[!code-csharp[DisplayResults1](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#DisplayResults1)]

## <a name="spike-detection-results"></a><span data-ttu-id="13e4e-215">Ergebnisse der Spitzenerkennung</span><span class="sxs-lookup"><span data-stu-id="13e4e-215">Spike detection results</span></span>

<span data-ttu-id="13e4e-216">Die Ergebnisse sollten den unten dargestellten ähneln.</span><span class="sxs-lookup"><span data-stu-id="13e4e-216">Your results should be similar to the following.</span></span> <span data-ttu-id="13e4e-217">Während der Verarbeitung werden Meldungen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="13e4e-217">During processing, messages are displayed.</span></span> <span data-ttu-id="13e4e-218">Sie können Warnungen oder Verarbeitungsmeldungen sehen.</span><span class="sxs-lookup"><span data-stu-id="13e4e-218">You may see warnings, or processing messages.</span></span> <span data-ttu-id="13e4e-219">Diese wurden der Übersichtlichkeit halber aus den folgenden Ergebnissen entfernt.</span><span class="sxs-lookup"><span data-stu-id="13e4e-219">These have been removed from the following results for clarity.</span></span>

```console
Detect temporary changes in pattern
=============== Training the model ===============
=============== End of training process ===============
Alert   Score   P-Value
0       271.00  0.50
0       150.90  0.00
0       188.10  0.41
0       124.30  0.13
0       185.30  0.47
0       173.50  0.47
0       236.80  0.19
0       229.50  0.27
0       197.80  0.48
0       127.90  0.13
1       341.50  0.00 <-- Spike detected
0       190.90  0.48
0       199.30  0.48
0       154.50  0.24
0       215.10  0.42
0       278.30  0.19
0       196.40  0.43
0       292.00  0.17
0       231.00  0.45
0       308.60  0.18
0       294.90  0.19
1       426.60  0.00 <-- Spike detected
0       269.50  0.47
0       347.30  0.21
0       344.70  0.27
0       445.40  0.06
0       320.90  0.49
0       444.30  0.12
0       406.30  0.29
0       442.40  0.21
1       580.50  0.00 <-- Spike detected
0       412.60  0.45
1       687.00  0.01 <-- Spike detected
0       480.30  0.40
0       586.30  0.20
0       651.90  0.14
```

## <a name="change-point-detection"></a><span data-ttu-id="13e4e-220">Erkennen von Änderungspunkten</span><span class="sxs-lookup"><span data-stu-id="13e4e-220">Change point detection</span></span>

<span data-ttu-id="13e4e-221">`Change points` sind anhaltende Änderungen in der Verteilung der Werte in einem Zeitreihen-Ereignisstrom, wie Niveauänderungen und Trends.</span><span class="sxs-lookup"><span data-stu-id="13e4e-221">`Change points` are persistent changes in a time series event stream distribution of values, like level changes and trends.</span></span> <span data-ttu-id="13e4e-222">Diese anhaltenden Änderungen dauern wesentlich länger als `spikes` und könnten auf katastrophale Ereignisse hinweisen.</span><span class="sxs-lookup"><span data-stu-id="13e4e-222">These persistent changes last much longer than `spikes` and could indicate catastrophic event(s).</span></span> <span data-ttu-id="13e4e-223">`Change points` sind in der Regel nicht mit bloßem Auge erkennbar, können aber mit Ansätzen wie beispielsweise der folgenden Methode in Ihren Daten festgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="13e4e-223">`Change points` are not usually visible to the naked eye, but can be detected in your data using approaches such as in the following method.</span></span>  <span data-ttu-id="13e4e-224">Das folgende Bild ist ein Beispiel für eine Änderungspunkterkennung:</span><span class="sxs-lookup"><span data-stu-id="13e4e-224">The following image is an example of a change point detection:</span></span>

![ChangePointDetection](./media/sales-anomaly-detection/ChangePointDetection.png)

### <a name="create-the-detectchangepoint-method"></a><span data-ttu-id="13e4e-226">Erstellen der DetectChangepoint()-Methode</span><span class="sxs-lookup"><span data-stu-id="13e4e-226">Create the DetectChangepoint() method</span></span>

<span data-ttu-id="13e4e-227">Fügen Sie der `DetectChangepoint()`-Methode den folgenden Aufruf als nächste Codezeile der `Main()`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="13e4e-227">Add the following call to the `DetectChangepoint()`method as the next line of code in the `Main()` method:</span></span>

[!code-csharp[CallDetectChangepoint](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#CallDetectChangepoint)]

<span data-ttu-id="13e4e-228">Die `DetectChangepoint()`-Methode führt die folgenden Aufgaben aus:</span><span class="sxs-lookup"><span data-stu-id="13e4e-228">The `DetectChangepoint()` method executes the following tasks:</span></span>

* <span data-ttu-id="13e4e-229">Trainieren des Modells.</span><span class="sxs-lookup"><span data-stu-id="13e4e-229">Trains the model.</span></span>
* <span data-ttu-id="13e4e-230">Erkennen von Änderungspunkten basierend auf historischen Vertriebsdaten.</span><span class="sxs-lookup"><span data-stu-id="13e4e-230">Detects change points based on historical sales data.</span></span>
* <span data-ttu-id="13e4e-231">Anzeigen der Ergebnisse.</span><span class="sxs-lookup"><span data-stu-id="13e4e-231">Displays the results.</span></span>

<span data-ttu-id="13e4e-232">Erstellen Sie die `DetectChangepoint()`-Methode mit dem folgenden Code direkt nach der `Main()`-Methode:</span><span class="sxs-lookup"><span data-stu-id="13e4e-232">Create the `DetectChangepoint()` method, just after the `Main()` method, using the following code:</span></span>

```csharp
static void DetectChangepoint(MLContext mlContext, int docSize, IDataView productSales)
{

}
```

<span data-ttu-id="13e4e-233">[iidChangePointEstimator](xref:Microsoft.ML.Transforms.TimeSeries.IidChangePointEstimator) wird zum Trainieren des Modells für die Erkennung von Änderungspunkten verwendet.</span><span class="sxs-lookup"><span data-stu-id="13e4e-233">The [iidChangePointEstimator](xref:Microsoft.ML.Transforms.TimeSeries.IidChangePointEstimator) is used to train the model for change point detection.</span></span> <span data-ttu-id="13e4e-234">Fügen Sie ihn mit dem folgenden Code zur `DetectChangepoint()`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="13e4e-234">Add it to the `DetectChangepoint()` method with the following code:</span></span>

[!code-csharp[AddChangepointTrainer](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#AddChangepointTrainer)]

<span data-ttu-id="13e4e-235">Fügen Sie wie bereits zuvor den unten aufgeführten Code als nächste Codezeilen in die `DetectChangePoint()`-Methode ein, um das Modell an die `productSales`-Daten anzupassen und das trainierte Modell zurückzugeben:</span><span class="sxs-lookup"><span data-stu-id="13e4e-235">As you did previously, fit the model to the `productSales` data and return the trained model by adding the following as the next line of code in the `DetectChangePoint()` method:</span></span>

[!code-csharp[TrainModel2](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#TrainModel2)]

<span data-ttu-id="13e4e-236">Verwenden Sie die `Transform()`-Methode, um die `Training`-Daten zu transformieren, indem Sie den folgenden Code zu `DetectChangePoint()` hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="13e4e-236">Use the `Transform()` method to transform the `Training` data by adding the following code to `DetectChangePoint()`:</span></span>

[!code-csharp[TransformData2](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#TransformData2)]

<span data-ttu-id="13e4e-237">Konvertieren Sie wie bereits zuvor Ihre `transformedData` mit der `CreateEnumerable()`-Methode mit dem folgenden Code in ein stark typisiertes `IEnumerable` zur einfacheren Anzeige:</span><span class="sxs-lookup"><span data-stu-id="13e4e-237">As you did previously, convert your `transformedData` into a strongly-typed `IEnumerable` for easier display using the `CreateEnumerable()`method with the following code:</span></span>

[!code-csharp[CreateEnumerable2](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#CreateEnumerable2)]

<span data-ttu-id="13e4e-238">Erstellen Sie mit dem folgenden Code als nächste Zeile in der `DetectChangePoint()`-Methode einen Anzeigeheader:</span><span class="sxs-lookup"><span data-stu-id="13e4e-238">Create a display header with the following code as the next line in the `DetectChangePoint()` method:</span></span>

[!code-csharp[DisplayHeader2](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#DisplayHeader2)]

<span data-ttu-id="13e4e-239">In den Ergebnissen der Änderungspunkterkennung werden folgende Informationen angezeigt:</span><span class="sxs-lookup"><span data-stu-id="13e4e-239">You'll display the following information in your change point detection results:</span></span>

* <span data-ttu-id="13e4e-240">`Alert` gibt eine Änderungspunktwarnung für einen bestimmten Datenpunkt an.</span><span class="sxs-lookup"><span data-stu-id="13e4e-240">`Alert` indicates a change point alert for a given data point.</span></span>
* <span data-ttu-id="13e4e-241">`Score` ist der `ProductSales`-Wert für einen bestimmten Datenpunkt im Dataset.</span><span class="sxs-lookup"><span data-stu-id="13e4e-241">`Score` is the `ProductSales` value for a given data point in the dataset.</span></span>
* <span data-ttu-id="13e4e-242">`P-Value` Das „P“ steht für „Probability“ (Wahrscheinlichkeit).</span><span class="sxs-lookup"><span data-stu-id="13e4e-242">`P-Value` The "P" stands for probability.</span></span> <span data-ttu-id="13e4e-243">Damit wird angegeben, wie wahrscheinlich es ist, dass dieser Datenpunkt eine Anomalie ist.</span><span class="sxs-lookup"><span data-stu-id="13e4e-243">This indicates how likely this data point is an anomaly.</span></span> 
* <span data-ttu-id="13e4e-244">`Martingale value` wird verwendet, um basierend auf der Reihenfolge der P-Werte zu bestimmen, wie „ungewöhnlich ein Datenpunkt ist.</span><span class="sxs-lookup"><span data-stu-id="13e4e-244">`Martingale value` is used to identify how "weird" a data point is, based on the sequence of P-values.</span></span>  

<span data-ttu-id="13e4e-245">Mit dem folgenden Code können Sie `predictions` `IEnumerable` durchlaufen und die Ergebnisse anzeigen:</span><span class="sxs-lookup"><span data-stu-id="13e4e-245">Iterate through the `predictions` `IEnumerable` and display the results with the following code:</span></span>

[!code-csharp[DisplayResults2](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#DisplayResults2)]

## <a name="change-point-detection-results"></a><span data-ttu-id="13e4e-246">Ergebnisse der Änderungspunkterkennung</span><span class="sxs-lookup"><span data-stu-id="13e4e-246">Change point detection results</span></span>

<span data-ttu-id="13e4e-247">Die Ergebnisse sollten den unten dargestellten ähneln.</span><span class="sxs-lookup"><span data-stu-id="13e4e-247">Your results should be similar to the following.</span></span> <span data-ttu-id="13e4e-248">Während der Verarbeitung werden Meldungen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="13e4e-248">During processing, messages are displayed.</span></span> <span data-ttu-id="13e4e-249">Sie können Warnungen oder Verarbeitungsmeldungen sehen.</span><span class="sxs-lookup"><span data-stu-id="13e4e-249">You may see warnings, or processing messages.</span></span> <span data-ttu-id="13e4e-250">Diese wurden der Übersichtlichkeit halber aus den folgenden Ergebnissen entfernt.</span><span class="sxs-lookup"><span data-stu-id="13e4e-250">These have been removed from the following results for clarity.</span></span>

```console
Detect Persistent changes in pattern
=============== Training the model Using Change Point Detection Algorithm===============
=============== End of training process ===============
Alert   Score   P-Value Martingale value
0       271.00  0.50    0.00
0       150.90  0.00    2.33
0       188.10  0.41    2.80
0       124.30  0.13    9.16
0       185.30  0.47    9.77
0       173.50  0.47    10.41
0       236.80  0.19    24.46
0       229.50  0.27    42.38
1       197.80  0.48    44.23 <-- alert is on, predicted changepoint
0       127.90  0.13    145.25
0       341.50  0.00    0.01
0       190.90  0.48    0.01
0       199.30  0.48    0.00
0       154.50  0.24    0.00
0       215.10  0.42    0.00
0       278.30  0.19    0.00
0       196.40  0.43    0.00
0       292.00  0.17    0.01
0       231.00  0.45    0.00
0       308.60  0.18    0.00
0       294.90  0.19    0.00
0       426.60  0.00    0.00
0       269.50  0.47    0.00
0       347.30  0.21    0.00
0       344.70  0.27    0.00
0       445.40  0.06    0.02
0       320.90  0.49    0.01
0       444.30  0.12    0.02
0       406.30  0.29    0.01
0       442.40  0.21    0.01
0       580.50  0.00    0.01
0       412.60  0.45    0.01
0       687.00  0.01    0.12
0       480.30  0.40    0.08
0       586.30  0.20    0.03
0       651.90  0.14    0.09
```

<span data-ttu-id="13e4e-251">Herzlichen Glückwunsch!</span><span class="sxs-lookup"><span data-stu-id="13e4e-251">Congratulations!</span></span> <span data-ttu-id="13e4e-252">Sie haben jetzt erfolgreich Machine Learning-Modelle zur Erkennung von Spitzen und Änderungspunktanomalien in Vertriebsdaten erstellt.</span><span class="sxs-lookup"><span data-stu-id="13e4e-252">You've now successfully built machine learning models for detecting spikes and change point anomalies in sales data.</span></span>

<span data-ttu-id="13e4e-253">Sie finden den Quellcode für dieses Tutorial im Repository [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/ProductSalesAnomalyDetection).</span><span class="sxs-lookup"><span data-stu-id="13e4e-253">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/ProductSalesAnomalyDetection) repository.</span></span>

<span data-ttu-id="13e4e-254">In diesem Tutorial haben Sie gelernt, wie die folgenden Aufgaben ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="13e4e-254">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="13e4e-255">Laden der Daten</span><span class="sxs-lookup"><span data-stu-id="13e4e-255">Load the data</span></span>
> * <span data-ttu-id="13e4e-256">Trainieren des Modells zur Erkennung von Anomaliespitzen</span><span class="sxs-lookup"><span data-stu-id="13e4e-256">Train the model for spike anomaly detection</span></span>
> * <span data-ttu-id="13e4e-257">Erkennen von Anomaliespitzen mit dem trainierten Modell</span><span class="sxs-lookup"><span data-stu-id="13e4e-257">Detect spike anomalies with the trained model</span></span>
> * <span data-ttu-id="13e4e-258">Trainieren des Modells zur Erkennung einer Änderungspunktanomalie</span><span class="sxs-lookup"><span data-stu-id="13e4e-258">Train the model for change point anomaly detection</span></span>
> * <span data-ttu-id="13e4e-259">Erkennen von Änderungspunktanomalien mit dem trainierten Modell</span><span class="sxs-lookup"><span data-stu-id="13e4e-259">Detect change point anomalies with the trained mode</span></span>

## <a name="next-steps"></a><span data-ttu-id="13e4e-260">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="13e4e-260">Next steps</span></span>

<span data-ttu-id="13e4e-261">Durchsuchen Sie das GitHub-Repository für Machine Learning-Beispiele nach einem Beispiel für die Erkennung von Stromverbrauchsanomalien, damit Sie es untersuchen können.</span><span class="sxs-lookup"><span data-stu-id="13e4e-261">Check out the Machine Learning samples GitHub repository to explore a Power Consumption Anomaly Detection sample.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="13e4e-262">dotnet/machinelearning-samples-GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="13e4e-262">dotnet/machinelearning-samples GitHub repository</span></span>](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/getting-started/TimeSeries_PowerAnomalyDetection)