---
title: Modelli di Visual Studio WCF
ms.date: 03/30/2017
ms.assetid: 6a608575-3535-4190-89da-911e24c8374f
ms.openlocfilehash: 2e192e671d37e096e4199b295d4f533194ab89b6
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64613225"
---
# <a name="wcf-visual-studio-templates"></a>Modelli di Visual Studio WCF
Modelli di Visual Studio di Windows Communication Foundation (WCF) sono progetto predefiniti e modelli di elementi che è possibile usare in Visual Studio per compilare rapidamente i servizi WCF e le relative applicazioni.  
  
## <a name="using-the-wcf-templates"></a>Utilizzo dei modelli di WCF  
 Modelli di Visual Studio WCF offrono una struttura di classe di base per lo sviluppo del servizio. In particolare, questi modelli forniscono le definizioni di base per il contratto di servizio, contratto dati, implementazione del servizio e configurazione. È possibile utilizzare questi modelli per creare un semplice servizio con interazione di codice minima e come blocco predefinito per servizi più avanzati.  
  
### <a name="wcf-service-library-project-template"></a>Modello di progetto della libreria di servizi WCF  
 Il modello di progetto libreria di servizi WCF è disponibile nella finestra di dialogo Nuovo progetto sotto **Visual C# \WCF** e **Visual Basic\WCF**.  
  
 Quando si crea un nuovo progetto usando il **servizio WCF** modello, il nuovo progetto include automaticamente i tre file seguenti:  
  
- File di contratto del servizio (IService1.cs o IService1.vb). Il file di contratto di servizio è un'interfaccia che ha gli attributi del servizio WCF applicati. Questo file fornisce una definizione di un semplice servizio per indicare come definire i servizi e include operazioni basate su parametri e un semplice esempio di contratto dati. Questo è il file predefinito visualizzato nell'editor del codice dopo la creazione di un progetto di servizio WCF.  
  
- File di implementazione del servizio (Service1.cs o Service1.vb). Il file di implementazione del servizio implementa il contratto definito nel file del contratto di servizio.  
  
- File di configurazione dell'applicazione (App.config). Il file di configurazione fornisce gli elementi di base di un modello di servizio WCF con un'associazione HTTP protetta. Include anche un endpoint per il servizio e abilita scambio di metadati.  
  
> [!NOTE]
>  Visual Studio è configurato per riconoscere il file app. config come file di configurazione per il progetto quando viene eseguita usando il [Host del servizio WCF (WcfSvcHost.exe)](../../../docs/framework/wcf/wcf-service-host-wcfsvchost-exe.md), ovvero la configurazione predefinita. Se la libreria di servizi viene inserita in un eseguibile, è necessario spostare il codice di configurazione nel file di configurazione dell'eseguibile, in quanto i file di configurazione per le DLL non sono validi.  
  
### <a name="wcf-service-application-template"></a>Modello Applicazione di servizio WCF  
 Il modello di applicazione di servizio WCF è disponibile nella finestra di dialogo Nuovo progetto sotto **Visual C# \WCF** e **Visual Basic\WCF**.  
  
 Quando si crea un nuovo progetto usando il **servizio applicazione Web WCF** modello, il progetto include i quattro file seguenti:  
  
- File host del servizio (service1.svc).  
  
- File di contratto del servizio (IService1.cs o IService1.vb).  
  
- File di implementazione del servizio (Service1.svc.cs o Service1.svc.vb).  
  
- File di configurazione Web (Web.config).  
  
 Il modello crea automaticamente un sito Web (da distribuire in una directory virtuale) e funge da host di un servizio.  
  
### <a name="wcf-web-site-template"></a>Modello sito Web del servizio WCF  
 Il modello di sito Web di WCF è disponibile nella finestra di dialogo Nuovo progetto sotto **Visual C# \Web site\wcf Service** e **Visual Basic\Web Site\WCF Service**. In questo modo vengono creati gli stessi file del modello Applicazione di servizio WCF che viene però organizzato come se fosse un sito Web ASP.NET. Vengono create le cartelle App_Data e App_Code.  
  
### <a name="wcf-service-item-template"></a>Modello di elemento del servizio WCF  
 Il modello di elemento di servizio WCF è un modello personalizzato che offre un modo rapido per aggiungere i servizi WCF per i progetti di Visual Studio esistenti.  
  
 Per usare questo modello, vedere la **Esplora soluzioni** riquadro, fare clic sul nome del progetto, scegliere **Add**e quindi fare clic su **nuovo elemento** per avviare il **Aggiungi nuovo Elemento** nella finestra di dialogo.  
  
 L'interfaccia del servizio e file di implementazione vengono inseriti nella cartella radice del progetto.  
  
 Il modello tenterà di unire la sezione di configurazione del nuovo servizio con il file di configurazione esistente, nel caso siano compatibili.  
  
 Se il progetto esistente è un progetto Web viene creato anche un file del host del servizio (service1.svc).  
  
