---
title: Casi in cui distribuire i contenitori di Windows per istanze di contenitore di Azure (ACI)
description: Modernizzare le applicazioni .NET esistenti con contenitori Windows e il Cloud di Azure | Casi in cui distribuire i contenitori di Windows per istanze di contenitore di Azure (ACI)
ms.date: 04/29/2018
ms.openlocfilehash: 3b6ae1ced9c4e01f5ab400e2575947a396064ebd
ms.sourcegitcommit: 904b98d8d706f0e2d5ceaa00ce17ffbd92adfb88
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66758590"
---
# <a name="when-to-deploy-windows-containers-to-azure-container-instances-aci"></a>Casi in cui distribuire i contenitori di Windows per istanze di contenitore di Azure (ACI)

Proposta di valore principale è che è possibile distribuire i contenitori sin da subito ad esso e non è necessario gestire quell'ambiente istanze di contenitore di Azure, non devi aggiornamento/patch del sistema operativo o le macchine virtuali, tutto questo è trasparente sottostante ed è stato distribuito contenitori in un ambiente pronto da usare.

I motivi e gli scenari quando si vuole usare ACI sono simili agli scenari principali quando si usano macchine virtuali di Azure con i contenitori, essenzialmente, gli scenari principali per l'uso di istanze di contenitore di Azure:

- **Scenari di sviluppo/Test**
- **Automazione delle attività**
- **Agenti di integrazione continua/recapito Continuo**
- **Elaborazione batch di piccole dimensioni e della scala**
- **App web semplice**

Lo scenario di App web semplice è uno scenario equo per ACI ma prendere in considerazione che poiché in ACI può avere solo un'istanza di contenitore singolo per ogni immagine del contenitore, si verificherà la disponibilità elevata e dispone solo di una scalabilità limitata.

Tuttavia, anche quando ACI è considerata infrastruttura perché fornisce solo le istanze di contenitore singolo, vi è un enorme vantaggio rispetto alle normali macchine virtuali di Azure con Windows Server. Con ACI, sufficiente distribuire i contenitori in un ambiente autonoma e paghi solo per i contenitori. Non è necessario mantenere/aggiornamento/patch le macchine virtuali, pertanto è una piattaforma molto migliorata per la maggior parte degli scenari in cui è possibile utilizzare le macchine virtuali con i contenitori. Usando ACI è molto semplice, sufficiente distribuire un contenitore, non è necessario creare un ambiente di macchine Virtuali sufficiente distribuire i contenitori.

I vantaggi principali di istanze di contenitore di Azure (ACI) sono:

- Esegui i contenitori senza gestire server
- Aumenta la flessibilità con contenitori on demand
- Distribuire contenitori nel cloud con semplicità senza precedenti e velocità, con un unico comando.
- Proteggere le applicazioni con isolamento con hypervisor

In breve, con ACI è possibile sviluppare App rapidamente senza la gestione delle macchine virtuali o che sia necessario imparare nuovi strumenti. È solo la tua applicazione, in un contenitore, in esecuzione nel cloud.

> [!div class="step-by-step"]
> [Precedente](when-to-deploy-windows-containers-to-azure-vms-iaas-cloud.md)
> [Successivo](when-to-deploy-windows-containers-to-azure-container-service-kubernetes.md)
