---
title: Orchestrazione di microservizi e applicazioni a più contenitori per la scalabilità e la disponibilità elevate
description: Informazioni sulle opzioni che consentono di orchestrare microservizi e applicazioni a più contenitori per la scalabilità e la disponibilità elevate e le possibilità di Azure Dev Spaces nello sviluppo del ciclo di vita dell'applicazione Kubernetes.
ms.date: 09/20/2018
ms.openlocfilehash: 27155736c6b5308d4794b17e5f5bd0b93109b5c1
ms.sourcegitcommit: 96543603ae29bc05cecccb8667974d058af63b4a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66196043"
---
# <a name="orchestrating-microservices-and-multi-container-applications-for-high-scalability-and-availability"></a>Orchestrazione di microservizi e applicazioni a più contenitori per la scalabilità e la disponibilità elevate

L'uso di agenti di orchestrazione per applicazioni pronte per la produzione è essenziale se l'applicazione è basata su microservizi o semplicemente suddivisa tra più contenitori. Come illustrato in precedenza, in un approccio basato su microservizi ogni microservizio è proprietario dei rispettivi modelli e dati, quindi sarà autonomo dal punto di vista dello sviluppo e della distribuzione. Anche se è disponibile un'applicazione più tradizionale costituita da più servizi, ad esempio SOA, saranno comunque disponibili più contenitori o servizi che comprendono una singola applicazione aziendale da distribuire come sistema distribuito. Il ridimensionamento e la gestione di questi tipi di sistemi sono complessi ed è quindi assolutamente necessario un agente di orchestrazione se si vuole ottenere un'applicazione a più contenitori pronta per la produzione e ridimensionabile.

La figura 4-23 illustra la distribuzione in un cluster di un'applicazione costituita da più microservizi (contenitori).

