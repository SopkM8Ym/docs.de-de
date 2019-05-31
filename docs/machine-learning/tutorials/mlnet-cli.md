---
title: Automatisches Generieren eines binären Klassifizierers mit der ML.NET-CLI
description: Automatisches Generieren eines ML-Modells und des zugehörigen C#-Codes aus einem Beispieldataset
author: cesardl
ms.author: cesardl
ms.date: 04/24/2019
ms.custom: mvc
ms.topic: tutorial
ms.openlocfilehash: d7c4c774667e87fee2f71046aa0f91bfad7c6f3e
ms.sourcegitcommit: ca2ca60e6f5ea327f164be7ce26d9599e0f85fe4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65065943"
---
# <a name="auto-generate-a-binary-classifier-using-the-cli"></a><span data-ttu-id="d4eff-103">Automatisches Generieren eines binären Klassifizierers mit der CLI</span><span class="sxs-lookup"><span data-stu-id="d4eff-103">Auto generate a binary classifier using the CLI</span></span>

<span data-ttu-id="d4eff-104">Erfahren Sie, wie Sie mit der ML.NET-CLI automatisch ein ML.NET-Modell und den zugrunde liegenden C#-Code generieren.</span><span class="sxs-lookup"><span data-stu-id="d4eff-104">Learn how to use ML.NET CLI to automatically generate an ML.NET model and underlying C# code.</span></span> <span data-ttu-id="d4eff-105">Sie stellen Ihr Dataset und die Machine Learning-Aufgabe bereit, die Sie implementieren möchten, und die CLI verwendet die AutoML-Engine, um Quellcode für die Modellgenerierung und Bereitstellung sowie das Binärmodell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d4eff-105">You provide your dataset and the machine learning task you want to implement, and the CLI uses the AutoML engine to create model generation and deployment source code, as well as the binary model.</span></span>

<span data-ttu-id="d4eff-106">In diesem Tutorial führen Sie die folgenden Schritte durch:</span><span class="sxs-lookup"><span data-stu-id="d4eff-106">In this tutorial, you will do the following steps:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="d4eff-107">Vorbereiten der Daten für die ausgewählte Machine Learning-Aufgabe</span><span class="sxs-lookup"><span data-stu-id="d4eff-107">Prepare your data for the selected machine learning task</span></span>
> * <span data-ttu-id="d4eff-108">Ausführen des Befehls „mlnet auto-train“ über die CLI</span><span class="sxs-lookup"><span data-stu-id="d4eff-108">Run the 'mlnet auto-train' command from the CLI</span></span>
> * <span data-ttu-id="d4eff-109">Überprüfen der Ergebnisse der Qualitätsmetriken</span><span class="sxs-lookup"><span data-stu-id="d4eff-109">Review the quality metric results</span></span>
> * <span data-ttu-id="d4eff-110">Verstehen des generierten C#-Codes zur Verwendung des Modells in Ihrer Anwendung</span><span class="sxs-lookup"><span data-stu-id="d4eff-110">Understand the generated C# code to use the model in your application</span></span>
> * <span data-ttu-id="d4eff-111">Untersuchen des generierten C#-Codes, der zum Trainieren des Modells verwendet wurde</span><span class="sxs-lookup"><span data-stu-id="d4eff-111">Explore the generated C# code that was used to train the model</span></span>

