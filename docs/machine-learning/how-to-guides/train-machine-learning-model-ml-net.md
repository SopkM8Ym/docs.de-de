---
title: Trainieren und Auswerten eines Modells
description: Erfahren Sie, wie Sie ein Machine Learning-Modell in ML.NET trainieren und auswerten.
ms.date: 05/03/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc, how-to
ms.openlocfilehash: 2abb17aad6091cd6a5f0b6f82835011d01b40153
ms.sourcegitcommit: ca2ca60e6f5ea327f164be7ce26d9599e0f85fe4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65063637"
---
# <a name="train-and-evaluate-a-model"></a><span data-ttu-id="7938c-103">Trainieren und Auswerten eines Modells</span><span class="sxs-lookup"><span data-stu-id="7938c-103">Train and evaluate a model</span></span>

<span data-ttu-id="7938c-104">Erfahren Sie, wie Sie mit ML.NET Machine Learning-Modelle erstellen, erlernte Parameter extrahieren und die Leistung messen können.</span><span class="sxs-lookup"><span data-stu-id="7938c-104">Learn how to build machine learning models, extract learned parameters and measure performance with ML.NET.</span></span> <span data-ttu-id="7938c-105">Obwohl dieses Beispiel ein Regressionsmodell trainiert, sind die Konzepte für einen Großteil der anderen Algorithmen anwendbar.</span><span class="sxs-lookup"><span data-stu-id="7938c-105">Although this sample trains a regression model, the concepts are applicable throughout a majority of the other algorithms.</span></span>

## <a name="split-data-for-training-and-testing"></a><span data-ttu-id="7938c-106">Aufteilen von Daten für Training und Tests</span><span class="sxs-lookup"><span data-stu-id="7938c-106">Split data for training and testing</span></span>

<span data-ttu-id="7938c-107">Mit einem Machine Learning-Modell sollen Muster innerhalb von Trainingsdaten erkannt werden.</span><span class="sxs-lookup"><span data-stu-id="7938c-107">The goal of a machine learning model is to identify patterns within training data.</span></span> <span data-ttu-id="7938c-108">Diese Muster werden zum Treffen von Vorhersagen mit neuen Daten verwendet.</span><span class="sxs-lookup"><span data-stu-id="7938c-108">These patterns are used to make predictions using new data.</span></span>

<span data-ttu-id="7938c-109">Betrachten Sie das folgende Datenmodell:</span><span class="sxs-lookup"><span data-stu-id="7938c-109">Given the following data model:</span></span>

```csharp
public class HousingData
{
    [LoadColumn(0)]
    public float Size { get; set; }

    [LoadColumn(1, 3)]
    [VectorType(3)]
    public float[] HistoricalPrices { get; set; }

    [LoadColumn(4)]
    [ColumnName("Label")]
    public float CurrentPrice { get; set; }
}
```

<span data-ttu-id="7938c-110">Laden Sie die Daten in eine [`IDataView`](xref:Microsoft.ML.IDataView):</span><span class="sxs-lookup"><span data-stu-id="7938c-110">Load the data into an [`IDataView`](xref:Microsoft.ML.IDataView):</span></span>

```csharp
HousingData[] housingData = new HousingData[]
{
    new HousingData
    {
        Size = 600f,
        HistoricalPrices = new float[] { 100000f ,125000f ,122000f },
        CurrentPrice = 170000f
    },
    new HousingData
    {
        Size = 1000f,
        HistoricalPrices = new float[] { 200000f, 250000f, 230000f },
        CurrentPrice = 225000f
    },
    new HousingData
    {
        Size = 1000f,
        HistoricalPrices = new float[] { 126000f, 130000f, 200000f },
        CurrentPrice = 195000f
    },
    new HousingData
    {
        Size = 850f,
        HistoricalPrices = new float[] { 150000f,175000f,210000f },
        CurrentPrice = 205000f
    },
    new HousingData
    {
        Size = 900f,
        HistoricalPrices = new float[] { 155000f, 190000f, 220000f },
        CurrentPrice = 210000f
    },
    new HousingData
    {
        Size = 550f,
        HistoricalPrices = new float[] { 99000f, 98000f, 130000f },
        CurrentPrice = 180000f
    }
};
```

