---
title: Ripetere il training di un modello
description: Informazioni su come ripetere il training di un modello in ML.NET
ms.date: 05/03/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc, how-to
ms.openlocfilehash: 2f8f8c035166612aabede8a512485bdf296c5655
ms.sourcegitcommit: 682c64df0322c7bda016f8bfea8954e9b31f1990
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/13/2019
ms.locfileid: "65557913"
---
# <a name="re-train-a-model"></a>Ripetere il training di un modello

Informazioni su come ripetere il training di un modello di Machine Learning in ML.NET.

Il mondo e i dati presenti in ogni sua parte cambiano a un ritmo costante. Di conseguenza, è necessario modificare e aggiornare anche i modelli. ML.NET offre funzionalità per ripetere il training dei modelli usando i parametri del modello appreso come punto di partenza per continuare a sfruttare le esperienze precedenti anziché iniziare ogni volta da zero.  

In ML.NET è possibile ripetere il training degli algoritmi seguenti:

- [AveragedPerceptronTrainer](xref:Microsoft.ML.Trainers.AveragedPerceptronTrainer)
- [FieldAwareFactorizationMachineTrainer](xref:Microsoft.ML.Trainers.FieldAwareFactorizationMachineTrainer)
- [LbfgsLogisticRegressionBinaryTrainer](xref:Microsoft.ML.Trainers.LbfgsLogisticRegressionBinaryTrainer)
- [LbfgsMaximumEntropyMulticlassTrainer](xref:Microsoft.ML.Trainers.LbfgsMaximumEntropyMulticlassTrainer)
- [LbfgsPoissonRegressionTrainer](xref:Microsoft.ML.Trainers.LbfgsPoissonRegressionTrainer)
- [LinearSvmTrainer](xref:Microsoft.ML.Trainers.LinearSvmTrainer)
- [OnlineGradientDescentTrainer](xref:Microsoft.ML.Trainers.OnlineGradientDescentTrainer)
- [SgdCalibratedTrainer](xref:Microsoft.ML.Trainers.SgdCalibratedTrainer)
- [SgdNonCalibratedTrainer](xref:Microsoft.ML.Trainers.SgdNonCalibratedTrainer)
- [SymbolicSgdLogisticRegressionBinaryTrainer](xref:Microsoft.ML.Trainers.SymbolicSgdLogisticRegressionBinaryTrainer)

## <a name="load-pre-trained-model"></a>Caricare il modello con training preliminare

Caricare prima il modello con training preliminare nell'applicazione. Per altre informazioni su come caricare pipeline e modelli di training, vedere il relativo [articolo sulla procedura](./consuming-model-ml-net.md).

```csharp
// Create MLContext
MLContext mlContext = new MLContext();

// Define DataViewSchema of data prep pipeline and trained model
DataViewSchema dataPrepPipelineSchema, modelSchema;

// Load data prepration pipeline
ITransformer dataPrepPipeline = mlContext.Model.Load("data_preparation_pipeline.zip", out dataPrepPipelineSchema);

// Load trained model
ITransformer trainedModel = mlContext.Model.Load("ogd_model.zip", out modelSchema);
```

## <a name="extract-pre-trained-model-parameters"></a>Estrarre i parametri del modello con training preliminare

Dopo aver caricato il modello, estrarre i parametri del modello appreso accedendo alla proprietà [`Model`](xref:Microsoft.ML.Data.PredictionTransformerBase`1.Model*) del modello con training preliminare. Il modello con training preliminare era stato sottoposto a training usando il modello di regressione lineare [`OnlineGradientDescentTrainer`](xref:Microsoft.ML.Trainers.OnlineGradientDescentTrainer) che crea una classe [`RegressionPredictionTransformer`](xref:Microsoft.ML.Data.RegressionPredictionTransformer%601) che restituisce [`LinearRegressionModelParameters`](xref:Microsoft.ML.Trainers.LinearRegressionModelParameters). Questi parametri del modello di regressione lineare contengono la distorsione e i pesi o i coefficienti appresi del modello. Questi valori verranno usati come punto di partenza per il nuovo modello.

```csharp
// Extract trained model parameters
LinearRegressionModelParameters originalModelParameters = 
    ((ISingleFeaturePredictionTransformer<object>)trainedModel).Model as LinearRegressionModelParameters;
```

## <a name="re-train-model"></a>Ripetere il training del modello

Il processo di ripetizione del training di un modello non è diverso da quello di training di un modello. L'unica differenza sta nel fatto che oltre ai dati, il metodo [`Fit`](xref:Microsoft.ML.Trainers.OnlineLinearTrainer`2.Fit*) acquisisce come input anche i parametri del modello appreso originale e li usa come punto di partenza nel processo di ripetizione del training.  

```csharp
// New Data
HousingData[] housingData = new HousingData[]
{
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

//Load New Data
IDataView newData = mlContext.Data.LoadFromEnumerable<HousingData>(housingData);

// Preprocess Data
IDataView transformedNewData = dataPrepPipeline.Transform(newData);

// Retrain model
RegressionPredictionTransformer<LinearRegressionModelParameters> retrainedModel = 
    mlContext.Regression.Trainers.OnlineGradientDescent()
        .Fit(transformedNewData, originalModelParameters);
```

## <a name="compare-model-parameters"></a>Confrontare i parametri del modello

Come si può verificare che il training sia stato effettivamente ripetuto? Un modo per verificarlo è confrontare i parametri del modello di cui si è ripetuto il training con quelli del modello originale e vedere se sono diversi. Il codice di esempio riportato di seguito confronta i pesi del modello originale con quelli del modello di nuovo sottoposto a training e li visualizza nella console.

```csharp
// Extract Model Parameters of re-trained model
LinearRegressionModelParameters retrainedModelParameters = retrainedModel.Model as LinearRegressionModelParameters;

// Inspect Change in Weights
var weightDiffs = 
    originalModelParameters.Weights.Zip(
        retrainedModelParameters.Weights, (original, retrained) => original - retrained).ToArray();

Console.WriteLine("Original | Retrained | Difference");
for(int i=0;i < weightDiffs.Count();i++)
{
    Console.WriteLine($"{originalModelParameters.Weights[i]} | {retrainedModelParameters.Weights[i]} | {weightDiffs[i]}");
}
```

La tabella seguente illustra il possibile output. 

|Originale | Training ripetuto | Differenza |
|---|---|---|
| 33039.86 | 56293.76 | -23253.9 |
| 29099.14 | 49586.03 | -20486.89 |
| 28938.38 | 48609.23 | -19670.85 |
| 30484.02 | 53745.43 | -23261.41 |