> [!NOTE]
> <span data-ttu-id="d4eff-112">Dieses Thema bezieht sich auf das ML.NET-CLI-Tools, was derzeit als Vorschau verfügbar ist, und das Material kann jederzeit geändert werden.</span><span class="sxs-lookup"><span data-stu-id="d4eff-112">This topic refers to the ML.NET CLI tool, which is currently in Preview, and material may be subject to change.</span></span> <span data-ttu-id="d4eff-113">Weitere Informationen finden Sie in [der ML.NET-Einführung](https://www.microsoft.com/net/learn/apps/machine-learning-and-ai/ml-dotnet).</span><span class="sxs-lookup"><span data-stu-id="d4eff-113">For more information, visit [the ML.NET introduction](https://www.microsoft.com/net/learn/apps/machine-learning-and-ai/ml-dotnet).</span></span>

<span data-ttu-id="d4eff-114">Die ML.NET-CLI ist Teil von ML.NET und das Hauptziel ist es, ML.NET für .NET-Entwickler beim Erlernen von ML.NET zu „demokratisieren“, damit Sie den Code nicht von Grund auf neu schreiben müssen, um loszulegen.</span><span class="sxs-lookup"><span data-stu-id="d4eff-114">The ML.NET CLI is part of ML.NET and its main goal is to "democratize" ML.NET for .NET developers when learning ML.NET so you don't need to code from scratch to get started.</span></span>

<span data-ttu-id="d4eff-115">Sie können die ML.NET-CLI über jede Eingabeaufforderung (Windows, Mac oder Linux) ausführen, um qualitativ hochwertige ML.NET-Modelle und Quellcode basierend auf den von Ihnen bereitgestellten Trainingsdatasets zu generieren.</span><span class="sxs-lookup"><span data-stu-id="d4eff-115">You can run the ML.NET CLI on any command-prompt (Windows, Mac, or Linux) to generate good quality ML.NET models and source code based on training datasets you provide.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="d4eff-116">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="d4eff-116">Pre-requisites</span></span>

- <span data-ttu-id="d4eff-117">[.NET Core 2.2 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.2) oder höher</span><span class="sxs-lookup"><span data-stu-id="d4eff-117">[.NET Core 2.2 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.2) or later</span></span>
- <span data-ttu-id="d4eff-118">(Optional) [Visual Studio 2017 oder 2019](https://visualstudio.microsoft.com/vs/)</span><span class="sxs-lookup"><span data-stu-id="d4eff-118">(Optional) [Visual Studio 2017 or 2019](https://visualstudio.microsoft.com/vs/)</span></span>
- [<span data-ttu-id="d4eff-119">ML.NET-CLI</span><span class="sxs-lookup"><span data-stu-id="d4eff-119">ML.NET CLI</span></span>](../how-to-guides/install-ml-net-cli.md)

<span data-ttu-id="d4eff-120">Sie können die generierten C#-Codeprojekte entweder über Visual Studio oder mit `dotnet run` ausführen (.NET Core-CLI).</span><span class="sxs-lookup"><span data-stu-id="d4eff-120">You can either run the generated C# code projects from Visual Studio or with `dotnet run` (.NET Core CLI).</span></span>

## <a name="prepare-your-data"></a><span data-ttu-id="d4eff-121">Vorbereiten Ihrer Daten</span><span class="sxs-lookup"><span data-stu-id="d4eff-121">Prepare your data</span></span>

<span data-ttu-id="d4eff-122">Wir werden ein vorhandenes Dataset verwenden, das für ein Szenario „Standpunktanalyse“ verwendet wird, bei dem es sich um eine Machine Learning-Aufgabe für die binäre Klassifizierung handelt.</span><span class="sxs-lookup"><span data-stu-id="d4eff-122">We are going to use an existing dataset used for a 'Sentiment Analysis' scenario, which is a binary classification machine learning task.</span></span> <span data-ttu-id="d4eff-123">Sie können Ihr eigenes Dataset auf ähnliche Weise verwenden, und das Modell und der Code werden für Sie generiert.</span><span class="sxs-lookup"><span data-stu-id="d4eff-123">You can use your own dataset in a similar way, and the model and code will be generated for you.</span></span>

1. <span data-ttu-id="d4eff-124">Laden Sie [die Dataset-ZIP-Datei „UCI Sentiment Labeled Sentences“ (siehe Zitate im folgenden Hinweis)](https://archive.ics.uci.edu/ml/machine-learning-databases/00331/sentiment%20labelled%20sentences.zip) herunter, und entzippen Sie sie in einem beliebigen Ordner Ihrer Wahl.</span><span class="sxs-lookup"><span data-stu-id="d4eff-124">Download [The UCI Sentiment Labeled Sentences dataset zip file (see citations in the following note)](https://archive.ics.uci.edu/ml/machine-learning-databases/00331/sentiment%20labelled%20sentences.zip), and unzip it on any folder you choose.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d4eff-125">Die in diesem Tutorial verwendeten Datasets stammen aus „From Group to Individual Labels using Deep Features“, Kotzias et al,.</span><span class="sxs-lookup"><span data-stu-id="d4eff-125">The datasets this tutorial uses a dataset from the 'From Group to Individual Labels using Deep Features', Kotzias et al,.</span></span> <span data-ttu-id="d4eff-126">KDD 2015, und werden im UCI Machine Learning Repository – Dua, D. und Karra Taniskidou, E. gehostet. (2017).</span><span class="sxs-lookup"><span data-stu-id="d4eff-126">KDD 2015, and hosted at the UCI Machine Learning Repository - Dua, D. and Karra Taniskidou, E. (2017).</span></span> <span data-ttu-id="d4eff-127">UCI Machine Learning Repository [http://archive.ics.uci.edu/ml].</span><span class="sxs-lookup"><span data-stu-id="d4eff-127">UCI Machine Learning Repository [http://archive.ics.uci.edu/ml].</span></span> <span data-ttu-id="d4eff-128">Irvine, CA: University of California, School of Information and Computer Science.</span><span class="sxs-lookup"><span data-stu-id="d4eff-128">Irvine, CA: University of California, School of Information and Computer Science.</span></span>

2. <span data-ttu-id="d4eff-129">Kopieren Sie die `yelp_labelled.txt`-Datei in einen vorher von Ihnen erstellten Ordner (wie z.B. `/cli-test`).</span><span class="sxs-lookup"><span data-stu-id="d4eff-129">Copy the `yelp_labelled.txt` file into any folder you previously created (such as `/cli-test`).</span></span>

3. <span data-ttu-id="d4eff-130">Öffnen Sie die von Ihnen bevorzugte Eingabeaufforderung, und wechseln Sie in den Ordner, in den Sie die Datasetdatei kopiert haben.</span><span class="sxs-lookup"><span data-stu-id="d4eff-130">Open your preferred command prompt and move to the folder where you copied the dataset file.</span></span> <span data-ttu-id="d4eff-131">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d4eff-131">For example:</span></span>

    ```console
    > cd /cli-test
    ```

    <span data-ttu-id="d4eff-132">Sie können die Datasetdatei `yelp_labelled.txt` mit einem beliebigen Text-Editor wie beispielsweise Visual Studio Code öffnen und durchsuchen.</span><span class="sxs-lookup"><span data-stu-id="d4eff-132">Using any text editor such as Visual Studio Code, you can open, and explore the `yelp_labelled.txt` dataset file.</span></span> <span data-ttu-id="d4eff-133">Sie sehen folgende Struktur:</span><span class="sxs-lookup"><span data-stu-id="d4eff-133">You can see that the structure is:</span></span>

    - <span data-ttu-id="d4eff-134">Die Datei hat keinen Header.</span><span class="sxs-lookup"><span data-stu-id="d4eff-134">The file has no header.</span></span> <span data-ttu-id="d4eff-135">Sie verwenden den Index der Spalte.</span><span class="sxs-lookup"><span data-stu-id="d4eff-135">You will use the column's index.</span></span>

    - <span data-ttu-id="d4eff-136">Es gibt nur zwei Spalten:</span><span class="sxs-lookup"><span data-stu-id="d4eff-136">There are just two columns:</span></span>

        | <span data-ttu-id="d4eff-137">Text (Spaltenindex 0)</span><span class="sxs-lookup"><span data-stu-id="d4eff-137">Text (Column index 0)</span></span> | <span data-ttu-id="d4eff-138">Bezeichnung (Spaltenindex 1)</span><span class="sxs-lookup"><span data-stu-id="d4eff-138">Label (Column index 1)</span></span>|
        |--------------------------|-------|
        | <span data-ttu-id="d4eff-139">Toll... Mir hat das Restaurant gefallen.</span><span class="sxs-lookup"><span data-stu-id="d4eff-139">Wow... Loved this place.</span></span> | <span data-ttu-id="d4eff-140">1</span><span class="sxs-lookup"><span data-stu-id="d4eff-140">1</span></span> |
        | <span data-ttu-id="d4eff-141">Die Kruste ist nicht gut.</span><span class="sxs-lookup"><span data-stu-id="d4eff-141">Crust is not good.</span></span> | <span data-ttu-id="d4eff-142">0</span><span class="sxs-lookup"><span data-stu-id="d4eff-142">0</span></span> |
        | <span data-ttu-id="d4eff-143">Sie hat nicht geschmeckt und die Textur wurde einfach unangenehm.</span><span class="sxs-lookup"><span data-stu-id="d4eff-143">Not tasty and the texture was just nasty.</span></span> | <span data-ttu-id="d4eff-144">0</span><span class="sxs-lookup"><span data-stu-id="d4eff-144">0</span></span> |
        | <span data-ttu-id="d4eff-145">... VIELE WEITERE TEXTZEILEN...</span><span class="sxs-lookup"><span data-stu-id="d4eff-145">...MANY MORE TEXT ROWS...</span></span> | <span data-ttu-id="d4eff-146">...(1 oder 0)...</span><span class="sxs-lookup"><span data-stu-id="d4eff-146">...(1 or 0)...</span></span> |

    <span data-ttu-id="d4eff-147">Stellen Sie sicher, dass Sie die Datasetdatei über den Editor schließen.</span><span class="sxs-lookup"><span data-stu-id="d4eff-147">Make sure you close the dataset file from the editor.</span></span>

    <span data-ttu-id="d4eff-148">Jetzt können Sie die CLI für das Szenario „Standpunktanalyse“ verwenden.</span><span class="sxs-lookup"><span data-stu-id="d4eff-148">Now, you are ready to start using the CLI for this 'Sentiment Analysis' scenario.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d4eff-149">Nach Abschluss dieses Tutorials können Sie auch eigene Datasets ausprobieren, sofern sie für die ML-Aufgaben geeignet sind, die derzeit von der ML.NET-CLI-Vorschau unterstützt werden, d.h. *„Binäre Klassifizierung“, „Multiklassenklassifizierung“ und „Regression“* ).</span><span class="sxs-lookup"><span data-stu-id="d4eff-149">After finishing this tutorial you can also try with your own datasets as long as they are ready to be used for any of the ML tasks currently supported by the ML.NET CLI Preview which are *'Binary Classification', 'Multi-class Classification' and 'Regression'*).</span></span>

## <a name="run-the-mlnet-auto-train-command"></a><span data-ttu-id="d4eff-150">Ausführen des Befehls „mlnet auto-train“</span><span class="sxs-lookup"><span data-stu-id="d4eff-150">Run the 'mlnet auto-train' command</span></span>

1. <span data-ttu-id="d4eff-151">Führen Sie den folgenden ML.NET CLI-Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="d4eff-151">Run the following ML.NET CLI command:</span></span>

    ```console
    > mlnet auto-train --task binary-classification --dataset "yelp_labelled.txt" --label-column-index 1 --has-header false --max-exploration-time 10
    ```

    <span data-ttu-id="d4eff-152">Dieser Befehl führt den **`mlnet auto-train`-Befehl** aus:</span><span class="sxs-lookup"><span data-stu-id="d4eff-152">This command runs the **`mlnet auto-train` command**:</span></span>
    - <span data-ttu-id="d4eff-153">für eine **ML-Aufgabe** des Typs **`binary-classification`**</span><span class="sxs-lookup"><span data-stu-id="d4eff-153">for an **ML task** of type **`binary-classification`**</span></span>
    - <span data-ttu-id="d4eff-154">verwendet die **Datasetdatei `yelp_labelled.txt`** als Trainings- und Testdataset (intern verwendet die CLI entweder eine Kreuzvalidierung oder teilt sie in zwei Datasets, eine für das Training und eine für den Test)</span><span class="sxs-lookup"><span data-stu-id="d4eff-154">uses the **dataset file `yelp_labelled.txt`** as training and testing dataset (internally the CLI will either use cross-validation or split it in two datasets, one for training and one for testing)</span></span>
    - <span data-ttu-id="d4eff-155">wenn die **Objekt-/Zielspalte**, die Sie vorhersagen möchten (allgemein **Bezeichnung** genannt), die **Spalte mit dem Index 1** ist (das ist die zweite Spalte, da der Index nullbasiert ist)</span><span class="sxs-lookup"><span data-stu-id="d4eff-155">where the **objective/target column** you want to predict (commonly called **'label'**) is the **column with index 1** (that is the second column, since the index is zero-based)</span></span>
    - <span data-ttu-id="d4eff-156">verwendet **keinen Dateiheader** mit Spaltennamen, da diese spezielle Datasetdatei keinen Header hat</span><span class="sxs-lookup"><span data-stu-id="d4eff-156">does **not use a file header** with column names since this particular dataset file doesn't have a header</span></span>
    - <span data-ttu-id="d4eff-157">die **angestrebte Explorationszeit** für das Experiment beträgt **10 Sekunden**</span><span class="sxs-lookup"><span data-stu-id="d4eff-157">the **targeted exploration time** for the experiment is **10 seconds**</span></span>

    <span data-ttu-id="d4eff-158">Die CLI-Ausgabe sieht in etwas so aus:</span><span class="sxs-lookup"><span data-stu-id="d4eff-158">You will see output from the CLI, similar to:</span></span>

    <!-- markdownlint-disable MD023 -->
    # <a name="windowstabwindows"></a>[<span data-ttu-id="d4eff-159">Windows</span><span class="sxs-lookup"><span data-stu-id="d4eff-159">Windows</span></span>](#tab/windows)

    ![Befehl „auto-train“ in der ML.NET-CLI auf PowerShell](./media/mlnet-cli/mlnet-auto-train-binary-classification-powershell.gif)

    # <a name="macos-bashtabmacosbash"></a>[<span data-ttu-id="d4eff-161">MacOS-Bash</span><span class="sxs-lookup"><span data-stu-id="d4eff-161">macOS Bash</span></span>](#tab/macosbash)

    ![Befehl „auto-train“ in der ML.NET-CLI auf PowerShell](./media/mlnet-cli/mlnet-auto-train-binary-classification-bash.gif)

    <span data-ttu-id="d4eff-163">In diesem speziellen Fall konnte das CLI-Tool in nur 10 Sekunden und mit dem bereitgestellten kleinen Dataset eine ganze Reihe von Iterationen durchführen. Das bedeutet, dass das Training basierend auf verschiedenen Kombinationen von Algorithmen/Konfigurationen mit unterschiedlichen internen Datentransformationen und den Hyperparametern des Algorithmus mehrfach durchgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="d4eff-163">In this particular case, in only 10 seconds and with the small dataset provided, the CLI tool was able to run quite a few iterations, meaning training multiple times based on different combinations of algorithms/configuration with different internal data transformations and algorithm's hyper-parameters.</span></span> 

    <span data-ttu-id="d4eff-164">Schließlich ist das „Modell mit der besten Qualität“, das in 10 Sekunden gefunden wurde, ein Modell, das einen bestimmten Trainer/Algorithmus mit einer bestimmten Konfiguration verwendet.</span><span class="sxs-lookup"><span data-stu-id="d4eff-164">Finally, the "best quality" model found in 10 seconds is a model using a particular trainer/algorithm with any specific configuration.</span></span> <span data-ttu-id="d4eff-165">Je nach Explorationszeit kann der Befehl ein anderes Ergebnis liefern.</span><span class="sxs-lookup"><span data-stu-id="d4eff-165">Depending on the exploration time, the command can produce a different result.</span></span> <span data-ttu-id="d4eff-166">Die Auswahl basiert auf den zahlreichen dargestellten Metriken, wie z.B. `Accuracy`.</span><span class="sxs-lookup"><span data-stu-id="d4eff-166">The selection is based on the multiple metrics shown, such as `Accuracy`.</span></span>

    <span data-ttu-id="d4eff-167">**Verstehen der Qualitätsmetriken des Modells**</span><span class="sxs-lookup"><span data-stu-id="d4eff-167">**Understanding the model's quality metrics**</span></span>

    <span data-ttu-id="d4eff-168">Die erste und einfachste Metrik zur Bewertung eines binären Klassifikationsmodells ist die Genauigkeit. Diese Metrik ist einfach zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="d4eff-168">The first and easiest metric to evaluate a binary-classification model is the accuracy, which is simple to understand.</span></span> <span data-ttu-id="d4eff-169">„Die Genauigkeit ist der Anteil der korrekten Vorhersagen mit einem Testdataset.“</span><span class="sxs-lookup"><span data-stu-id="d4eff-169">"Accuracy is the proportion of correct predictions with a test data set.".</span></span> <span data-ttu-id="d4eff-170">Je näher der Wert an 100 % (1,00) liegt, desto besser.</span><span class="sxs-lookup"><span data-stu-id="d4eff-170">The closer to 100% (1.00), the better.</span></span>

    <span data-ttu-id="d4eff-171">Es gibt jedoch Fälle, in denen das bloße Messen mit der Metrik „Accuracy“ nicht ausreicht, insbesondere wenn die Bezeichnung (in diesem Fall 0 und 1) im Testdataset unausgewogen ist.</span><span class="sxs-lookup"><span data-stu-id="d4eff-171">However, there are cases where just measuring with the Accuracy metric is not enough, especially when the label (0 and 1 in this case) is unbalanced in the test dataset.</span></span>

    <span data-ttu-id="d4eff-172">Weitere Metriken und **detailliertere Informationen über die Metriken,** wie z.B. „Accuracy“. „AUC“, „AUCPR“, „F1-score“, die zur Bewertung der verschiedenen Modelle verwendet werden, finden Sie in [Verstehen der ML.NET-Metriken](../resources/metrics.md).</span><span class="sxs-lookup"><span data-stu-id="d4eff-172">For additional metrics and more **detailed information about the metrics** such as Accuracy, AUC, AUCPR, F1-score used to evaluate the different models, you can read [Understanding ML.NET metrics](../resources/metrics.md)</span></span>

    > [!NOTE]
    >  <span data-ttu-id="d4eff-173">Sie können genau dieses Dataset ausprobieren und für `--max-exploration-time` ein paar Minuten angeben (z.B. für drei Minuten geben Sie 180 Sekunden an). Dadurch wird mit einer anderen Konfiguration der Trainingspipeline für dieses Dataset (das ziemlich klein ist, 1.000 Zeilen) ein besseres „bestes Modell“ für Sie ermittelt.</span><span class="sxs-lookup"><span data-stu-id="d4eff-173">You can try this very same dataset and specify a few minutes for `--max-exploration-time` (for instance three minutes so you specify 180 seconds) which will find a better "best model" for you with a different training pipeline configuration for this dataset (which is pretty small, 1000 rows).</span></span> 
        
    <span data-ttu-id="d4eff-174">Um für größere Datasets ein einsatzbereites Modell mit „bester/gute Qualität“ zu finden, sollten Sie mit der CLI experimentieren, wobei in der Regel je nach Größe des Datasets viel mehr Explorationszeit angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="d4eff-174">In order to find a "best/good quality" model that is a "production-ready model" targeting larger datasets, you should make experiments with the CLI usually specifying much more exploration time depending on the size of the dataset.</span></span> <span data-ttu-id="d4eff-175">In der Tat kann es in vielen Fällen vorkommen, dass Sie mehrere Stunden Explorationszeit benötigen, insbesondere wenn das Dataset viele Zeilen und Spalten enthält.</span><span class="sxs-lookup"><span data-stu-id="d4eff-175">In fact, in many cases you might require multiple hours of exploration time especially if the dataset is large on rows and columns.</span></span> 

1. <span data-ttu-id="d4eff-176">Durch die vorherige Befehlsausführung wurden die folgenden Objekte generiert:</span><span class="sxs-lookup"><span data-stu-id="d4eff-176">The previous command execution has generated the following assets:</span></span>

    - <span data-ttu-id="d4eff-177">Eine serialisierte ZIP-Datei des Modells („bestes Modell“), die sofort verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="d4eff-177">A serialized model .zip ("best model") ready to use.</span></span> 
    - <span data-ttu-id="d4eff-178">C#-Code, um das generierte Modell auszuführen/zu bewerten (um mit diesem Modell Vorhersagen in Ihren Endbenutzer-Apps zu treffen).</span><span class="sxs-lookup"><span data-stu-id="d4eff-178">C# code to run/score that generated model (To make predictions in your end-user apps with that model).</span></span>
    - <span data-ttu-id="d4eff-179">C#-Trainingscode, der zur Erstellung dieses Modells verwendet wird (zu Lernzwecken).</span><span class="sxs-lookup"><span data-stu-id="d4eff-179">C# training code used to generate that model (Learning purposes).</span></span>
    - <span data-ttu-id="d4eff-180">Eine Protokolldatei mit allen untersuchten Iterationen mit spezifischen Detailinformationen zu jedem Algorithmus, der mit seiner Kombination aus Hyperparametern und Datentransformationen getestet wurde.</span><span class="sxs-lookup"><span data-stu-id="d4eff-180">A log file with all the iterations explored having specific detailed information about each algorithm tried with its combination of hyper-parameters and data transformations.</span></span> 

    <span data-ttu-id="d4eff-181">Die ersten beiden Objekte (ZIP-Datei des Modells und der C#-Code zur Ausführung des Modells) können direkt in Ihren Endbenutzer-App (ASP.NET Core-Web-App, Dienste, Desktop-App usw.) verwendet werden, um mit dem generierten ML-Modell Vorhersagen zu treffen.</span><span class="sxs-lookup"><span data-stu-id="d4eff-181">The first two assets (.ZIP file model and C# code to run that model) can directly be used in your end-user apps (ASP.NET Core web app, services, desktop app, etc.) to make predictions with that generated ML model.</span></span>

    <span data-ttu-id="d4eff-182">Das dritte Objekt, der Trainingscode, zeigt Ihnen, welchen ML.NET-API-Code die CLI zum Trainieren des generierten Modells verwendet hat, sodass Sie untersuchen können, welchen spezifischen Trainer/Algorithmus und Hyperparameter die CLI ausgewählt hat.</span><span class="sxs-lookup"><span data-stu-id="d4eff-182">The third asset, the training code, shows you what ML.NET API code was used by the CLI to train the generated model, so you can investigate what specific trainer/algorithm and hyper-parameters were selected by the CLI.</span></span>

<span data-ttu-id="d4eff-183">Die aufgezählten Objekte werden in den folgenden Schritten des Tutorials erläutert.</span><span class="sxs-lookup"><span data-stu-id="d4eff-183">Those enumerated assets are explained in the following steps of the tutorial.</span></span>

## <a name="explore-the-generated-c-code-to-use-for-running-the-model-to-make-predictions"></a><span data-ttu-id="d4eff-184">Erkunden des generierten C#-Codes, der für die Ausführung des Modells zum Treffen von Vorhersagen verwendet werden kann</span><span class="sxs-lookup"><span data-stu-id="d4eff-184">Explore the generated C# code to use for running the model to make predictions</span></span>

1. <span data-ttu-id="d4eff-185">Öffnen Sie in Visual Studio (2017 oder 2019) die Projektmappe, die in dem Ordner mit dem Namen `SampleBinaryClassification` in Ihrem ursprünglichen Zielordner generiert wurde (im Tutorial wurde der Name `/cli-test` verwendet).</span><span class="sxs-lookup"><span data-stu-id="d4eff-185">In Visual Studio (2017 or 2019) open the solution generated in the folder named `SampleBinaryClassification` within your original destination folder (in the tutorial was named `/cli-test`).</span></span> <span data-ttu-id="d4eff-186">Die angezeigte Projektmappe sollte ungefähr so aussehen:</span><span class="sxs-lookup"><span data-stu-id="d4eff-186">You should see a solution similar to:</span></span>

    > [!NOTE]
    > <span data-ttu-id="d4eff-187">Im Tutorial empfehlen wir die Verwendung von Visual Studio. Sie können aber auch den generierten C#-Code (zwei Projekte) mit einem beliebigen Text-Editor untersuchen und die generierte Konsolen-App mit der `dotnet CLI` auf MacOS-, Linux- oder Windows-Computern ausführen.</span><span class="sxs-lookup"><span data-stu-id="d4eff-187">In the tutorial we suggest to use Visual Studio, but you can also explore the generated C# code (two projects) with any text editor and run the generated console app with the `dotnet CLI` on macOS, Linux or Windows machine.</span></span>

    ![Von der CLI generierte VS-Projektmappe](./media/mlnet-cli/generated-csharp-solution-detailed.png)

    - <span data-ttu-id="d4eff-189">Die generierte **Klassenbibliothek**, die das serialisierte ML-Modell und die Datenklassen enthält, können Sie direkt in Ihrer Endbenutzeranwendung verwenden, auch wenn Sie direkt auf diese Klassenbibliothek verweisen (oder den Code verschieben, ganz wie Sie möchten).</span><span class="sxs-lookup"><span data-stu-id="d4eff-189">The generated **class library** containing the serialized ML model and the data classes is something you can directly use in your end-user application, even by directly referencing that class library (or moving the code, as you prefer).</span></span>
    - <span data-ttu-id="d4eff-190">Die generierte **Konsolen-App** enthält Ausführungscode, den Sie überprüfen müssen. Dann wird der „Bewertungscode“ (Code, der das ML-Modell ausführt, um Vorhersagen zu treffen) in der Regel wiederverwendet, indem Sie diesen einfachen Code (nur ein paar Zeilen) in Ihre Endbenutzeranwendung verschieben, wo Sie die Vorhersagen machen möchten.</span><span class="sxs-lookup"><span data-stu-id="d4eff-190">The generated **console app** contains execution code that you must review and then you usually reuse the 'scoring code' (code that runs the ML model to make predictions) by moving that simple code (just a few lines) to your end-user application where you want to make the predictions.</span></span> 

1. <span data-ttu-id="d4eff-191">Öffnen Sie die Klassendateien **Observation.cs** und **Prediction.cs** im Klassenbibliotheksprojekt.</span><span class="sxs-lookup"><span data-stu-id="d4eff-191">Open the **Observation.cs** and **Prediction.cs** class files within the class library project.</span></span> <span data-ttu-id="d4eff-192">Sie sehen, dass diese Klassen „Datenklassen“ oder POCO-Klassen sind, die zum Speichern von Daten verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="d4eff-192">You will see that these classes are 'data classes' or POCO classes used to hold data.</span></span> <span data-ttu-id="d4eff-193">Es handelt sich um einen "Textbausteincode", dessen Generierung jedoch sinnvoll ist, wenn Ihr Dataset Dutzende oder sogar Hunderte von Spalten enthält.</span><span class="sxs-lookup"><span data-stu-id="d4eff-193">It is 'boilerplate code' but useful to have it generated if your dataset has tens or even hundreds of columns.</span></span> 
    - <span data-ttu-id="d4eff-194">Die `SampleObservation`-Klasse wird beim Auslesen von Daten aus dem Dataset verwendet.</span><span class="sxs-lookup"><span data-stu-id="d4eff-194">The `SampleObservation` class is used when reading data from the dataset.</span></span> 
    - <span data-ttu-id="d4eff-195">Die `SamplePrediction`-Klasse oder wenn</span><span class="sxs-lookup"><span data-stu-id="d4eff-195">The `SamplePrediction` class or when</span></span>

1. <span data-ttu-id="d4eff-196">Öffnen Sie die Datei „Program.cs“, und untersuchen Sie den Code.</span><span class="sxs-lookup"><span data-stu-id="d4eff-196">Open the Program.cs file and explore the code.</span></span> <span data-ttu-id="d4eff-197">Mit nur wenigen Zeilen können Sie das Modell ausführen und eine Beispielvorhersage treffen.</span><span class="sxs-lookup"><span data-stu-id="d4eff-197">In just a few lines, you are able to run the model and make a sample prediction.</span></span>

    ```csharp
    static void Main(string[] args)
    {
        MLContext mlContext = new MLContext();

        // Training code used by ML.NET CLI and AutoML to generate the model
        //ModelBuilder.CreateModel();

        ITransformer mlModel = mlContext.Model.Load(MODEL_FILEPATH, out DataViewSchema inputSchema);
        var predEngine = mlContext.Model.CreatePredictionEngine<SampleObservation, SamplePrediction>(mlModel);

        // Create sample data to do a single prediction with it 
        SampleObservation sampleData = CreateSingleDataSample(mlContext, DATA_FILEPATH);

        // Try a single prediction
        SamplePrediction predictionResult = predEngine.Predict(sampleData);

        Console.WriteLine($"Single Prediction --> Actual value: {sampleData.Label} | Predicted value: {predictionResult.Prediction}");
    }
    ```

- <span data-ttu-id="d4eff-198">Die erste Zeile des Codes erstellt einfach ein `MLContext`-Objekt, das bei der Ausführung von ML.NET-Code benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="d4eff-198">The first line of code simply creates an `MLContext` object needed whenever you run ML.NET code.</span></span> 

- <span data-ttu-id="d4eff-199">Die zweite Codezeile wird auskommentiert, da Sie das Modell nicht trainieren müssen, da es bereits vom CLI-Tool für Sie trainiert und in der serialisierten ZIP-Datei des Modells gespeichert wurde.</span><span class="sxs-lookup"><span data-stu-id="d4eff-199">The second line of code is commented because you don't need to train the model since it was already trained for you by the CLI tool and saved into the model's serialized .ZIP file.</span></span> <span data-ttu-id="d4eff-200">Wenn Sie jedoch sehen möchten, *„wie das Modell von der CLI trainiert wurde“* , können Sie die Auskommentierung dieser Zeile aufheben und den für dieses spezielle ML-Modell verwendeten Trainingscode ausführen/debuggen.</span><span class="sxs-lookup"><span data-stu-id="d4eff-200">But if you want to see *"how the model was trained"* by the CLI, you could uncomment that line and run/debug the training code used for that particular ML model.</span></span>

- <span data-ttu-id="d4eff-201">In der dritten Zeile des Codes laden Sie das Modell mit der `mlContext.Model.Load()`-API aus der serialisierten ZIP-Datei des Modells, indem Sie den Pfad zu dieser Datei angeben.</span><span class="sxs-lookup"><span data-stu-id="d4eff-201">In the third line of code, you load the model from the serialized model .ZIP file with the `mlContext.Model.Load()` API by providing the path to that model .ZIP file.</span></span>

- <span data-ttu-id="d4eff-202">In der vierten Codezeile, die Sie laden, erstellen Sie das `PredictionEngine`-Objekt mit der `mlContext.Model.CreatePredictionEngine<TObservation, TPrediction>()`-API.</span><span class="sxs-lookup"><span data-stu-id="d4eff-202">In the fourth line of code you load create the `PredictionEngine` object with the `mlContext.Model.CreatePredictionEngine<TObservation, TPrediction>()` API.</span></span> <span data-ttu-id="d4eff-203">Sie benötigen das `PredictionEngine`-Objekt, wenn Sie eine Vorhersage für ein einzelnes Datenbeispiel erstellen möchten (in diesem Fall ein einzelnes Textstück, um dessen Standpunkt vorherzusagen).</span><span class="sxs-lookup"><span data-stu-id="d4eff-203">You need the `PredictionEngine` object whenever you want to make a prediction targeting a single sample of data (In this case, a single piece of text to predict its sentiment).</span></span>

- <span data-ttu-id="d4eff-204">In der fünften Zeile des Codes erstellen Sie die *einzelnen Beispieldaten*, die für die Vorhersage verwendet werden sollen, indem Sie die Funktion `CreateSingleDataSample()` aufrufen.</span><span class="sxs-lookup"><span data-stu-id="d4eff-204">The fifth line of code is where you create that *single sample data* to be used for the prediction by calling the function `CreateSingleDataSample()`.</span></span> <span data-ttu-id="d4eff-205">Da das CLI-Tool nicht weiß, welche Art von Beispieldaten verwendet werden sollen, lädt es innerhalb dieser Funktion die erste Zeile des Datasets.</span><span class="sxs-lookup"><span data-stu-id="d4eff-205">Since the CLI tool doesn't know what kind of sample data to use, within that function it is loading the first row of the dataset.</span></span> <span data-ttu-id="d4eff-206">Für diesen Fall können Sie jedoch auch eigene „hartcodierten“ Daten anstelle der aktuellen Implementierung der Funktion `CreateSingleDataSample()` erstellen, indem Sie diesen einfacheren Code aktualisieren, der diese Funktion implementiert:</span><span class="sxs-lookup"><span data-stu-id="d4eff-206">However, for this case you can also create you own 'hard-coded' data instead of the current implementation of the `CreateSingleDataSample()` function by updating this simpler code implementing that function:</span></span>

    ```csharp
    private static SampleObservation CreateSingleDataSample()
    {
        SampleObservation sampleForPrediction = new SampleObservation() { Col0 = "The ML.NET CLI is great for getting started. Very cool!", Label = true };
        return sampleForPrediction;
    }
    ```

1. <span data-ttu-id="d4eff-207">Führen Sie das Projekt entweder mit den ursprünglichen Beispieldaten aus der ersten Zeile des Datasets oder mit Ihren eigenen, hartcodierten Beispieldaten aus.</span><span class="sxs-lookup"><span data-stu-id="d4eff-207">Run the project, either using the original sample data loaded from the first row of the dataset or by providing your own custom hard-coded sample data.</span></span> <span data-ttu-id="d4eff-208">Die Vorhersage sollte ungefähr so aussehen:</span><span class="sxs-lookup"><span data-stu-id="d4eff-208">You should get a prediction comparable to:</span></span>

    # <a name="windowstabwindows"></a>[<span data-ttu-id="d4eff-209">Windows</span><span class="sxs-lookup"><span data-stu-id="d4eff-209">Windows</span></span>](#tab/windows)

    <span data-ttu-id="d4eff-210">Führen Sie die Konsolen-App über Visual Studio durch Drücken auf F5 („Wiedergabe“) aus:</span><span class="sxs-lookup"><span data-stu-id="d4eff-210">Run the console app from Visual Studio by hitting F5 (Play button):</span></span>

    ![Befehl „auto-train“ in der ML.NET-CLI auf PowerShell](./media/mlnet-cli/sample-cli-prediction-execution.png)<span data-ttu-id="d4eff-212">)</span><span class="sxs-lookup"><span data-stu-id="d4eff-212">)</span></span>

    # <a name="macos-bashtabmacosbash"></a>[<span data-ttu-id="d4eff-213">MacOS-Bash</span><span class="sxs-lookup"><span data-stu-id="d4eff-213">macOS Bash</span></span>](#tab/macosbash)

    <span data-ttu-id="d4eff-214">Führen Sie die Konsolen-App über die Eingabeaufforderung aus, indem Sie die folgenden Befehle eingeben:</span><span class="sxs-lookup"><span data-stu-id="d4eff-214">Run the console app from the command-prompt by typing the following commands:</span></span>

     ```
     > cd SampleBinaryClassification
     > cd SampleBinaryClassification.ConsoleApp

     > dotnet run
     ```

    ![Befehl „auto-train“ in der ML.NET-CLI auf PowerShell](./media/mlnet-cli/sample-cli-prediction-execution-bash.png)<span data-ttu-id="d4eff-216">)</span><span class="sxs-lookup"><span data-stu-id="d4eff-216">)</span></span>

    ---

