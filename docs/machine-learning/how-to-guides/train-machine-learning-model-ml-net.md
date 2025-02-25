---
title: Eseguire il training di un modello e valutarlo
description: Informazioni su come eseguire il training di modelli di Machine Learning e su come valutarli in ML.NET
ms.date: 05/03/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc, how-to
ms.openlocfilehash: 3a3f1f672ed078754162dc377cf5c239d206b715
ms.sourcegitcommit: 682c64df0322c7bda016f8bfea8954e9b31f1990
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/13/2019
ms.locfileid: "65557844"
---
# <a name="train-and-evaluate-a-model"></a>Eseguire il training di un modello e valutarlo

Informazioni su come compilare modelli di Machine Learning, estrarre i parametri appresi e misurare le prestazioni con ML.NET. Questo campione esegue il training di un modello di regressione. I concetti, tuttavia, sono applicabili alla maggior parte degli altri algoritmi.

## <a name="split-data-for-training-and-testing"></a>Divisione dei dati per training e test

L'obiettivo di un modello di Machine Learning è di identificare motivi all'interno dei dati di training. Questi motivi vengono usati per eseguire stime con nuovi dati.

Dato il modello di dati seguente:

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

Caricare i dati in un'interfaccia [`IDataView`](xref:Microsoft.ML.IDataView):

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

Usare il metodo [`TrainTestSplit`](xref:Microsoft.ML.DataOperationsCatalog.TrainTestSplit*) per dividere i dati in set di training e set di test. Il risultato sarà un oggetto [`TrainTestData`](xref:Microsoft.ML.DataOperationsCatalog.TrainTestData) contenente due membri [`IDataView`](xref:Microsoft.ML.IDataView), uno per il set di training e l'altro per il set di test. La percentuale di suddivisione dei dati è determinata dal parametro `testFraction`. Il frammento di codice seguente contiene il 20% dei dati originali per il set di test.

```csharp
DataOperationsCatalog.TrainTestData dataSplit = mlContext.Data.TrainTestSplit(data, testFraction: 0.2);
IDataView trainData = dataSplit.TrainSet;
IDataView testData = dataSplit.TestSet;
```

## <a name="prepare-the-data"></a>Preparare i dati

I dati devono essere pre-elaborati prima del training di un modello di Machine Learning. Altre informazioni sulla preparazione dei dati sono reperibili nell'[articolo sulla procedura di preparazione dei dati](prepare-data-ml-net.md), nonché in [`transforms page`](../resources/transforms.md).

Gli algoritmi ML.NET presentano vincoli per i tipi di colonna di input. Quando non viene specificato alcun valore, poi, vengono usati valori predefiniti per i nomi delle colonne di input e di output.

### <a name="working-with-expected-column-types"></a>Uso di tipi di colonna previsti

Gli algoritmi di Machine Learning in ML.NET prevedono come input un vettore float di dimensione nota. Applicare l'attributo [`VectorType`](xref:Microsoft.ML.Data.VectorTypeAttribute) al modello di dati quando tutti i dati sono già in formato numerico e si intende elaborarli insieme, ad esempio nel caso dei pixel di un'immagine. 

Se i dati non sono tutti numerici e si vogliono applicare trasformazioni di dati diverse per ogni colonna singolarmente, usare il metodo [`Concatenate`](xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate*) dopo che tutte le colonne sono state elaborate, per combinare tutte le colonne singole in un unico vettore di funzionalità che viene restituito come output in una nuova colonna. 

Il frammento di codice seguente combina le colonne `Size` e `HistoricalPrices` in un unico vettore di funzionalità che viene restituito come output in una nuova colonna denominata `Features`. Poiché esiste una differenza di scala, viene applicato il metodo [`NormalizeMinMax`](xref:Microsoft.ML.NormalizationCatalog.NormalizeMinMax*) alla colonna `Features` per normalizzare i dati.

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

### <a name="working-with-default-column-names"></a>Uso di nomi di colonna predefiniti

Quando non vengono specificati nomi di colonna, gli algoritmi ML.NET usano nomi di colonna predefiniti. Tutti gli strumenti di training hanno un parametro denominato `featureColumnName` per gli input dell'algoritmo e, quando applicabile, hanno anche un parametro per il valore previsto, denominato `labelColumnName`. Per impostazione predefinita, tali valori sono rispettivamente `Features` e `Label`. 

