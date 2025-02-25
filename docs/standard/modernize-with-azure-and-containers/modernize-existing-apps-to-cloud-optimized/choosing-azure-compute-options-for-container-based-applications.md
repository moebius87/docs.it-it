---
title: Scelta delle piattaforme di calcolo di Azure per le applicazioni basate su contenitore
description: Modernizzare le applicazioni .NET esistenti con contenitori Windows e il Cloud di Azure | Scegliere le piattaforme di calcolo di Azure per le applicazioni basate su contenitore
ms.date: 05/04/2018
ms.openlocfilehash: d91cd279402dc24beb5f766c06cb85ac8d74f482
ms.sourcegitcommit: 904b98d8d706f0e2d5ceaa00ce17ffbd92adfb88
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66758814"
---
# <a name="choosing-azure-compute-platforms-for-container-based-applications"></a>Scelta delle piattaforme di calcolo di Azure per le applicazioni basate su contenitore

Come è stato certamente notato dopo la lettura delle sezioni precedenti, Azure è un cloud aperta che offre più opzioni. È possibile usare la soluzione ottimale per le proprie esigenze, tuttavia, lo segnala anche domande su quale prodotto/tecnologia deve usare per le applicazioni in contenitori.

Come un *per impostazione predefinita* raccomandazione, di seguito è riportato il criterio principale consigliato in questa guida:

- **Singola app monolitica, occorre:** Scegliere servizio App di Azure
- **App a più livelli:** Scegliere gli agenti di orchestrazione, ad esempio Azure Kubernetes Service (AKS) o servizio App se si dispone di uno o alcuni servizi di back-end
- **Microservizi di Linux:** Scegliere servizio contenitore di AZURE/Kubernetes
- **Microservizi di Windows:** Scegliere le app Web di Azure per contenitori
- **Le funzioni senza server e i gestori eventi:** Scegliere le funzioni di Azure
- **Batch su vasta scala:** Scegli Azure Batch

Tuttavia, questa raccomandazione attenzione con un con avvicinamento delle dita di salt, come selezione del prodotto varia in base alle esigenze e le caratteristiche dell'applicazione specifico. Non tutte le applicazioni sono gli stessi, anche se inizialmente risultino tipi simili.

Dopo un'analisi più approfondita delle esigenze dell'applicazione, il prodotto selezionato potrebbe essere diverso. Ma, come punto di partenza, è buona norma disporre le indicazioni iniziali da dove iniziare la valutazione e test di base di determinate priorità.

Nella figura che segue, è possibile visualizzare un riepilogo dei diversi tipi di App e i relativi scenari di hosting di Azure ideale.

![](./media/image8.5.png)

> [!div class="step-by-step"]
> [Precedente](when-to-deploy-windows-containers-to-azure-container-service-kubernetes.md)
> [Successivo](build-resilient-services-ready-for-the-cloud-embrace-transient-failures-in-the-cloud.md)