1. <span data-ttu-id="d4eff-217">Versuchen Sie, die hartcodierten Beispieldaten in andere Sätze mit verschiedenen Standpunkten zu ändern, und schauen Sie sich an, wie das Modell positive oder negative Standpunkte vorhersagt.</span><span class="sxs-lookup"><span data-stu-id="d4eff-217">Try changing the hard-coded sample data to other sentences with different sentiment and see how the model predicts positive or negative sentiment.</span></span> 

## <a name="infuse-your-end-user-applications-with-ml-model-predictions"></a><span data-ttu-id="d4eff-218">Ergänzen Ihrer Endbenutzeranwendungen mit ML-Modellvorhersagen</span><span class="sxs-lookup"><span data-stu-id="d4eff-218">Infuse your end-user applications with ML model predictions</span></span>

<span data-ttu-id="d4eff-219">Sie können einen ähnlichen „Bewertungscode für das ML-Modell“ verwenden, um das Modell in Ihrer Endbenutzeranwendung auszuführen und Vorhersagen zu treffen.</span><span class="sxs-lookup"><span data-stu-id="d4eff-219">You can use similar 'ML model scoring code' to run the model in your end-user application and make predictions.</span></span> 

<span data-ttu-id="d4eff-220">Beispielsweise können Sie diesen Code direkt in eine beliebige Windows-Desktop-Anwendung wie **WPP** und **WinForms** verschieben und das Modell auf die gleiche Weise ausführen wie in der Konsolen-App.</span><span class="sxs-lookup"><span data-stu-id="d4eff-220">For instance, you could directly move that code to any Windows desktop application such as **WPP** and **WinForms** and run the model in the same way than it was done in the console app.</span></span>