Se si usa il metodo [`Concatenate`](xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate*) durante la pre-elaborazione per creare una nuova colonna denominata `Features`, non è necessario specificare la colonna Features nei parametri dell'algoritmo, perché è già presente nell'interfaccia `IDataView` pre-elaborata. La colonna Label è `CurrentPrice`, ma, poiché l'attributo [`ColumnName`](xref:Microsoft.ML.Data.ColumnNameAttribute) viene usato nel modello di dati, ML.NET rinomina la colonna `CurrentPrice` in `Label`. Questa operazione elimina la necessità di specificare il parametro `labelColumnName` per la stima dell'algoritmo di Machine Learning. 

Se non si vogliono usare i nomi di colonna predefiniti, passare i nomi delle colonne Features e Label come parametri quando si definisce la stima dell'algoritmo di Machine Learning, come illustrato dal frammento di codice seguente:

```csharp
var UserDefinedColumnSdcaEstimator = mlContext.Regression.Trainers.Sdca(labelColumnName: "MyLabelColumnName", featureColumnName: "MyFeatureColumnName");
```

## <a name="train-the-machine-learning-model"></a>Eseguire il training del modello di Machine Learning

Dopo che i dati sono stati pre-elaborati, usare il metodo [`Fit`](xref:Microsoft.ML.Trainers.TrainerEstimatorBase`2.Fit*) per eseguire il training del modello di Machine Learning con l'algoritmo di regressione [`StochasticDualCoordinateAscent`](xref:Microsoft.ML.Trainers.SdcaRegressionTrainer).

```csharp
// Define StochasticDualCoodrinateAscent regression algorithm estimator
var sdcaEstimator = mlContext.Regression.Trainers.Sdca();

// Build machine learning model
var trainedModel = sdcaEstimator.Fit(transformedTrainingData);
```

## <a name="extract-model-parameters"></a>Estrarre i parametri del modello

Dopo il training del modello, estrarre i parametri [`ModelParameters` ](xref:Microsoft.ML.Trainers.ModelParametersBase%601) appresi per l'ispezione o la riesecuzione del training. I parametri [`LinearRegressionModelParameters`](xref:Microsoft.ML.Trainers.LinearRegressionModelParameters) offrono il valore di distorsione e i coefficienti o i pesi appresi del modello con training. 

```csharp
var trainedModelParameters = trainedModel.Model as LinearRegressionModelParameters;
```

> [!NOTE]
> Altri modelli hanno parametri specifici per le proprie attività. L'[algoritmo K-Means](xref:Microsoft.ML.Trainers.KMeansTrainer), ad esempio, inserisce dati in un cluster in base al baricentro e [`KMeansModelParameters`](xref:Microsoft.ML.Trainers.KMeansModelParameters) contiene una proprietà che archivia i baricentri appresi. Per altre informazioni, vedere la [ documentazione dell'API `Microsoft.ML.Trainers`](xref:Microsoft.ML.Trainers) e cercare le classi con un nome contenente `ModelParameters`. 

## <a name="evaluate-model-quality"></a>Valutare la qualità del modello

Per facilitare la scelta del modello dalle prestazioni migliori, è essenziale valutarne le prestazioni su dati di test. Usare il metodo [`Evaluate`](xref:Microsoft.ML.RegressionCatalog.Evaluate*) per misurare diverse metriche del modello con training.

> [!NOTE]
> Il metodo `Evaluate` genera metriche diverse a seconda dell'attività di Machine Learning eseguita. Per altri dettagli, vedere la [ documentazione dell'API `Microsoft.ML.Data`](xref:Microsoft.ML.Data) e cercare le classi con un nome contenente `Metrics`. 

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

Nel codice di esempio precedente:  
1. Il set di dati di test viene pre-elaborato tramite le trasformazioni di preparazione dei dati definite in precedenza. 
2. Il modello di Machine Learning con training viene usato per eseguire stime sui dati di test.
3. Nel metodo `Evaluate` i valori nella colonna `CurrentPrice` del set di dati di test vengono confrontati con la colonna `Score` delle stime appena generate come output per calcolare le metriche per il modello di regressione, una delle quali, R quadrato, viene memorizzata nella variabile `rSquared`.

> [!NOTE]
> In questo breve esempio, R quadrato è un numero non compreso nell'intervallo 0-1 a causa della dimensione limitata dei dati. In uno scenario reale, si deve prevedere un valore compreso tra 0 e 1.