<span data-ttu-id="7938c-111">Verwenden Sie die [`TrainTestSplit`](xref:Microsoft.ML.DataOperationsCatalog.TrainTestSplit*)-Methode, um die Daten in Trainings- und Testsätze aufzuteilen.</span><span class="sxs-lookup"><span data-stu-id="7938c-111">Use the [`TrainTestSplit`](xref:Microsoft.ML.DataOperationsCatalog.TrainTestSplit*) method to split the data into train and test sets.</span></span> <span data-ttu-id="7938c-112">Das Ergebnis ist ein [`TrainTestData`](xref:Microsoft.ML.DataOperationsCatalog.TrainTestData)-Objekt, das zwei [`IDataView`](xref:Microsoft.ML.IDataView)-Member enthält, eines für den Trainings- und das andere für den Testsatz.</span><span class="sxs-lookup"><span data-stu-id="7938c-112">The result will be a [`TrainTestData`](xref:Microsoft.ML.DataOperationsCatalog.TrainTestData) object which contains two [`IDataView`](xref:Microsoft.ML.IDataView) members, one for the train set and the other for the test set.</span></span> <span data-ttu-id="7938c-113">Der Prozentsatz der Datenaufteilung wird durch den `testFraction`-Parameter bestimmt.</span><span class="sxs-lookup"><span data-stu-id="7938c-113">The data split percentage is determined by the `testFraction` parameter.</span></span> <span data-ttu-id="7938c-114">Der folgende Ausschnitt enthält 20 Prozent der Originaldaten für den Testsatz.</span><span class="sxs-lookup"><span data-stu-id="7938c-114">The snippet below is holding out 20 percent of the original data for the test set.</span></span>

```csharp
DataOperationsCatalog.TrainTestData dataSplit = mlContext.Data.TrainTestSplit(data, testFraction: 0.2);
IDataView trainData = dataSplit.TrainSet;
IDataView testData = dataSplit.TestSet;
```

## <a name="prepare-the-data"></a><span data-ttu-id="7938c-115">Vorbereiten der Daten</span><span class="sxs-lookup"><span data-stu-id="7938c-115">Prepare the data</span></span>

<span data-ttu-id="7938c-116">Die Daten müssen vor dem Training eines Machine Learning-Modells vorverarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="7938c-116">The data needs to be pre-processed before training a machine learning model.</span></span> <span data-ttu-id="7938c-117">Weitere Informationen zur Datenaufbereitung finden Sie im [Anleitungsartikel für die Datenaufbereitung](prepare-data-ml-net.md) sowie in [`transforms page`](../resources/transforms.md).</span><span class="sxs-lookup"><span data-stu-id="7938c-117">More information on data preparation can be found on the [data prep how-to article](prepare-data-ml-net.md) as well as the [`transforms page`](../resources/transforms.md).</span></span>

<span data-ttu-id="7938c-118">Bei ML.NET-Algorithmen gibt es Einschränkungen hinsichtlich der Eingabespaltentypen.</span><span class="sxs-lookup"><span data-stu-id="7938c-118">ML.NET algorithms have constraints on input column types.</span></span> <span data-ttu-id="7938c-119">Außerdem werden für Ein- und Ausgabespaltennamen Standardwerte verwendet, wenn keine Werte angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="7938c-119">Additionally, default values are used for input and output column names when no values are specified.</span></span>

### <a name="working-with-expected-column-types"></a><span data-ttu-id="7938c-120">Arbeiten mit erwarteten Spaltentypen</span><span class="sxs-lookup"><span data-stu-id="7938c-120">Working with expected column types</span></span>

<span data-ttu-id="7938c-121">Die Machine Learning-Algorithmen in ML.NET erwarten als Eingabe einen Float-Vektor mit bekannter Größe.</span><span class="sxs-lookup"><span data-stu-id="7938c-121">The machine learning algorithms in ML.NET expect a float vector of known size as input.</span></span> <span data-ttu-id="7938c-122">Wenden Sie das [`VectorType`](xref:Microsoft.ML.Data.VectorTypeAttribute)-Attribut auf Ihr Datenmodell an, wenn alle Daten bereits im numerischen Format vorliegen und gemeinsam verarbeitet werden sollen (z.B. Bildpunkte).</span><span class="sxs-lookup"><span data-stu-id="7938c-122">Apply the [`VectorType`](xref:Microsoft.ML.Data.VectorTypeAttribute) attribute to your data model when all of the data is already in numerical format and is intended to be processed together (i.e. image pixels).</span></span> 

<span data-ttu-id="7938c-123">Wenn die Daten nicht alle numerisch sind und Sie unterschiedliche Datentransformationen auf jede der Spalten einzeln anwenden möchten, verwenden Sie die [`Concatenate`](xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate*)-Methode, nachdem alle Spalten verarbeitet wurden, um alle einzelnen Spalten zu einem einzigen Featurevektor zu kombinieren, der an eine neue Spalte ausgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="7938c-123">If data is not all numerical and you want to apply different data transformations on each of the columns individually, use the [`Concatenate`](xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate*) method after all of the columns have been processed to combine all of the individual columns into a single feature vector that is output to a new column.</span></span> 