<span data-ttu-id="d4eff-221">Die Art und Weise, wie Sie diese Codezeilen zur Ausführung des ML-Modells implementieren, sollte jedoch optimiert werden (d.h. die ZIP-Datei des Modells zwischenspeichern und einmal laden) und Sie sollten Singleton-Objekte verwenden, anstatt sie bei jeder Anforderung zu erstellen, insbesondere wenn Ihre Anwendung skalierbar sein muss, wie beispielsweise eine Webanwendung oder ein verteilter Dienst, wie im folgenden Abschnitt erläutert.</span><span class="sxs-lookup"><span data-stu-id="d4eff-221">However, the way you implement those lines of code to run an ML model should be optimized (that is, cache the model .zip file and load it once) and have singleton objects instead of creating them on every request, especially if your application needs to be scalable such as a web application or distributed service, as explained in the following section.</span></span>

### <a name="running-mlnet-models-in-scalable-aspnet-core-web-apps-and-services-multi-threaded-apps"></a><span data-ttu-id="d4eff-222">Ausführen von ML.NET Modellen in skalierbaren ASP.NET Core-Web-Apps und -Diensten (Multithread-Apps)</span><span class="sxs-lookup"><span data-stu-id="d4eff-222">Running ML.NET models in scalable ASP.NET Core web apps and services (multi-threaded apps)</span></span>