![Applicazioni Docker composte in un cluster: usare un contenitore per ogni istanza del servizio. I contenitori Docker sono "unità di distribuzione" e un contenitore è un'istanza di un Docker. Un host gestisce più contenitori](./media/image23.png)

**Figura 4-23**. Cluster di contenitori

Si tratta di un approccio apparentemente logico. Occorre tuttavia stabilire come vengono gestiti il bilanciamento del carico, il routing e l'orchestrazione di queste applicazioni.

Il semplice motore Docker in host Docker singoli soddisfa le esigenze di gestione di istanze singole per immagine su un host, ma non è sufficiente in caso di gestione di più contenitori distribuiti su più host per applicazioni distribuite più complesse. Nella maggior parte dei casi è necessaria una piattaforma di gestione in grado di avviare automaticamente i contenitori, ridimensionare i contenitori con più istanze per immagine, sospenderli o arrestarli in caso di necessità e idealmente controllare anche la rispettiva modalità di accesso a risorse come la rete e l'archiviazione dati.

Per passare a un livello superiore rispetto alla gestione di singoli contenitori o app composte molto semplici e gestire ad applicazioni aziendali di dimensioni maggiori con microservizi, è necessario usare l'orchestrazione e le piattaforme di clustering.

Dal punto di vista dell'architettura e dello sviluppo, se si creano applicazioni aziendali composte di grandi dimensioni e basate su microservizi, è importante comprendere le piattaforme e i prodotti seguenti che supportano gli scenari avanzati:

**Cluster e agenti di orchestrazione.** Quando è necessario aumentare il numero di istanze delle applicazioni in molti host Docker, ad esempio nel caso di un'applicazione di grandi dimensioni basata su microservizi, è essenziale poter gestire tutti questi host come un singolo cluster tramite l'astrazione della complessità della piattaforma sottostante. I cluster di contenitori e gli agenti di orchestrazione offrono questo vantaggio. Sono esempi di agenti di orchestrazione Azure Service Fabric e Kubernetes. Kubernetes è disponibile in Azure tramite il servizio Azure Kubernetes.

**Utilità di pianificazione.** Per *pianificazione* si intende la possibilità per un amministratore di avviare contenitori in un cluster in modo che forniscano anche un'interfaccia utente. Un'utilità di pianificazione di cluster ha molte responsabilità, tra cui usare in modo efficiente le risorse del cluster, configurare i vincoli specificati dall'utente, bilanciare in modo efficiente il carico dei contenitori nei nodi e negli host e infine assicurare l'affidabilità in caso di errori, offrendo al tempo stesso una disponibilità elevata.

I concetti di cluster e utilità di pianificazione sono strettamente correlati, quindi i prodotti offerti da diversi fornitori includono spesso entrambi i set di funzionalità. L'elenco seguente mostra le opzioni più importanti a livello di piattaforma e software disponibili per i cluster e per le utilità di pianificazione. Questi agenti di orchestrazione sono in genere offerti in cloud pubblici quali Azure.

## <a name="software-platforms-for-container-clustering-orchestration-and-scheduling"></a>Piattaforme software per il clustering, l'orchestrazione e la pianificazione dei contenitori

### <a name="kubernetes"></a>Kubernetes

![Logo di Kubernetes](./media/image24.png)

> [*Kubernetes*](https://kubernetes.io/) è un prodotto open source che offre funzionalità per l'infrastruttura dei cluster, la pianificazione dei contenitori e l'orchestrazione. Consente di automatizzare la distribuzione, il ridimensionamento e la gestione di contenitori di applicazioni in cluster di host.
>
> *Kubernetes* offre un'infrastruttura incentrata sui contenitori che raggruppa i contenitori di applicazioni in unità logiche per semplificarne la gestione e l'individuazione.
>
> *Kubernetes* è maturo in Linux, meno maturo in Windows.

### <a name="azure-kubernetes-service-aks"></a>Servizio Azure Kubernetes

![Logo del servizio Azure Kubernetes](./media/image41.png)

> Il [servizio Azure Kubernetes](https://azure.microsoft.com/services/kubernetes-service/) è un servizio di orchestrazione dei contenitori Kubernetes gestito in Azure, che semplifica gestione, distribuzioni e operazioni del cluster Kubernetes.

### <a name="azure-service-fabric"></a>Azure Service Fabric

![Logo di Azure Service Fabric](./media/image27.png)

> [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) è una piattaforma di microservizi Microsoft per la creazione di applicazioni. È un [agente di orchestrazione](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-introduction) di servizi e crea cluster di macchine virtuali. Service Fabric può distribuire servizi come contenitori o come processi semplici. Può anche combinare servizi nei processi con servizi nei contenitori entro la stessa applicazione e lo stesso cluster.
>
> I cluster di *Service Fabric* possono essere distribuiti in Azure, in locale o in qualsiasi cloud. La distribuzione in Azure è tuttavia semplificata con un approccio gestito.
>
> *Service Fabric* offre [modelli di programmazione di Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-choose-framework) prescrittivi aggiuntivi e facoltativi, come [servizi con stato](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction) e [Reliable Actors](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction).
>
> *Service Fabric* è maturo in Windows (anni di evoluzione in Windows), meno maturo in Linux.
>
> I contenitori Linux e Windows sono supportati in Service Fabric dal 2017.

### <a name="azure-service-fabric-mesh"></a>Azure Service Fabric Mesh

![Logo di Azure Service Fabric Mesh](./media/image35.png)

> [*Azure Service Fabric Mesh*](https://docs.microsoft.com/azure/service-fabric-mesh/service-fabric-mesh-overview) è analogo a Service Fabric in termini di affidabilità, prestazioni cruciali e scalabilità, ma offre una piattaforma completamente gestita e senza server. Non è necessario gestire un cluster, macchine virtuali, risorse di archiviazione o configurazione della rete. È sufficiente concentrarsi sullo sviluppo dell'applicazione.
>
> *Service Fabric Mesh* supporta contenitori Windows e Linux, consentendo lo sviluppo con qualsiasi linguaggio di programmazione e framework di propria scelta.

## <a name="using-container-based-orchestrators-in-microsoft-azure"></a>Uso degli agenti di orchestrazione basati su contenitori in Microsoft Azure

Alcuni fornitori cloud, tra cui Microsoft Azure, Amazon EC2 Container Service e Google Container Engine, offrono il supporto per contenitori Docker oltre al supporto per i contenitori e l'agente di orchestrazione Docker. Microsoft Azure offre il supporto per cluster e agente di orchestrazione Docker tramite il servizio Azure Kubernetes, Azure Service Fabric e Azure Service Fabric Mesh.

## <a name="using-azure-kubernetes-service"></a>Servizio Azure Kubernetes

Un cluster Kubernetes crea più pool di host Docker e li espone come un singolo host Docker virtuale, per consentire di distribuire più contenitori nel cluster e scalare orizzontalmente qualsiasi numero di istanze contenitore. Il cluster gestirà tutte le operazioni di gestione complesse, ad esempio la scalabilità, l'integrità e così via.

Il servizio Azure Kubernetes consente di semplificare la creazione, la configurazione e la gestione in Azure di un cluster di macchine virtuali preconfigurate per l'esecuzione di applicazioni in contenitori. Usando una configurazione ottimizzata degli strumenti open source più diffusi per la pianificazione e l'orchestrazione, il servizio Azure Kubernetes consente di usare le competenze esistenti o di avvalersi delle vaste competenze in continua crescita della community per distribuire e gestire le applicazioni basate su contenitori in Microsoft Azure.

Il servizio Azure Kubernetes ottimizza la configurazione degli strumenti e delle tecnologie open source più diffusi per clustering Docker in modo specifico per Azure. Si ottiene una soluzione che offre la portabilità per la configurazione di contenitori e applicazioni. È sufficiente selezionare le dimensioni, il numero di host e gli strumenti dell'agente di orchestrazione. Il servizio Azure Kubernetes gestisce tutte le altre operazioni.

![Struttura del cluster Kubernetes: è presente un nodo master che gestisce DNS, utilità di pianificazione, proxy e così via e diversi nodi del ruolo di lavoro che ospitano i contenitori.](media/image36.png)

**Figura 4-24**. Struttura semplificata e topologia del cluster Kubernetes

La figura 4-24 illustra la struttura di un cluster Kubernetes in cui un nodo master (VM) controlla la maggior parte del coordinamento del cluster. È possibile distribuire i contenitori al resto dei nodi (che vengono gestiti come un singolo pool dal punto di vista dell'applicazione) e la struttura consente la scalabilità a migliaia o persino decine di migliaia di contenitori.

## <a name="development-environment-for-kubernetes"></a>Ambiente di sviluppo per Kubernetes

Nell'ambiente di sviluppo, [Docker ha annunciato nel mese di luglio 2018](https://blog.docker.com/2018/07/kubernetes-is-now-available-in-docker-desktop-stable-channel/) che Kubernetes può essere eseguito anche in un singolo computer di sviluppo (Windows 10 o macOS) semplicemente installando [ Docker Desktop](https://docs.docker.com/install/). È possibile procedere in un secondo momento alla distribuzione nel cloud (servizio Azure Kubernetes) per altri test di integrazione, come illustrato nella figura 4-25.

![Docker ha annunciato il supporto dei computer di sviluppo per i cluster Kubernetes nel luglio 2018 con Docker Desktop.](media/image37.png) 

**Figura 4-25**. Esecuzione di Kubernetes in computer di sviluppo e nel cloud

## <a name="getting-started-with-azure-kubernetes-service-aks"></a>Introduzione al servizio Azure Kubernetes 

Per iniziare a usare il servizio Azure Kubernetes, distribuire un cluster del servizio Azure Kubernetes dal portale di Azure o tramite l'interfaccia della riga di comando. Per altre informazioni sulla distribuzione di un cluster Kubernetes in Azure, vedere [Distribuire un cluster del servizio Azure Kubernetes](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal).

Non sono previsti addebiti per il software installato per impostazione predefinita come parte del servizio Azure Kubernetes. Tutte le opzioni predefinite vengono implementate con software open source. Il servizio Azure Kubernetes è disponibile per più macchine virtuali in Azure. Vengono applicati addebiti solo per le istanze di risorse di calcolo scelte, oltre che per le altre risorse di infrastruttura sottostanti usate, ad esempio per le risorse di archiviazione e di rete. Non sono previsti addebiti incrementali per il servizio Azure Kubernetes.

Per altre informazioni sull'implementazione nella distribuzione in Kubernetes basata su kubectl e sui file con estensione yaml originali, vedere il post sull'[impostazione di eShopOnContainers nel servizio Azure Kubernetes](https://github.com/dotnet-architecture/eShopOnContainers/wiki/10.-Setting-the-solution-up-in-AKS-(Azure-Kubernetes-Service)).

## <a name="deploying-with-helm-charts-into-kubernetes-clusters"></a>Distribuzione con grafici Helm in cluster Kubernetes

Quando si distribuisce un'applicazione a un cluster Kubernetes, è possibile usare lo strumento dell'interfaccia della riga di comando kubectl.exe originale con i file di distribuzione basati sul formato nativo (file con estensione yaml), come già accennato nella sezione precedente. Per le applicazioni Kubernetes più complesse, ad esempio durante la distribuzione di applicazioni complesse basate su microservizi, è tuttavia consigliabile usare [Helm](https://helm.sh/).

I grafici Helm consentono di definire, controllare la versione, installare, condividere, aggiornare o ripristinare lo stato precedente anche dell'applicazione Kubernetes più complessa.

Inoltre l'uso di Helm è consigliabile perché anche altri ambienti Kubernetes in Azure, come [Azure Dev Spaces](https://docs.microsoft.com/azure/dev-spaces/azure-dev-spaces) sono basati sui grafici Helm.

Helm è gestito dal [Cloud Native Computing Foundation (CNCF)](https://www.cncf.io/), in collaborazione con Microsoft, Google, Bitnami e la community di collaboratori di Helm.

Per altre informazioni sull'implementazione dei grafici Helm e Kubernetes, vedere il post che spiega come [usare i grafici Helm per distribuire eShopOnContainers nel servizio Azure Kubernetes](https://github.com/dotnet-architecture/eShopOnContainers/wiki/10.1-Deploying-to-AKS-using-Helm-Charts).

## <a name="use-azure-dev-spaces-for-your-kubernetes-application-lifecycle"></a>Usare Azure Dev Spaces per il ciclo di vita dell'applicazione Kubernetes

[Azure Dev Spaces](https://docs.microsoft.com/azure/dev-spaces/azure-dev-spaces) offre un'esperienza di sviluppo rapida e iterativa di Kubernetes per i team. Con una configurazione minima del computer di sviluppo, è possibile eseguire in modo iterativo ed eseguire il debug dei contenitori direttamente nel servizio Azure Kubernetes. Sviluppare in Windows, Mac o Linux usando strumenti familiari come Visual Studio, Visual Studio Code o la riga di comando.

Come accennato, Azure Dev Spaces usa i grafici Helm nella distribuzione delle applicazioni basate su contenitori.

Azure Dev Spaces aiuta i team di sviluppo a essere più produttivi in Kubernetes perché consente di eseguire rapidamente l'iterazione e il debug del codice direttamente in un cluster Kubernetes globale in Azure usando semplicemente Visual Studio 2017 o Visual Studio Code. Tale cluster Kubernetes in Azure è un cluster Kubernetes gestito condiviso, in modo che il team possa lavorare in modo collaborativo. È possibile sviluppare il codice in isolamento e quindi distribuirlo al cluster globale per poi eseguire test completi con altri componenti senza replicare o simulare le dipendenze.

Come illustrato nella figura 4-26, la funzionalità che si distingue maggiormente in Azure Dev Spaces è la capacità di creare "spazi" che possono essere eseguiti in modo integrato rispetto al resto della distribuzione globale nel cluster.

![Azure Dev Spaces può combinare in modo trasparente microservizi di produzione con istanze di contenitore di sviluppo, per testare facilmente le nuove versioni.](media/image38.png)

**Figura 4-26**. Uso di più spazi in Azure Dev Spaces

È possibile impostare uno spazio di sviluppo condiviso in Azure. Ogni sviluppatore può concentrarsi solo sulla parte assegnata dell'applicazione e sviluppare in modo iterativo il codice pre-commit in uno spazio di sviluppo che già contiene tutti gli altri servizi e le risorse del cloud da cui dipendono i suoi scenari. Le dipendenze sono sempre aggiornate e gli sviluppatori lavorano in un ambiente che rispecchia quello di produzione.

Azure Dev Spaces usa il concetto di spazio, che consente di lavorare in relativo isolamento e senza timore di inficiare il lavoro degli altri membri del team. Ogni spazio fa parte di una struttura gerarchica che consente di sostituire uno o più microservizi dello spazio master superiore con un proprio microservizio in corso.

Questa funzionalità è basata su prefissi di URL, quindi quando si usa un prefisso di spazio di Azure Dev Spaces nell'URL, una richiesta viene gestita dal microservizio di destinazione, se esiste nello spazio, altrimenti viene inoltrata alla prima istanza del microservizio di destinazione trovata nella gerarchia, fino ad arrivare eventualmente allo spazio master superiore.

Per un ottenere una visualizzazione pratica basata su un esempio concreto, vedere la [pagina wiki di eShopOnContainers su Azure Dev Spaces](https://github.com/dotnet-architecture/eShopOnContainers/wiki/10.1-Using-Azure-Dev-Spaces-and-AKS).

Per altre informazioni, vedere l'articolo sullo [sviluppo in team con Azure Dev Spaces](https://docs.microsoft.com/azure/dev-spaces/team-development-netcore).

## <a name="additional-resources"></a>Risorse aggiuntive

- **Introduzione al servizio Azure Kubernetes** \
  <https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal>

- **Azure Dev Spaces** \
  <https://docs.microsoft.com/azure/dev-spaces/azure-dev-spaces>

- **Kubernetes** Il sito ufficiale. \
  <https://kubernetes.io/>

>[!div class="step-by-step"]
>[Precedente](resilient-high-availability-microservices.md)
>[Successivo](using-azure-service-fabric.md)