### <a name="wcf-wf-service-project-and-item-template"></a>Modello di elemento e progetto di servizio WF WCF.  
 Questi modelli creano servizi WCF che ospitano un servizio del flusso di lavoro, ovvero un flusso di lavoro che è accessibile come un servizio web. Sono disponibili modelli separati per XAML o modelli di programmazione imperativi. Mediante i modelli, è possibile creare un flusso di lavoro sequenziale o di macchina a stati. Per altre informazioni su questi tipi di flusso di lavoro, vedere [come: Creare un flusso di lavoro](../windows-workflow-foundation/how-to-create-a-workflow.md). Per altre informazioni sulla creazione di progetti di flusso di lavoro, vedere [creazione di progetti di flusso di lavoro Legacy](/visualstudio/workflow-designer/creating-legacy-workflow-projects).  
  
 Progettazione di Visual Studio è più reattiva quando vengono usati i flussi di lavoro di tipo XOML invece di codice basato su quelle. XOML è il tipo di flusso predefinito che viene creato.  
  
### <a name="wcf-syndication-service-library-template"></a>Modello Libreria di servizi di diffusione WCF  
 Questo modello consente di esporre il feed in formato RSS o ATOM come servizio WCF. Per altre informazioni, vedere [diffusione WCF](../../../docs/framework/wcf/feature-details/wcf-syndication.md).  
  
#### <a name="changing-the-address-of-the-feed"></a>Modifica dell'indirizzo del feed  
 Il modello di pubblicazione utilizza Internet Explorer durante l'esecuzione. Quando si pulsante destro del mouse sul progetto in **Esplora soluzioni** in Visual Studio, selezionare **delle proprietà**, quindi selezionare il **Debug** tab ed è possibile visualizzare l'indirizzo predefinito del modello. Internet Explorer tenterà di aprire il feed a questo indirizzo.  
  
 Se si modifica l'indirizzo del proprio feed, è necessario modificare anche l'indirizzo nel **Debug** scheda. In caso contrario, Internet Explorer tenterà di aprire il feed all'indirizzo predefinito e l'operazione avrà esito negativo.  
  
### <a name="ajax-enabled-wcf-service-item-template"></a>Modello di elemento del servizio WCF con supporto AJAX  
 Questo modello espone un controllo AJAX come servizio WCF. Per altre informazioni sui controlli AJAX, vedere la [documentazione sul controllo JAX](https://go.microsoft.com/fwlink/?LinkId=96717).  
  
### <a name="silverlight-enabled-wcf-service-item-template"></a>Modello di elemento del servizio WCF con supporto Silverlight  
 Questo modello consente di creare un servizio Web che fornisce dati a un client Silverlight o front-end. Il modello può essere aggiunto a un progetto di applicazione Web o sito Web per creare un servizio WCF, che include codice del servizio e la configurazione che supportano la comunicazione con un client Silverlight. È quindi possibile usare **Aggiungi riferimento al servizio** per aggiungere un proxy client del servizio al client e scambiare dati tra il client Silverlight e il servizio WCF abilitato per Silverlight.  
  
 Per accedere a questo modello, fare doppio clic su un progetto di applicazione Web o sito Web in **Esplora soluzioni**, fare clic su **aggiungere un nuovo elemento**, fare clic su **servizio WCF abilitato per Silverlight**.  
  
> [!NOTE]
>  Il servizio WCF con supporto Silverlight espone un endpoint `basicHttpBinding` senza abilitare alcuna impostazione di sicurezza. Le informazioni sul servizio possono pertanto essere recuperate da tutti i client che si connettono a questo servizio. I messaggi scambiati tra il servizio e il client non sono inoltre firmati o crittografati. Per proteggere l'endpoint in modo appropriato, è necessario utilizzare l'autenticazione ASP.NET, HTTPS o altri meccanismi.  
  
## <a name="see-also"></a>Vedere anche

- [Host del servizio WCF (WcfSvcHost.exe)](../../../docs/framework/wcf/wcf-service-host-wcfsvchost-exe.md)
- [Client di prova WCF (WcfTestClient.exe)](../../../docs/framework/wcf/wcf-test-client-wcftestclient-exe.md)