<span data-ttu-id="d4eff-223">Die Erstellung des Modellobjekts (`ITransformer`, das aus der ZIP-Datei eines Modells geladen wurde) und des `PredictionEngine`-Objekts sollte optimiert werden, insbesondere bei der Ausführung auf skalierbaren Web-Apps und verteilten Diensten.</span><span class="sxs-lookup"><span data-stu-id="d4eff-223">The creation of the model object (`ITransformer` loaded from a model's .zip file) and the `PredictionEngine` object should be optimized especially when running on scalable web apps and distributed services.</span></span> <span data-ttu-id="d4eff-224">Für den ersten Fall, das Modellobjekt (`ITransformer`), ist die Optimierung einfach.</span><span class="sxs-lookup"><span data-stu-id="d4eff-224">For the first case, the model object (`ITransformer`) the optimization is straightforward.</span></span> <span data-ttu-id="d4eff-225">Da das `ITransformer`-Objekt threadsicher ist, können Sie das Objekt als Singleton-Objekt oder statisches Objekt zwischenspeichern und das Modell einmal laden.</span><span class="sxs-lookup"><span data-stu-id="d4eff-225">Since the `ITransformer` object is thread-safe, you can cache the object as a singleton or static object so you load the model once.</span></span>

<span data-ttu-id="d4eff-226">Für das zweite Objekt, das `PredictionEngine`-Objekt, ist es nicht so einfach, da das `PredictionEngine`-Objekt nicht threadsicher ist. Daher können Sie dieses Objekt nicht als Singleton- oder statisches Objekt in einer ASP.NET Core-App instanziieren.</span><span class="sxs-lookup"><span data-stu-id="d4eff-226">For the second object, the `PredictionEngine` object, it is not so easy because the `PredictionEngine` object is not thread-safe, therefore you cannot instantiate this object as singleton or static object in an ASP.NET Core app.</span></span> <span data-ttu-id="d4eff-227">Dieses Problem der Threadsicherheit und Skalierbarkeit wird in diesem [Blogbeitrag](https://devblogs.microsoft.com/cesardelatorre/how-to-optimize-and-run-ml-net-models-on-scalable-asp-net-core-webapis-or-web-apps/) ausführlich behandelt.</span><span class="sxs-lookup"><span data-stu-id="d4eff-227">This thread-safe and scalability problem is deeply discussed in this [Blog Post](https://devblogs.microsoft.com/cesardelatorre/how-to-optimize-and-run-ml-net-models-on-scalable-asp-net-core-webapis-or-web-apps/).</span></span> 

<span data-ttu-id="d4eff-228">Allerdings ist die Praxis für Sie heute wesentlich einfacher, als in diesem Blogbeitrag beschrieben.</span><span class="sxs-lookup"><span data-stu-id="d4eff-228">However, things got a lot easier for you than what's explained in that blog post.</span></span> <span data-ttu-id="d4eff-229">Wir haben für Sie einen einfacheren Ansatz erarbeitet und ein praktisches **.NET Core-Integrationspaket** erstellt, das Sie problemlos in Ihren ASP.NET Core-Apps und -Diensten verwenden können, indem Sie es in den DI-Diensten der Anwendung (Dependency Injection Services) registrieren und dann direkt über Ihren Code verwenden.</span><span class="sxs-lookup"><span data-stu-id="d4eff-229">We worked on a simpler approach for you and have created a nice **'.NET Core Integration Package'** that you can  easily use in your ASP.NET Core apps and services by registering it in the application DI services (Dependency Injection services) and then directly use it from your code.</span></span> <span data-ttu-id="d4eff-230">Schauen Sie sich dafür das folgende Tutorial und Beispiel an:</span><span class="sxs-lookup"><span data-stu-id="d4eff-230">Check the following tutorial and example for doing that:</span></span>

- [<span data-ttu-id="d4eff-231">Tutorial: Ausführen von ML.NET-Modellen auf skalierbaren ASP.NET Core-Web-Apps und -Web-APIs</span><span class="sxs-lookup"><span data-stu-id="d4eff-231">Tutorial: Running ML.NET models on scalable ASP.NET Core web apps and WebAPIs</span></span>](https://aka.ms/mlnet-tutorial-netcoreintegrationpkg)
- [<span data-ttu-id="d4eff-232">Beispiel: Skalierbares ML.NET-Modell auf ASP.NET Core-Web-API</span><span class="sxs-lookup"><span data-stu-id="d4eff-232">Sample: Scalable ML.NET model on ASP.NET Core WebAPI</span></span>](https://aka.ms/mlnet-sample-netcoreintegrationpkg)

## <a name="explore-the-generated-c-code-that-was-used-to-train-the-best-quality-model"></a><span data-ttu-id="d4eff-233">Untersuchen des generierten C#-Codes, der zum Trainieren des „Modell mit der besten Qualität“ verwendet wurde</span><span class="sxs-lookup"><span data-stu-id="d4eff-233">Explore the generated C# code that was used to train the "best quality" model</span></span> 

<span data-ttu-id="d4eff-234">Für weiterführende Lernzwecke können Sie auch den generierten C#-Code untersuchen, der vom CLI-Tool zum Trainieren des generierten Modells verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="d4eff-234">For more advanced learning purposes, you can also explore the generated C# code that was used by the CLI tool to train the generated model.</span></span>

<span data-ttu-id="d4eff-235">Dieser „Code des Trainingsmodells“ wird derzeit in der generierten benutzerdefinierten Klasse namens `ModelBuilder` generiert, sodass Sie diesen Trainingscode untersuchen können.</span><span class="sxs-lookup"><span data-stu-id="d4eff-235">That 'training model code' is currently generated in the custom class generated named `ModelBuilder` so you can investigate that training code.</span></span>

<span data-ttu-id="d4eff-236">Noch wichtiger ist, dass Sie diesen generierten Trainingscode in diesem speziellen Szenario (Modell zur Standpunktanalyse) auch mit dem im folgenden Tutorial beschriebenen Code vergleichen können:</span><span class="sxs-lookup"><span data-stu-id="d4eff-236">More importantly, for this particular scenario (Sentiment Analysis model) you can also compare that generated training code with the code explained in the following tutorial:</span></span>

- <span data-ttu-id="d4eff-237">Vergleich: [Tutorial: Verwenden von ML.NET in einem Standpunktanalyse-Szenario mit binärer Klassifizierung](https://docs.microsoft.com/en-us/dotnet/machine-learning/tutorials/sentiment-analysis).</span><span class="sxs-lookup"><span data-stu-id="d4eff-237">Compare: [Tutorial: Use ML.NET in a sentiment analysis binary classification scenario](https://docs.microsoft.com/en-us/dotnet/machine-learning/tutorials/sentiment-analysis).</span></span>

<span data-ttu-id="d4eff-238">Es ist interessant, den gewählten Algorithmus und die Pipelinekonfiguration im Tutorial mit dem vom CLI-Tool generierten Code zu vergleichen.</span><span class="sxs-lookup"><span data-stu-id="d4eff-238">It is interesting to compare the chosen algorithm and pipeline configuration in the tutorial with the code generated by the CLI tool.</span></span> <span data-ttu-id="d4eff-239">Je nachdem, wie viel Zeit Sie mit der Iteration und der Suche nach besseren Modellen verbringen, können der ausgewählte Algorithmus, die jeweiligen Hyperparametern und die Pipelinekonfiguration jeweils unterschiedlich sein.</span><span class="sxs-lookup"><span data-stu-id="d4eff-239">Depending on how much time you spend iterating and searching for better models, the chosen algorithm might be different along with its particular hyper-parameters and pipeline configuration.</span></span>

## <a name="see-also"></a><span data-ttu-id="d4eff-240">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="d4eff-240">See also</span></span>

- [<span data-ttu-id="d4eff-241">Automatisieren des Modelltrainings mit der ML.NET-CLI</span><span class="sxs-lookup"><span data-stu-id="d4eff-241">Automate model training with the ML.NET CLI</span></span>](../automate-training-with-cli.md)
- [<span data-ttu-id="d4eff-242">Tutorial: Ausführen von ML.NET-Modellen auf skalierbaren ASP.NET Core-Web-Apps und -Web-APIs</span><span class="sxs-lookup"><span data-stu-id="d4eff-242">Tutorial: Running ML.NET models on scalable ASP.NET Core web apps and WebAPIs</span></span>](https://aka.ms/mlnet-tutorial-netcoreintegrationpkg)
- [<span data-ttu-id="d4eff-243">Beispiel: Skalierbares ML.NET-Modell auf ASP.NET Core-Web-API</span><span class="sxs-lookup"><span data-stu-id="d4eff-243">Sample: Scalable ML.NET model on ASP.NET Core WebAPI</span></span>](https://aka.ms/mlnet-sample-netcoreintegrationpkg)
- [<span data-ttu-id="d4eff-244">Referenzhandbuch für den „auto-train“-Befehl in der ML.NET-CLI</span><span class="sxs-lookup"><span data-stu-id="d4eff-244">ML.NET CLI auto-train command reference guide</span></span>](../reference/ml-net-cli-reference.md) 
- [<span data-ttu-id="d4eff-245">Installieren des ML.NET-Befehlszeilenschnittstellen-Tools (CLI)</span><span class="sxs-lookup"><span data-stu-id="d4eff-245">How to install the ML.NET Command-Line Interface (CLI) tool</span></span>](../how-to-guides/install-ml-net-cli.md)
- [<span data-ttu-id="d4eff-246">Telemetrie in der ML.NET-CLI</span><span class="sxs-lookup"><span data-stu-id="d4eff-246">Telemetry in ML.NET CLI</span></span>](../resources/ml-net-cli-telemetry.md)

## <a name="next-steps"></a><span data-ttu-id="d4eff-247">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="d4eff-247">Next steps</span></span>

<span data-ttu-id="d4eff-248">In diesem Tutorial haben Sie gelernt, wie die folgenden Aufgaben ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="d4eff-248">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="d4eff-249">Vorbereiten der Daten für die ausgewählte ML-Aufgabe (zu lösendes Problem)</span><span class="sxs-lookup"><span data-stu-id="d4eff-249">Prepare your data for the selected ML task (problem to solve)</span></span>
> * <span data-ttu-id="d4eff-250">Ausführen des Befehls „mlnet auto-train“ über das CLI-Tool</span><span class="sxs-lookup"><span data-stu-id="d4eff-250">Run the 'mlnet auto-train' command in the CLI tool</span></span>
> * <span data-ttu-id="d4eff-251">Überprüfen der Ergebnisse der Qualitätsmetriken</span><span class="sxs-lookup"><span data-stu-id="d4eff-251">Review the quality metric results</span></span>
> * <span data-ttu-id="d4eff-252">Verstehen des generierten C#-Codes zur Ausführung des Modells (Code zur Verwendung in Ihrer Endbenutzer-App)</span><span class="sxs-lookup"><span data-stu-id="d4eff-252">Understand the generated C# code to run the model (Code to use in your end-user app)</span></span>
> * <span data-ttu-id="d4eff-253">Untersuchen des generierten C#-Codes, der zum Trainieren des „Modell mit der besten Qualität“ verwendet wurde (zu Lernzwecken)</span><span class="sxs-lookup"><span data-stu-id="d4eff-253">Explore the generated C# code that was used to train the "best quality" model (Learning purposes)</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d4eff-254">Automatisieren des Modelltrainings mit der ML.NET-CLI</span><span class="sxs-lookup"><span data-stu-id="d4eff-254">Automate model training with the ML.NET CLI</span></span>](../automate-training-with-cli.md)