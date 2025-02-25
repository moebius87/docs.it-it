---
title: Terminologia di Docker
description: Informazioni su alcuni termini di base che sono usato ogni giorno quando si lavora con Docker.
ms.date: 02/15/2019
ms.openlocfilehash: c352bf7235e8a3dc2d52bbbfe4390863fff9991f
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2019
ms.locfileid: "65644738"
---
# <a name="docker-terminology"></a>Terminologia di Docker

Questa sezione elenca i termini e le definizioni che è necessario conoscere prima di approfondire ulteriormente Docker. Per altre definizioni, vedere l'ampia [glossario](https://docs.docker.com/glossary/) fornito da Docker.

**Immagine del contenitore**: pacchetto con tutte le dipendenze e le informazioni necessarie per creare un contenitore. Un'immagine include tutte le dipendenze, ad esempio i framework, oltre alla configurazione della distribuzione e dell'esecuzione che deve essere usata dal runtime del contenitore. In genere, un'immagine deriva da più immagini di base con livelli impilati uno sopra l'altro in modo da formare il file system del contenitore. Un'immagine non può essere modificata dopo che è stata creata.

**Dockerfile**: Un file di testo contenente istruzioni su come creare un'immagine Docker. È ad esempio uno script batch, la prima riga indica l'immagine di base per iniziare con e quindi seguire le istruzioni per installare i programmi necessari, i file di copia, e così via, finché non viene visualizzato l'ambiente di lavoro è necessario.

**Compilazione**: azione di compilazione di un'immagine del contenitore in base alle informazioni e al contesto forniti dal Dockerfile corrispondente e ad altri file aggiuntivi nella cartella in cui viene creata l'immagine. È possibile compilare immagini con Docker **`docker build`** comando.

**Contenitore**: istanza di un'immagine Docker. Un contenitore rappresenta l'esecuzione di una singola applicazione o di un singolo processo o servizio. È costituito dal contenuto di un'immagine Docker, da un ambiente di esecuzione e da un set di istruzioni standard. Quando si ridimensiona un servizio, si creano più istanze di un contenitore dalla stessa immagine oppure in un processo batch può creare più contenitori dalla stessa immagine, passando parametri diversi a ogni istanza.

**Volumi**: offrono un file system scrivibile che può essere usato dal contenitore. Poiché le immagini sono di sola lettura, ma la maggior parte dei programmi deve poter scrivere nel file system, i volumi aggiungono un livello scrivibile, superiore a quello dell'immagine del contenitore, in modo che i programmi abbiano accesso a un file system scrivibile. Il programma non sa accede a un file System a più livelli, è semplicemente il file System come di consueto. I volumi si trovano nel sistema host e sono gestiti da Docker.

**Tag**: contrassegno o etichetta che si può applicare alle immagini per poter identificare immagini o versioni diverse della stessa immagine, a seconda del numero di versione o dell'ambiente di destinazione.

**Compilazione in più fasi**: funzionalità disponibile in Docker 17.05 o versioni successive, che consente di ridurre le dimensioni delle immagini finali. In breve, con la compilazione in più fasi è possibile usare, ad esempio, un'immagine di base di grandi dimensioni, contenente l'SDK, per la compilazione e la pubblicazione dell'applicazione e quindi usare la cartella di pubblicazione con un'immagine di base solo runtime di piccole dimensioni, per produrre un'immagine finale molto più piccola

**Repository**: raccolta di immagini Docker correlate, etichettate con un tag che indica la versione dell'immagine. Alcuni repository contengono più varianti di un'immagine specifica, ad esempio un'immagine contenente gli SDK (più pesante), un'immagine contenente solo i runtime (più leggera) e così via. Tali varianti possono essere contrassegnate con i tag. Un singolo repository può contenere varianti di piattaforme, ad esempio un'immagine Linux e un'immagine Windows.