<span data-ttu-id="7938c-124">Das folgende Ausschnitt kombiniert die Spalten `Size` und `HistoricalPrices` zu einem einzigen Featurevektor, der in eine neue Spalte namens `Features` ausgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="7938c-124">The following snippet combines the `Size` and `HistoricalPrices` columns into a single feature vector that is output to a new column called `Features`.</span></span> <span data-ttu-id="7938c-125">Da es einen Unterschied in den Skalierungen gibt, wird [`NormalizeMinMax`](xref:Microsoft.ML.NormalizationCatalog.NormalizeMinMax*) auf die Spalte `Features` angewendet, um die Daten zu normalisieren.</span><span class="sxs-lookup"><span data-stu-id="7938c-125">Because there is a difference in scales, [`NormalizeMinMax`](xref:Microsoft.ML.NormalizationCatalog.NormalizeMinMax*) is applied to the `Features` column to normalize the data.</span></span>

```csharp
// Define Data Prep Estimator
// 1. Concatenate Size and Historical into a single feature vector output to a new column called Features
// 2. Normalize Features vector
IEstimator<ITransformer> dataPrepEstimator =
    mlContext.Transforms.Concatenate("Features", "Size", "HistoricalPrices")
        .Append(mlContext.Transforms.NormalizeMinMax("Features"));

// Create data prep transformer
ITransformer dataPrepTransformer = dataPrepEstimator.Fit(trainData);

// Apply tranforms to training data
IDataView transformedTrainingData = dataPrepTransformer.Transform(trainData);
```

### <a name="working-with-default-column-names"></a><span data-ttu-id="7938c-126">Arbeiten mit standardmäßigen Spaltennamen</span><span class="sxs-lookup"><span data-stu-id="7938c-126">Working with default column names</span></span>

<span data-ttu-id="7938c-127">ML.NET-Algorithmen verwenden standardmäßige Spaltennamen, wenn keine Namen angegeben sind.</span><span class="sxs-lookup"><span data-stu-id="7938c-127">ML.NET algorithms use default column names when none are specified.</span></span> <span data-ttu-id="7938c-128">Alle Trainer haben einen Parameter namens `featureColumnName` für die Eingaben des Algorithmus und gegebenenfalls auch einen Parameter für den erwarteten Wert namens `labelColumnName`.</span><span class="sxs-lookup"><span data-stu-id="7938c-128">All trainers have a parameter called `featureColumnName` for the inputs of the algorithm and when applicable they also have a parameter for the expected value called `labelColumnName`.</span></span> <span data-ttu-id="7938c-129">Standardmäßig sind das die Werte `Features` bzw. `Label`.</span><span class="sxs-lookup"><span data-stu-id="7938c-129">By default those values are `Features` and `Label` respectively.</span></span> 

<span data-ttu-id="7938c-130">Durch die Verwendung der [`Concatenate`](xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate*)-Methode während der Vorverarbeitung zum Erstellen einer neuen Spalte mit dem Namen `Features` muss der Name der Featurespalte nicht in den Parametern des Algorithmus angegeben werden, da er bereits in der vorverarbeiteten `IDataView` vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="7938c-130">By using the [`Concatenate`](xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate*) method during pre-processing to create a new column called `Features`, there is no need to specify the feature column name in the parameters of the algorithm since it already exists in the pre-processed `IDataView`.</span></span> <span data-ttu-id="7938c-131">Die Bezeichnungsspalte ist `CurrentPrice`, aber da das [`ColumnName`](xref:Microsoft.ML.Data.ColumnNameAttribute)-Attribut im Datenmodell verwendet wird, benennt ML.NET die `CurrentPrice`-Spalte in `Label` um, sodass der `labelColumnName`-Parameter dem Estimator des Machine Learning-Algorithmus nicht mehr bereitgestellt werden muss.</span><span class="sxs-lookup"><span data-stu-id="7938c-131">The label column is `CurrentPrice`, but since the [`ColumnName`](xref:Microsoft.ML.Data.ColumnNameAttribute) attribute is used in the data model, ML.NET renames the `CurrentPrice` column to `Label` which removes the need to provide the `labelColumnName` parameter to the machine learning algorithm estimator.</span></span> 

