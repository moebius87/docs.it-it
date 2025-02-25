---
title: Comando dotnet list package
description: Il comando "dotnet list package" offre un'opzione utile per visualizzare un elenco dei riferimenti al pacchetto per un progetto o una soluzione.
ms.date: 04/09/2019
ms.openlocfilehash: 88ef3302a955eadc4167384312e4eb721dd496fb
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2019
ms.locfileid: "65631758"
---
# <a name="dotnet-list-package"></a>dotnet list package

[!INCLUDE [topic-appliesto-net-core-22plus](../../../includes/topic-appliesto-net-core-22plus.md)]

## <a name="name"></a>nome

`dotnet list package`: elenca i riferimenti al pacchetto per un progetto o una soluzione.

## <a name="synopsis"></a>Riepilogo

```
dotnet list [<PROJECT | SOLUTION>] package [--config] [--framework] [--highest-minor] [--highest-patch] 
   [--include-prerelease] [--include-transitive] [--outdated] [--source]
dotnet list package [-h|--help]
```

## <a name="description"></a>Description

Il comando `dotnet list package` offre un'opzione utile per visualizzare un elenco di tutti i riferimenti al pacchetto NuGet per un progetto o una soluzione specifica. Occorre prima di tutto compilare il progetto in modo da avere le risorse che il comando dovrà elaborare. L'esempio seguente mostra l'output del comando `dotnet list package` per il progetto [SentimentAnalysis](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/SentimentAnalysis):

```output
Project 'SentimentAnalysis' has the following package references
   [netcoreapp2.1]:
   Top-level Package               Requested   Resolved
   > Microsoft.ML                  0.11.0      0.11.0
   > Microsoft.NETCore.App   (A)   [2.1.0, )   2.1.0

(A) : Auto-referenced package.
```

La colonna **Requested** si riferisce alla versione del pacchetto specificata nel file di progetto e può essere un intervallo. La colonna **Resolved** indica la versione attualmente usata dal progetto ed è sempre un valore singolo. I pacchetti contrassegnati da una `(A)` accanto al nome rappresentano [riferimenti impliciti al pacchetto](csproj.md#implicit-package-references) derivati dalle impostazioni del progetto (tipo `Sdk`, proprietà `<TargetFramework>` o `<TargetFrameworks>` e così via).

Usare l'opzione `--outdated` per determinare se sono disponibili versioni più recenti dei pacchetti usati nei progetti. Per impostazione predefinita, `--outdated` elenca i pacchetti stabili più recenti, a meno che anche la versione risolta non sia una versione non definitiva. Per includere le versioni non definitive nell'elenco delle versioni più recenti, specificare anche l'opzione `--include-prerelease`. Gli esempi seguenti mostrano l'output del comando `dotnet list package --outdated --include-prerelease` per lo stesso progetto dell'esempio precedente:

```output
The following sources were used:
   https://api.nuget.org/v3/index.json

Project `SentimentAnalysis` has the following updates to its packages
   [netcoreapp2.1]:
   Top-level Package      Requested   Resolved   Latest
   > Microsoft.ML         0.11.0      0.11.0     1.0.0-preview
```

Se occorre verificare se il progetto ha dipendenze transitive, usare l'opzione `--include-transitive`. Le dipendenze transitive si hanno quando si aggiunge al progetto un pacchetto che a sua volta si basa su un altro pacchetto. L'esempio seguente mostra l'output dell'esecuzione del comando `dotnet list package --include-transitive` per il progetto [HelloPlugin](https://github.com/dotnet/samples/tree/master/core/extensions/AppWithPlugin/HelloPlugin), che mostra i pacchetti di primo livello e i pacchetti da cui dipendono:

```output
Project 'HelloPlugin' has the following package references
   [netcoreapp3.0]:
   Top-level Package                      Requested                    Resolved
   > Microsoft.NETCore.Platforms    (A)   [3.0.0-preview3.19128.7, )   3.0.0-preview3.19128.7
   > Microsoft.WindowsDesktop.App   (A)   [3.0.0-preview3-27504-2, )   3.0.0-preview3-27504-2

   Transitive Package               Resolved
   > Microsoft.NETCore.Targets      2.0.0
   > PluginBase                     1.0.0

(A) : Auto-referenced package.
```

## <a name="arguments"></a>Argomenti

`PROJECT | SOLUTION`

File di progetto o di soluzione su cui eseguire le operazioni. Se non specificato, il comando ne cerca uno nella directory corrente. Se vengono trovati più progetti o soluzioni, viene generato un errore.

## <a name="options"></a>Opzioni

* **`--config <SOURCE>`**

  Origini NuGet da usare per cercare i pacchetti più recenti. Richiede l'opzione `--outdated`.

* **`--framework <FRAMEWORK>`**

  Visualizza solo i pacchetti validi per il [framework di destinazione](../../standard/frameworks.md) specificato. Per specificare più framework, ripetere l'opzione più volte. Ad esempio: `--framework netcoreapp2.2 --framework netstandard2.0`.

* **`-h|--help`**

  Stampa una breve guida per il comando.

* **`--highest-minor`**

  Prende in considerazione solo i pacchetti con il numero di versione principale corrispondente quando si cercano i pacchetti più recenti. Richiede l'opzione `--outdated`.

* **`--highest-patch`**

  Prende in considerazione solo i pacchetti con i numeri di versione principale e secondaria corrispondenti quando si cercano i pacchetti più recenti. Richiede l'opzione `--outdated`.

* **`--include-prerelease`**

  Prende in considerazione i pacchetti con versioni non definitive quando si cercano i pacchetti più recenti. Richiede l'opzione `--outdated`.

* **`--include-transitive`**

  Elenca i pacchetti transitivi, oltre ai pacchetti di primo livello. Quando si specifica questa opzione, si ottiene un elenco di pacchetti da cui dipendono i pacchetti di primo livello.

* **`--outdated`**

  Elenca i pacchetti per cui sono disponibili versioni più recenti.

* **`-s|--source <SOURCE>`**

  Origini NuGet da usare per cercare i pacchetti più recenti. Richiede l'opzione `--outdated`.

## <a name="examples"></a>Esempi

* Elencare i riferimenti al pacchetto di un progetto specifico:

  ```console
  dotnet list SentimentAnalysis.csproj package
  ```

* Elencare i riferimenti al pacchetto per cui sono disponibili versioni più recenti, incluse le versioni non definitive:

  ```console
  dotnet list package --outdated --include-prerelease
  ```

* Elencare i riferimenti al pacchetto per un framework di destinazione specifico:

  ```console
  dotnet list package --framework netcoreapp3.0
  ```
