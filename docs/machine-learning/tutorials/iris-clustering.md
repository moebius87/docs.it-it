---
title: 'Esercitazione: Categorizzare i fiori di iris - clustering k-means'
description: Informazioni su come usare ML.NET in uno scenario di clustering
author: pkulikov
ms.author: johalex
ms.date: 05/16/2019
ms.topic: tutorial
ms.custom: mvc, seodec18, title-hack-0516
ms.openlocfilehash: 208e97419faee097db8e187081f2910b71ca2e35
ms.sourcegitcommit: c4e9d05644c9cb89de5ce6002723de107ea2e2c4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/19/2019
ms.locfileid: "65882279"
---
# <a name="tutorial-categorize-iris-flowers-using-k-means-clustering-with-mlnet"></a>Esercitazione: Classificare i fiori di iris usando il clustering k-means con ML.NET

Questa esercitazione illustra come usare ML.NET per compilare un [modello di clustering](../resources/tasks.md#clustering) per il [set di dati dei fiori iris](https://en.wikipedia.org/wiki/Iris_flower_data_set).

In questa esercitazione si imparerà a:
> [!div class="checklist"]
> - Informazioni sul problema
> - Selezionare l'attività di apprendimento automatico appropriata
> - Preparare i dati
> - Caricare e trasformare i dati
> - Scegliere un algoritmo di apprendimento
> - Eseguire il training del modello
> - Usare il modello per le stime

## <a name="prerequisites"></a>Prerequisiti

- [Visual Studio 2017 15.6 o versione successiva](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2017) con il carico di lavoro "Sviluppo multipiattaforma .NET Core" installato.

## <a name="understand-the-problem"></a>Informazioni sul problema

Questo problema riguarda il set dei fiori iris in gruppi diversi in base alle caratteristiche dei fiori. Tali caratteristiche sono la lunghezza e la larghezza di un sepalo e la lunghezza e la larghezza di un petalo. Per questa esercitazione, si supponga che il tipo di ogni fiore sia sconosciuto. Si desidera apprendere la struttura di un set di dati dalle caratteristiche e prevedere come un'istanza di dati si adatta a questa struttura.

## <a name="select-the-appropriate-machine-learning-task"></a>Selezionare l'attività di apprendimento automatico appropriata

Poiché non si sa a quale gruppo appartenga ciascun fiore, scegliere l'attività di [apprendimento automatico senza supervisione](../resources/glossary.md#unsupervised-machine-learning). Per dividere un set di dati in gruppi in modo che gli elementi nello stesso gruppo siano più simili tra loro rispetto a quelli di altri gruppi, usare un'attività di apprendimento automatico di [clustering](../resources/tasks.md#clustering).

## <a name="create-a-console-application"></a>Creare un'applicazione console

1. Aprire Visual Studio. Selezionare **File** > **Nuovo** > **Progetto** dalla barra dei menu. Nella finestra di dialogo **Nuovo progetto** selezionare il nodo **Visual C#** seguito dal nodo **.NET Core**. Selezionare quindi il modello di progetto **App console (.NET Core)**. Nella casella di testo **Nome** digitare "IrisFlowerClustering" e quindi selezionare il pulsante **OK**.

1. Creare una directory denominata *Dati* nel progetto per salvare i file del set di dati e del modello:

    In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **Nuova cartella**. Digitare "Data" e premere INVIO.

1. Installare il pacchetto NuGet **Microsoft.ML**:

    In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**. Scegliere "nuget.org" come origine pacchetto, selezionare la scheda **Sfoglia**, cercare **Microsoft.ML**, selezionare il pacchetto **v1.0.0** nell'elenco e selezionare il pulsante **Installa**. Selezionare il pulsante **OK** nella finestra di dialogo **Anteprima modifiche** e quindi selezionare il pulsante **Accetto** nella finestra di dialogo **Accettazione della licenza** se si accettano le condizioni di licenza per i pacchetti elencati.

## <a name="prepare-the-data"></a>Preparare i dati

1. Scaricare il set di dati [iris.data](https://github.com/dotnet/machinelearning/blob/master/test/data/iris.data) e salvarlo nella cartella *Dati* creata nel passaggio precedente. Per altre informazioni sui set di dati iris, vedere la pagina [Dataset Iris](https://en.wikipedia.org/wiki/Iris_flower_data_set) di Wikipedia e la pagina [Iris Data Set](https://archive.ics.uci.edu/ml/datasets/Iris), che rappresenta l'origine del set di dati.

1. In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul file *iris.data* e selezionare **Proprietà**. In **Avanzate** impostare il valore di **Copia nella directory di output** su **Copia se più recente**.

Il file *iris.data* contiene cinque colonne che rappresentano:

- lunghezza del sepalo in centimetri
- larghezza del sepalo in centimetri
- lunghezza del petalo in centimetri
- larghezza del petalo in centimetri
- tipo di fiore iris

Per l'esempio del clustering, questa esercitazione ignora l'ultima colonna.

## <a name="create-data-classes"></a>Creare classi di dati

Creare le classi per i dati di input e le stime:

1. In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **Nuovo elemento**.
1. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare **Classe** e impostare il campo **Nome** su *IrisData.cs*. Selezionare quindi il pulsante **Aggiungi**.
1. Aggiungere la direttiva `using` seguente al nuovo file:

   [!code-csharp[Add necessary usings](~/samples/machine-learning/tutorials/IrisFlowerClustering/IrisData.cs#Usings)]

Rimuovere la definizione di classe esistente e aggiungere il codice seguente, che contiene le classi `IrisData` e `ClusterPrediction`, al file *IrisData.cs*:

[!code-csharp[Define data classes](~/samples/machine-learning/tutorials/IrisFlowerClustering/IrisData.cs#ClassDefinitions)]

`IrisData` è la classe dei dati di input e contiene le definizioni per ogni caratteristica del set di dati. Usare l'attributo [LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute) per specificare gli indici delle colonne di origine nel file del set di dati.

La classe `ClusterPrediction` rappresenta l'output del modello di clustering applicato a un'istanza `IrisData`. Usare l'attributo [ColumnName](xref:Microsoft.ML.Data.ColumnNameAttribute) per associare i campi `PredictedClusterId` e `Distances` rispettivamente alle colonne **PredictedLabel** e **Score**. In caso di attività di clustering, queste colonne hanno il significato seguente:

- La colonna **PredictedLabel** include l'ID del cluster stimato.
- La colonna **Score** contiene una matrice con le distanze Euclidee quadrate rispetto ai centroidi del cluster. La lunghezza della matrice è uguale al numero di cluster.

> [!NOTE]
> Usare il tipo `float` per rappresentare i valori a virgola mobile nelle classi di dati di input e di previsione.

## <a name="define-data-and-model-paths"></a>Definire i percorsi dei dati e del modello

Tornare al file *Program.cs* e aggiungere due campi per i percorsi del file dei set di dati e del file per il salvataggio del modello:

- `_dataPath` contiene il percorso del file con il set di dati usato per il training del modello.
- `_modelPath` contiene il percorso del file in cui è archiviato il modello sottoposto a training.

Aggiungere il codice seguente immediatamente sopra il metodo `Main` per specificare questi percorsi:

[!code-csharp[Initialize paths](~/samples/machine-learning/tutorials/IrisFlowerClustering/Program.cs#Paths)]

Per consentire la compilazione del codice precedente, aggiungere le direttive `using` seguenti all'inizio del file *Program.cs*:

[!code-csharp[Add usings for paths](~/samples/machine-learning/tutorials/IrisFlowerClustering/Program.cs#UsingsForPaths)]

## <a name="create-ml-context"></a>Creare il contesto di Machine Learning

Aggiungere le direttive `using` seguenti all'inizio del file *Program.cs*:

[!code-csharp[Add Microsoft.ML usings](~/samples/machine-learning/tutorials/IrisFlowerClustering/Program.cs#MLUsings)]

Nel metodo `Main` sostituire la riga `Console.WriteLine("Hello World!");` con il codice seguente:

[!code-csharp[Create ML context](~/samples/machine-learning/tutorials/IrisFlowerClustering/Program.cs#CreateContext)]

La classe <xref:Microsoft.ML.MLContext?displayProperty=nameWithType> rappresenta l'ambiente di machine learning (apprendimento automatico) e offre meccanismi per la registrazione e punti di ingresso per il caricamento dei dati, il training del modello, le stime e altre attività. A livello concettuale questa operazione è paragonabile all'uso di `DbContext` in Entity Framework.

## <a name="setup-data-loading"></a>Impostazione del caricamento di dati

Aggiungere il codice seguente al metodo `Main` per configurare la modalità di caricamento di dati:

[!code-csharp[Create text loader](~/samples/machine-learning/tutorials/IrisFlowerClustering/Program.cs#CreateDataView)]

[ (`MLContext.Data.LoadFromTextFile`metodo di estensione](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) generico) deduce lo schema del set di dati dal tipo `IrisData` fornito e restituisce <xref:Microsoft.ML.IDataView>, che può essere usato come input per i trasformatori.

## <a name="create-a-learning-pipeline"></a>Creare una pipeline di apprendimento

Per questa esercitazione, la pipeline di apprendimento dell'attività di clustering è costituita dai due passaggi seguenti:

- Concatenare le colonne caricate in un'unica colonna **Features**, che viene usata da un trainer di clustering;
- Usare un trainer <xref:Microsoft.ML.Trainers.KMeansTrainer> per eseguire il training del modello usando l'algoritmo di clustering k-means++.

Aggiungere al metodo `Main` il codice seguente:

[!code-csharp[Create pipeline](~/samples/machine-learning/tutorials/IrisFlowerClustering/Program.cs#CreatePipeline)]

Il codice specifica che il set di dati deve essere suddiviso in tre cluster.

## <a name="train-the-model"></a>Eseguire il training del modello

I passaggi aggiunti nelle sezioni precedenti hanno preparato la pipeline per il training, che tuttavia non è stato eseguito. Aggiungere la riga seguente al metodo `Main` per eseguire il caricamento dei dati e il training del modello:

[!code-csharp[Train the model](~/samples/machine-learning/tutorials/IrisFlowerClustering/Program.cs#TrainModel)]

### <a name="save-the-model"></a>Salvare il modello

A questo punto, è disponibile un modello che può essere integrato in tutte le applicazioni .NET nuove o esistenti. Per salvare il modello in un file con estensione zip, aggiungere il codice seguente al metodo `Main`:

[!code-csharp[Save the model](~/samples/machine-learning/tutorials/IrisFlowerClustering/Program.cs#SaveModel)]

## <a name="use-the-model-for-predictions"></a>Usare il modello per le stime

Per eseguire stime, usare la classe <xref:Microsoft.ML.PredictionEngine%602>, che accetta le istanze del tipo di input tramite la pipeline convertitore e genera istanze del tipo di output. Aggiungere la riga seguente al metodo `Main` per creare un'istanza della classe:

[!code-csharp[Create predictor](~/samples/machine-learning/tutorials/IrisFlowerClustering/Program.cs#Predictor)]

Creare la classe `TestIrisData` per ospitare le istanze dei dati di test:

1. In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **Nuovo elemento**.
1. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare **Classe** e impostare il campo **Nome** su *TestIrisData.cs*. Selezionare quindi il pulsante **Aggiungi**.
1. Modificare la classe in modo da renderla statica, come nell'esempio seguente:

   [!code-csharp[Make class static](~/samples/machine-learning/tutorials/IrisFlowerClustering/TestIrisData.cs#Static)]

Questa esercitazione presenta un'istanza di dati iris all'interno di questa classe. È possibile aggiungere altri scenari da sperimentare con il modello. Aggiungere il codice seguente nella classe `TestIrisData`:

[!code-csharp[Test data](~/samples/machine-learning/tutorials/IrisFlowerClustering/TestIrisData.cs#TestData)]

Per individuare il cluster a cui appartiene l'elemento specificato, tornare al file *Program.cs* e aggiungere il codice seguente nel metodo `Main`:

[!code-csharp[Predict and output results](~/samples/machine-learning/tutorials/IrisFlowerClustering/Program.cs#PredictionExample)]

Eseguire il programma per vedere quale cluster contiene l'istanza dati specificata e le distanze al quadrato da tale istanza ai centroidi del cluster. I risultati saranno simili ai seguenti:

```text
Cluster: 2
Distances: 11.69127 0.02159119 25.59896
```

La procedura è stata completata. È stato creato un modello di apprendimento automatico per il clustering iris, quindi il modello è stato usato per produrre stime. È possibile trovare il codice sorgente per questa esercitazione nel repository GitHub [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/IrisFlowerClustering).

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione si è appreso come:
> [!div class="checklist"]
> - Informazioni sul problema
> - Selezionare l'attività di apprendimento automatico appropriata
> - Preparare i dati
> - Caricare e trasformare i dati
> - Scegliere un algoritmo di apprendimento
> - Eseguire il training del modello
> - Usare il modello per le stime

Visitare il repository GitHub per ottenere altre informazioni ed esempi.
> [!div class="nextstepaction"]
> [Repository GitHub dotnet/machinelearning](https://github.com/dotnet/machinelearning/)
