---
title: Che cos'è Docker?
description: Ottenere un po' più approfondito la comprensione di Docker, una semplice analogia può aiutarti.
ms.date: 02/15/2019
ms.openlocfilehash: 7747c4985af27be0a073fad2f22622f697f4ce27
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2019
ms.locfileid: "65644772"
---
# <a name="what-is-docker"></a>Che cos'è Docker?

[Docker](https://www.docker.com/) è un [progetto open source](https://github.com/docker/docker) per automatizzare la distribuzione di app come contenitori portabili e autosufficienti che possono essere eseguiti nel cloud o in locale. Docker è anche una [società](https://www.docker.com/) che promuove e sviluppa questa tecnologia, collaborando con fornitori cloud, Linux e Windows, inclusa Microsoft.

![I contenitori Docker possono essere eseguiti ovunque, in locale nel data center del cliente, in un provider di servizi esterno o nel cloud, in Azure.](./media/image2.png)

**La figura 1-2**. Docker distribuisce contenitori a tutti i livelli del cloud ibrido

I contenitori di immagini Docker possono essere eseguiti in modo nativo in Linux e Windows. Tuttavia, le immagini Windows possono essere eseguite solo negli host Windows e le immagini Linux possono essere eseguite in host Linux e in host Windows (con una macchina virtuale Linux Hyper-V, per il momento), dove con host si intende una macchina virtuale o un server.

Gli sviluppatori possono usare gli ambienti di sviluppo in Windows, Linux o macOS. Nel computer di sviluppo lo sviluppatore esegue un host Docker in cui vengono distribuite le immagini Docker, inclusa l'app e le relative dipendenze. Gli sviluppatori che lavorano in Linux o Mac, usare un host Docker basato su Linux, e possono solo creare immagini per i contenitori Linux. (Gli sviluppatori che lavorano nel Mac è possono modificare il codice o eseguire l'interfaccia della riga di comando di Docker (CLI) da macOS, ma al momento della stesura di questo articolo, non vengono eseguiti i contenitori direttamente in macOS.) Gli sviluppatori che lavorano in Windows possono creare le immagini sia per i contenitori Linux che per i contenitori Windows.

