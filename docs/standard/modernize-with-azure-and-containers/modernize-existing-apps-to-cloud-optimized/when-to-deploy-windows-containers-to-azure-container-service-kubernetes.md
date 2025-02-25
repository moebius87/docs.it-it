---
title: Casi in cui distribuire i contenitori di Windows al servizio contenitore di Azure (ovvero Kubernetes)
description: Modernizzare le applicazioni .NET esistenti con contenitori Windows e il Cloud di Azure | Casi in cui distribuire i contenitori di Windows al servizio contenitore di Azure (ovvero Kubernetes)
ms.date: 04/30/2018
ms.openlocfilehash: 903082deba635dd0dfc22d0186fbc589f8d05b92
ms.sourcegitcommit: 904b98d8d706f0e2d5ceaa00ce17ffbd92adfb88
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66758561"
---
# <a name="when-to-deploy-windows-containers-to-azure-container-service-that-is-kubernetes"></a>Casi in cui distribuire i contenitori di Windows al servizio contenitore di Azure (ovvero Kubernetes)

Servizio contenitore di Azure ottimizza la configurazione di diffusi strumenti open source e tecnologie in modo specifico per Azure. Si ottiene una soluzione che offre la portabilità per i contenitori sia per la configurazione dell'applicazione. Si seleziona la dimensione, il numero di host e gli strumenti di orchestrator. Servizio contenitore di Azure gestisce l'infrastruttura per l'utente.

Se sta già lavorando con gli agenti di orchestrazione open source come Kubernetes, Docker Swarm o DC/OS, non devi modificare le procedure di gestione esistenti per spostare i carichi di lavoro di contenitore nel cloud. Usare gli strumenti di gestione di applicazioni che ha già familiarità e connettersi tramite l'endpoint API standard per l'agente di orchestrazione di propria scelta.

Tutti questi agenti di orchestrazione sono ambienti maturi se si usano contenitori Docker di Linux, ma potrebbe essere solo stavu anteprima per i contenitori di Windows.

Ad esempio, in Kubernetes, il supporto per contenitori è nativo (cittadino di prima classe), quindi l'uso di contenitori Windows in Kubernetes è inoltre efficace (in anteprima nel servizio contenitore di AZURE a partire dall'inizio del 2018).

Nota importante: L'evoluto e "PaaS più" versione di ACS (servizio contenitore di Azure) per Kubernetes è servizio contenitore di AZURE (Azure Kubernetes Service), tuttavia, i contenitori di Windows non sono ancora supportati a partire da 2018 (domanda 2), ma sarà supportata a breve.

>[!div class="step-by-step"]
>[Precedente](when-to-deploy-windows-containers-to-azure-container-instances-ACI.md)
>[Successivo](choosing-azure-compute-options-for-container-based-applications.md)