<span data-ttu-id="7938c-132">Wenn Sie die standardmäßigen Spaltennamen nicht verwenden möchten, übergeben Sie die Namen der Feature- und Bezeichnungsspalten als Parameter bei der Definition des Estimators des Maschine Learning-Algorithmus, wie im nachfolgenden Ausschnitt gezeigt:</span><span class="sxs-lookup"><span data-stu-id="7938c-132">If you don't want to use the default column names, pass in the names of the feature and label columns as parameters when defining the machine learning algorithm estimator as demonstrated by the subsequent snippet:</span></span>

```csharp
var UserDefinedColumnSdcaEstimator = mlContext.Regression.Trainers.Sdca(labelColumnName: "MyLabelColumnName", featureColumnName: "MyFeatureColumnName");
```

## <a name="train-the-machine-learning-model"></a><span data-ttu-id="7938c-133">Trainieren des Machine Learning-Modells</span><span class="sxs-lookup"><span data-stu-id="7938c-133">Train the machine learning model</span></span>

<span data-ttu-id="7938c-134">Sobald die Daten vorverarbeitet sind, verwenden Sie die [`Fit`](xref:Microsoft.ML.Trainers.TrainerEstimatorBase`2.Fit*)-Methode, um das Machine Learning-Modell mit dem [`StochasticDualCoordinateAscent`](xref:Microsoft.ML.Trainers.SdcaRegressionTrainer)-Regressionsalgorithmus zu trainieren.</span><span class="sxs-lookup"><span data-stu-id="7938c-134">Once the data is pre-processed, use the [`Fit`](xref:Microsoft.ML.Trainers.TrainerEstimatorBase`2.Fit*) method to train the machine learning model with the [`StochasticDualCoordinateAscent`](xref:Microsoft.ML.Trainers.SdcaRegressionTrainer) regression algorithm.</span></span>

```csharp
// Define StochasticDualCoodrinateAscent regression algorithm estimator
var sdcaEstimator = mlContext.Regression.Trainers.Sdca();

// Build machine learning model
var trainedModel = sdcaEstimator.Fit(transformedTrainingData);
```

## <a name="extract-model-parameters"></a><span data-ttu-id="7938c-135">Extrahieren von Modellparametern</span><span class="sxs-lookup"><span data-stu-id="7938c-135">Extract model parameters</span></span>

<span data-ttu-id="7938c-136">Nachdem das Modell trainiert wurde, extrahieren Sie die gelernten [`ModelParameters`](xref:Microsoft.ML.Trainers.ModelParametersBase`1) zur Überprüfung oder für das erneute Training.</span><span class="sxs-lookup"><span data-stu-id="7938c-136">After the model has been trained, extract the learned [`ModelParameters`](xref:Microsoft.ML.Trainers.ModelParametersBase`1) for inspection or re-training.</span></span> <span data-ttu-id="7938c-137">Die [`LinearRegressionModelParameters`](xref:Microsoft.ML.Trainers.LinearRegressionModelParameters) stellen den den Trend und die erlernten Koeffizienten oder Gewichtungen des trainierten Modells bereit.</span><span class="sxs-lookup"><span data-stu-id="7938c-137">The [`LinearRegressionModelParameters`](xref:Microsoft.ML.Trainers.LinearRegressionModelParameters) provide the bias and learned coefficients or weights of the trained model.</span></span> 

```csharp
var trainedModelParameters = trainedModel.Model as LinearRegressionModelParameters;
```

> [!NOTE]
> <span data-ttu-id="7938c-138">Andere Modelle haben für ihre Aufgaben spezifische Parameter.</span><span class="sxs-lookup"><span data-stu-id="7938c-138">Other models have parameters that are specific to their tasks.</span></span> <span data-ttu-id="7938c-139">So stellt beispielsweise der [K-Means-Algorithmus](xref:Microsoft.ML.Trainers.KMeansTrainer) Daten in einen Cluster basierend auf Schwerpunkten bereit, und der [`KMeansModelParameters`](xref:Microsoft.ML.Trainers.KMeansModelParameters) enthält eine Eigenschaft, die diese erlernten Schwerpunkte speichert.</span><span class="sxs-lookup"><span data-stu-id="7938c-139">For example, the [K-Means algorithm](xref:Microsoft.ML.Trainers.KMeansTrainer) puts data into cluster based on centroids and the [`KMeansModelParameters`](xref:Microsoft.ML.Trainers.KMeansModelParameters) contains a property that stores these learned centroids.</span></span> <span data-ttu-id="7938c-140">Um mehr zu erfahren, lesen Sie die [`Microsoft.ML.Trainers`-API-Dokumentation](xref:Microsoft.ML.Trainers), und suchen Sie nach Klassen, die `ModelParameters` in ihrem Namen enthalten.</span><span class="sxs-lookup"><span data-stu-id="7938c-140">To learn more, visit the [`Microsoft.ML.Trainers` API Documentation](xref:Microsoft.ML.Trainers) and look for classes that contain `ModelParameters` in their name.</span></span> 

