---
title: Machine Learning automatizzato con ML.NET
description: Panoramica sulla selezione e il training automatici del modello
author: natke
ms.date: 05/01/2019
ms.topic: overview
ms.custom: mvc
ms.author: nakersha
ms.openlocfilehash: e6654718f174c9d8b628b05d85d74952c222eb66
ms.sourcegitcommit: 4c10802ad003374641a2c2373b8a92e3c88babc8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2019
ms.locfileid: "65452736"
---
# <a name="automated-machine-learning-with-mlnet"></a>Machine Learning automatizzato con ML.NET

Il Machine Learning automatizzato è una funzionalità di ML.NET che esegue la selezione e il training automatici del modello. Dopo aver specificato l'attività di Machine Learning e un set di dati, il Machine Learning automatizzato sceglie il modello con le metriche migliori. Vengono generati:
- un file di modello che può essere caricato nell'applicazione di previsione
- il codice dell'applicazione per eseguire previsioni
- il codice sorgente usato per la selezione delle caratteristiche e il training del modello (per comprendere il modello)

> [!NOTE]
> Questa funzionalità è attualmente in anteprima e il materiale può essere soggetto a modifiche. 

Il Machine Learning automatizzato è attualmente limitato alle [attività](resources/tasks.md) di Machine Learning di classificazione binaria, classificazione multiclasse e regressione. Le altre attività di Machine Learning saranno supportate nelle versioni future.

Il Machine Learning automatizzato può essere usato in tre modi:
1. Nella riga di comando con l'[interfaccia della riga di comando di ML.NET](automate-training-with-cli.md)
1. Tramite un'applicazione con l'[API di Machine Learning automatizzato](how-to-guides/how-to-use-the-automl-api.md)
1. Con un'interfaccia utente grafica con il generatore di modelli di ML.NET