Per ospitare i contenitori negli ambienti di sviluppo e offrire strumenti di sviluppo aggiuntivi, Docker fornisce [Docker Community Edition (CE)](https://www.docker.com/community-edition) per Windows o per macOS. Questi i prodotti installano la macchina virtuale necessaria (host Docker) per l'hosting dei contenitori. Docker mette anche a disposizione [Docker Enterprise Edition (EE)](https://www.docker.com/enterprise-edition), progettato per lo sviluppo aziendale e usato dai team IT che compilano, distribuiscono ed eseguono applicazioni critiche di grandi dimensioni nell'ambiente di produzione.

Per eseguire i [contenitori Windows](/virtualization/windowscontainers/about/), esistono due tipi di runtime:

- **Contenitori di Windows Server** forniscono isolamento dell'applicazione tramite la tecnologia di isolamento dei processi e dello spazio dei nomi. Un contenitore Windows Server condivide un kernel con l'host del contenitore e con tutti i contenitori in esecuzione nell'host.

- **Contenitori di Hyper-V** espandono l'isolamento fornito dai contenitori di Windows Server eseguendo ciascun contenitore in una macchina virtuale altamente ottimizzata. In questa configurazione, il kernel dell'host dei contenitori non è condiviso con i contenitori Hyper-V per fornire un isolamento migliore.

Le immagini per questi contenitori vengono create e funzionano allo stesso modo. La differenza consiste nella modalità di creazione del contenitore dall'immagine, ovvero l'esecuzione di un contenitore di Hyper-V richiede un parametro aggiuntivo. Per informazioni dettagliate, vedere [Contenitori Hyper-V](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/hyperv-container).

## <a name="comparing-docker-containers-with-virtual-machines"></a>Confronto tra contenitori Docker e macchine virtuali

La figura 1-3 illustra un confronto tra le macchine virtuali e Docker dei contenitori.

![Per le macchine virtuali, sono disponibili tre livelli di base nel server host, dal basso verso l'alto: infrastruttura, sistema operativo host e un hypervisor. Ogni macchina virtuale ha inoltre un proprio sistema operativo e tutte le librerie necessarie. D'altra parte, per Docker, il server host ha solo l'infrastruttura e il sistema operativo e per di più, il motore del contenitore, che mantiene contenitore isolato, ma la condivisione di servizi di base del sistema operativo.](./media/image3.png)

**La figura 1-3**. Confronto tra macchine virtuali tradizionali e contenitori Docker

Poiché i contenitori richiedono un numero molto ridotto di risorse, ad esempio non necessitano di un sistema operativo completo, sono facili da distribuire e si avviano rapidamente. In questo modo è possibile ottenere un aumento della densità, vale a dire che è possibile eseguire più servizi nella stessa unità hardware, riducendo i costi.

Come effetto collaterale dell'esecuzione sullo stesso kernel, si ottiene un isolamento minore rispetto alle macchine virtuali.

L'obiettivo principale di un'immagine è garantire lo stesso ambiente (dipendenze) su distribuzioni diverse. Ciò significa che è possibile eseguirne il debug nel computer e quindi distribuirla in un altro computer, lo stesso ambiente garantito.

Un'immagine del contenitore consente di creare un pacchetto di app o servizi e distribuirlo in modo affidabile e riproducibile. Si potrebbe dire che Docker non è solo una tecnologia, ma anche una filosofia e un processo.

Quando si usa Docker, gli sviluppatori Docker non diranno mai "Funziona sul mio computer, perché non in produzione?" Può solo dicono "È in esecuzione in Docker", perché l'applicazione Docker in pacchetto può essere eseguito in qualsiasi ambiente Docker è supportata e viene eseguito il modo in cui si intendeva in tutte le destinazioni di distribuzione (ad esempio sviluppo, controllo qualità, staging e produzione).

## <a name="a-simple-analogy"></a>Una semplice analogia

Forse una semplice analogia può aiutare a comprendere a fondo il concetto di base di Docker.

Torniamo per qualche istante agli anni '50. Si sono verificati alcun elaboratori di testo e sono state utilizzate ovunque la fotocopiatrici (un certo senso).

Si immagini di dover distribuire rapidamente un numero elevato di lettere, ovvero di spedirle ai clienti, usando carta e buste, e di recapitarle fisicamente all'indirizzo di ogni cliente perché la posta elettronica non esisteva.

A un certo punto ci si rende conto che le lettere sono semplicemente composte da una lunga serie di paragrafi, che vengono presi e combinati in base alle esigenze e allo scopo della lettera e pertanto si progetta un sistema per distribuire le lettere rapidamente, aspettandosi un incremento notevole.

Il sistema è semplice:

1. Si inizia con un blocco di fogli trasparenti ognuno dei quali contiene un paragrafo.

2. Per distribuire una serie di lettere, si prendono i fogli con i paragrafi necessari, quindi li si dispone e li si allinea in modo da poterli leggere agevolmente.

3. Infine, si inserisce il blocco nella fotocopiatrice e si premere Start per ottenere tutte le lettere necessarie.

Questa è in sostanza l'idea alla base di Docker.

In Docker ogni livello è il set di modifiche che si verificano nel file system dopo l'esecuzione di un comando, ad esempio, l'installazione di un programma.

Quando dunque si esamina il file system dopo che il livello è stato copiato, vengono visualizzati tutti i file, incluso il livello quando il programma è stato installato.

Un'immagine può essere considerata come un disco rigido di sola lettura ausiliario pronto per essere installato in "computer" in cui è già installato il sistema operativo.

Allo stesso modo, un contenitore può essere considerato come il "computer" con il disco rigido dell'immagine installato. Il contenitore, proprio come un computer, può essere acceso o spento.

>[!div class="step-by-step"]
>[Precedente](index.md)
>[Successivo](docker-terminology.md)