## <a name="evaluate-model-quality"></a><span data-ttu-id="7938c-141">Bewerten der Modellqualität</span><span class="sxs-lookup"><span data-stu-id="7938c-141">Evaluate model quality</span></span>

<span data-ttu-id="7938c-142">Um die Auswahl des leistungsstärksten Modells zu erleichtern, muss die Leistung anhand von Testdaten bewertet werden.</span><span class="sxs-lookup"><span data-stu-id="7938c-142">To help choose the best performing model, it is essential to evaluate its performance on test data.</span></span> <span data-ttu-id="7938c-143">Verwenden Sie die [`Evaluate`](xref:Microsoft.ML.RegressionCatalog.Evaluate*)-Methode, um verschiedene Metriken für das trainierte Modell zu messen.</span><span class="sxs-lookup"><span data-stu-id="7938c-143">Use the [`Evaluate`](xref:Microsoft.ML.RegressionCatalog.Evaluate*) method, to measure various metrics for the trained model.</span></span>

> [!NOTE]
> <span data-ttu-id="7938c-144">Die `Evaluate`-Methode liefert je nachdem, welche Machine Learning-Aufgabe durchgeführt wurde, unterschiedliche Metriken.</span><span class="sxs-lookup"><span data-stu-id="7938c-144">The `Evaluate` method produces different metrics depending on which machine learning task was was performed.</span></span> <span data-ttu-id="7938c-145">Um mehr zu erfahren, lesen Sie die [`Microsoft.ML.Data`-API-Dokumentation](xref:Microsoft.ML.Data), und suchen Sie nach Klassen, die `Metrics` in ihrem Namen enthalten.</span><span class="sxs-lookup"><span data-stu-id="7938c-145">For more details, visit the [`Microsoft.ML.Data` API Documentation](xref:Microsoft.ML.Data) and look for classes that contain `Metrics` in their name.</span></span> 

```csharp
// Measure trained model performance
// Apply data prep transformer to test data
IDataView transformedTestData = dataPrepTransformer.Transform(testData);

// Use trained model to make inferences on test data
IDataView testDataPredictions = trainedModel.Transform(transformedTestData);

// Extract model metrics and get RSquared
RegressionMetrics trainedModelMetrics = mlContext.Regression.Evaluate(testDataPredictions);
double rSquared = trainedModelMetrics.RSquared;
```

<span data-ttu-id="7938c-146">Im vorherigen Codebeispiel:</span><span class="sxs-lookup"><span data-stu-id="7938c-146">In the previous code sample:</span></span>  
1. <span data-ttu-id="7938c-147">Das Testdataset wird unter Verwendung der zuvor definierten Datenaufbereitungstransformationen vorverarbeitet.</span><span class="sxs-lookup"><span data-stu-id="7938c-147">Test data set is pre-processed using the data preparation transforms previously defined.</span></span> 
2. <span data-ttu-id="7938c-148">Das trainierte Machine Learning-Modell wird verwendet, um Vorhersagen zu den Testdaten zu treffen.</span><span class="sxs-lookup"><span data-stu-id="7938c-148">The trained machine learning model is used to make predictions on the test data.</span></span>
3. <span data-ttu-id="7938c-149">Bei der `Evaluate`-Methode werden die Werte in der Spalte `CurrentPrice` des Testdatasets mit der Spalte `Score` der neu ausgegebenen Vorhersagen verglichen, um die Metriken für das Regressionsmodell zu berechnen, von dem das Bestimmtheitsmaß in der `rSquared`-Variablen gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="7938c-149">In the `Evaluate` method, the values in the `CurrentPrice` column of the test data set are compared against the `Score` column of the newly output predictions to calculate the metrics for the regression model, one of which, R-Squared is stored in the `rSquared` variable.</span></span>

> [!NOTE]
> <span data-ttu-id="7938c-150">In diesem kleinen Beispiel ist das Bestimmtheitsmaß eine Zahl, die aufgrund der begrenzten Größe der Daten nicht im Bereich von 0-1 liegt.</span><span class="sxs-lookup"><span data-stu-id="7938c-150">In this small example, the R-Squared is a number not in the range of 0-1 because of the limited size of the data.</span></span> <span data-ttu-id="7938c-151">In einem realen Szenario sollten Sie mit einem Wert zwischen 0 und 1 rechnen.</span><span class="sxs-lookup"><span data-stu-id="7938c-151">In a real-world scenario, you should expect to see a value between 0 and 1.</span></span>