**Registro**: servizio che fornisce l'accesso ai repository. Il registro predefinito per la maggior parte delle immagini pubbliche è l'[Hub Docker](https://hub.docker.com/), di proprietà di Docker a livello di organizzazione. Un registro contiene in genere i repository di più team. Spesso le aziende hanno registri privati in cui archiviare e gestire le immagini che hanno creato. Registro Azure Container è un esempio.

**Immagine con multiarchitettura**: Per multi-un'architettura, è una funzionalità che semplifica la selezione dell'immagine appropriata, a seconda della piattaforma in cui Docker è in esecuzione, ad esempio, quando un documento Dockerfile richiede un'immagine di base **`FROM mcr.microsoft.com/dotnet/core/sdk:2.2`** dal Registro di sistema in realtà Ottiene **`2.2-nanoserver-1709`**, **`2.2-nanoserver-1803`**, **`2.2-nanoserver-1809`** o **`2.2-stretch`**, a seconda del sistema operativo e versione in cui è in esecuzione Docker.

**Hub Docker**: registro pubblico in cui caricare le immagini e usarle. L'hub Docker fornisce l'hosting di immagini Docker, registri pubblici o privati, trigger e webhook di compilazione e integrazione con GitHub e Bitbucket.

**Registro Azure Container**: risorsa pubblica per l'uso di immagini Docker e dei relativi componenti in Azure. Fornisce un registro vicino le distribuzioni in Azure e che consente di controllare l'accesso, rendendo possibile usare i gruppi di Azure Active Directory e le autorizzazioni.

**Docker Trusted Registry (DTR)**: servizio di registro Docker (di Docker) che può essere installato in locale per risiedere all'interno del data center e della rete dell'organizzazione. È utile per le immagini private che devono essere gestite all'interno dell'organizzazione. Docker Trusted Registry è incluso nel prodotto Docker Datacenter. Per altre informazioni, vedere [Docker Trusted Registry (DTR)](https://docs.docker.com/docker-trusted-registry/overview/).

**Docker Community Edition (CE)**: strumenti di sviluppo per Windows e macOS per la compilazione, l'esecuzione e il test dei contenitori in locale. Docker CE per Windows offre ambienti di sviluppo per contenitori Linux e Windows. L'host Docker Linux in Windows è basato su una macchina virtuale [Hyper-V](https://www.microsoft.com/cloud-platform/server-virtualization). L'host per Contenitori Windows è direttamente basato su Windows. Docker CE per Mac è basato sul framework Hypervisor di Apple e sull'[hypervisor xhyve](https://github.com/mist64/xhyve), che fornisce una macchina virtuale host Docker Linux in Mac OS X. Docker CE per Windows e per Mac sostituisce Docker Toolbox, basato su Oracle VirtualBox.

**Docker Enterprise Edition (EE)**: versione di livello aziendale degli strumenti Docker per lo sviluppo per Linux e Windows.

**Compose**: strumento da riga di comando e formato di file YAML con i metadati per definire ed eseguire applicazioni multicontenitore. Si definisce una singola applicazione in base a più immagini con uno o più file YML che possono eseguire l'override dei valori a seconda dell'ambiente. Dopo aver creato le definizioni, è possibile distribuire l'intera applicazione multicontenitore con un unico comando (docker-compose backup) che crea un contenitore per ogni immagine nell'host Docker.

**Cluster**: raccolta di host Docker esposta come se si trattasse di un singolo host Docker virtuale, per consentire la scalabilità dell'applicazione aggiungendo più istanze dei servizi distribuite tra più host all'interno del cluster. È possibile creare i cluster Docker con Kubernetes, Azure Service Fabric Docker Swarm e Mesosphere DC/OS.

**Agente di orchestrazione**: strumento che semplifica la gestione di cluster e host Docker. Gli agenti di orchestrazione consentono di gestire le immagini, contenitori e host tramite un'interfaccia della riga di comando (CLI) o un'interfaccia utente grafica. È possibile gestire le reti di contenitori, le configurazioni, il bilanciamento del carico, l'individuazione di servizi, la disponibilità elevata, la configurazione dell'host Docker e altro ancora. Un agente di orchestrazione è responsabile dell'esecuzione, della distribuzione, del ridimensionamento e della correzione dei carichi di lavoro in una raccolta di nodi. In genere, i prodotti per l'agente di orchestrazione sono gli stessi che forniscono l'infrastruttura cluster, ad esempio Kubernetes, Azure Service Fabric e altre offerte disponibili sul mercato.

>[!div class="step-by-step"]
>[Precedente](what-is-docker.md)
>[Successivo](docker-containers-images-and-registries.md)
