---
title: Sessioni, istanze e concorrenza
ms.date: 03/30/2017
ms.assetid: 50797a3b-7678-44ed-8138-49ac1602f35b
ms.openlocfilehash: 74b9971fa9267ef6156b27261c61d3e998d01883
ms.sourcegitcommit: c4e9d05644c9cb89de5ce6002723de107ea2e2c4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/19/2019
ms.locfileid: "65877324"
---
# <a name="sessions-instancing-and-concurrency"></a>Sessioni, istanze e concorrenza
Una *sessione* è una correlazione di tutti i messaggi inviati tra due endpoint. La*creazione di istanze* fa riferimento al controllo della durata di oggetti servizio definiti dall'utente e di oggetti <xref:System.ServiceModel.InstanceContext> correlati. La*concorrenza* è il termine dato al controllo del numero di thread in esecuzione contemporaneamente in un <xref:System.ServiceModel.InstanceContext> .  
  
 In questo argomento vengono descritte tali impostazioni, come utilizzarle e le varie interazioni tra di esse.  
  
## <a name="sessions"></a>Sessions  
 Quando la proprietà <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A?displayProperty=nameWithType> viene impostata da un contratto di servizio su <xref:System.ServiceModel.SessionMode.Required?displayProperty=nameWithType>, tutte le chiamate, ovvero gli scambi di messaggi sottostanti che supportano le chiamate, devono essere parte della stessa conversazione. Se un contratto consente sessioni ma non ne richiede, i client possono connettersi e possono stabilire o meno una sessione. Se, al termine della sessione, un messaggio viene inviato sullo stesso canale basato sulla sessione, viene generata un'eccezione.  
  
 Le sessioni WCF hanno le principali funzionalità concettuale seguente:  
  
- Vengono avviate e terminate esplicitamente dall'applicazione chiamante.  
  
- I messaggi recapitati durante una sessione vengono elaborati nell'ordine in cui vengono ricevuti.  
  
- Le sessioni consentono di correlare un gruppo di messaggi in una conversazione. Il significato di tale correlazione è un'astrazione. Un canale basato sulla sessione, ad esempio, è in grado di correlare messaggi basati su una connessione di rete condivisa mentre un altro canale basato sulla sessione è in grado di correlare messaggi basati su un tag condiviso nel corpo del messaggio. Le funzionalità che possono derivare dalla sessione dipendono dalla natura della correlazione.  
  
- Non vi è alcun archivio dati generale associato a una sessione WCF.  
  
 Se si ha familiarità con il <xref:System.Web.SessionState.HttpSessionState?displayProperty=nameWithType> classe nelle applicazioni ASP.NET e la funzionalità fornisce, è possibile notare le differenze seguenti tra quel tipo di sessioni e le sessioni WCF:  
  
- Le sessioni ASP.NET sono sempre avviate dal server.  
  
- Le sessioni ASP.NET sono implicitamente non ordinate.  
  
- Le sessioni ASP.NET forniscono un meccanismo di archiviazione dati generale in tutte le richieste.  
  
 Le applicazioni client e le applicazioni di servizio interagiscono con le sessioni in modi diversi. Le applicazioni client avviano sessioni e quindi ricevono ed elaborano i messaggi inviati all'interno della sessione. Le applicazioni di servizio possono utilizzare le sessioni come punto di estensibilità per aggiungere comportamenti. Ciò si ottiene utilizzando direttamente <xref:System.ServiceModel.InstanceContext> o implementando un provider di contesto dell'istanza personalizzato.  
  
## <a name="instancing"></a>creazione di istanze  
 Il comportamento delle istanze (impostato tramite la proprietà <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> ) consente di controllare come viene creato <xref:System.ServiceModel.InstanceContext> in risposta ai messaggi in ingresso. Per impostazione predefinita, ogni <xref:System.ServiceModel.InstanceContext> viene associato a uno oggetto servizio definito dall'utente. In questo modo l'impostazione (nel caso predefinito) della proprietà <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> consente di controllare anche le istanze di oggetti servizio definiti dall'utente. L'enumerazione <xref:System.ServiceModel.InstanceContextMode> definisce le modalità di istanza.  
  
 Sono disponibili le modalità di istanza seguenti:  
  
- <xref:System.ServiceModel.InstanceContextMode.PerCall>: Un nuovo <xref:System.ServiceModel.InstanceContext> (e pertanto l'oggetto servizio) viene creato per ogni richiesta del client.  
  
- <xref:System.ServiceModel.InstanceContextMode.PerSession>: Un nuovo <xref:System.ServiceModel.InstanceContext> (e pertanto l'oggetto servizio) viene creata per ogni sessione client nuova e mantenuta per tutta la durata della sessione (richiede un'associazione che supporti sessioni).  
  
- <xref:System.ServiceModel.InstanceContextMode.Single>: Un singolo <xref:System.ServiceModel.InstanceContext> (e pertanto l'oggetto servizio) gestisce tutte le richieste client per la durata dell'applicazione.  
  
 Nell'esempio di codice seguente viene illustrato il valore <xref:System.ServiceModel.InstanceContextMode> predefinito, con <xref:System.ServiceModel.InstanceContextMode.PerSession> impostato esplicitamente su una classe del servizio.  
  
```  
[ServiceBehavior(InstanceContextMode=InstanceContextMode.PerSession)]   
public class CalculatorService : ICalculatorInstance   
{   
    ...  
}  
```  
  
 Mentre la proprietà <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> consente di controllare la frequenza di rilascio di <xref:System.ServiceModel.InstanceContext> , le proprietà <xref:System.ServiceModel.OperationBehaviorAttribute.ReleaseInstanceMode%2A?displayProperty=nameWithType> e <xref:System.ServiceModel.ServiceBehaviorAttribute.ReleaseServiceInstanceOnTransactionComplete%2A?displayProperty=nameWithType> consentono di controllare quando l'oggetto servizio viene rilasciato.  
  
### <a name="well-known-singleton-services"></a>Servizi Singleton noti  
 Una variazione su oggetti servizio di una singola istanza può a volte essere utile. È infatti possibile creare un oggetto servizio e quindi creare l'host del servizio tramite quell'oggetto. A tale scopo, è necessario impostare la proprietà <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> su <xref:System.ServiceModel.InstanceContextMode.Single> , in caso contrario verrà generata un'eccezione quando l'host del servizio viene aperto.  
  
 Utilizzare il costruttore <xref:System.ServiceModel.ServiceHost.%23ctor%28System.Object%2CSystem.Uri%5B%5D%29?displayProperty=nameWithType> per creare tale servizio. Fornisce un'alternativa all'implementazione di un'interfaccia <xref:System.ServiceModel.Dispatcher.IInstanceContextInitializer?displayProperty=nameWithType> personalizzata quando si desidera fornire un'istanza specifica dell'oggetto utilizzabile da un servizio singleton. Questo overload può risultare utile quando il tipo di implementazione del servizio è di difficile costruzione, ad esempio se non implementa alcun costruttore pubblico predefinito privo di parametri.  
  
 Si noti che quando un oggetto viene fornito a questo costruttore, alcune funzionalità correlate a Windows Communication Foundation (WCF) comportamento di istanza funzionano in modo diverso. La chiamata, ad esempio, di <xref:System.ServiceModel.InstanceContext.ReleaseServiceInstance%2A?displayProperty=nameWithType> non ha effetto quando viene fornita l'istanza di un oggetto Singleton. Analogamente, qualsiasi altro meccanismo di rilascio delle istanze viene ignorato. L'host <xref:System.ServiceModel.ServiceHost> si comporta sempre come se la proprietà <xref:System.ServiceModel.OperationBehaviorAttribute.ReleaseInstanceMode%2A?displayProperty=nameWithType> fosse impostata su <xref:System.ServiceModel.ReleaseInstanceMode.None?displayProperty=nameWithType> per tutte le operazioni.  
  
### <a name="sharing-instancecontext-objects"></a>Condivisione di oggetti InstanceContext  
 È inoltre possibile controllare l'associazione tra canali o chiamate con sessione e oggetti <xref:System.ServiceModel.InstanceContext> eseguendo quell'associazione.  
  
## <a name="concurrency"></a>concorrenza  
 La concorrenza è il controllo del numero di thread attivi in un <xref:System.ServiceModel.InstanceContext> contemporaneamente. Viene controllato tramite <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A?displayProperty=nameWithType> con l'enumerazione <xref:System.ServiceModel.ConcurrencyMode> .  
  
 Sono disponibili le tre modalità di concorrenza seguenti:  
  
- <xref:System.ServiceModel.ConcurrencyMode.Single>: Ogni contesto dell'istanza può avere un massimo di un thread di elaborazione dei messaggi nel contesto dell'istanza alla volta. Altri thread che devono utilizzare lo stesso contesto dell'istanza, devono restare bloccati fino alla chiusura del thread originale dal contesto dell'istanza.  
  
- <xref:System.ServiceModel.ConcurrencyMode.Multiple>: Ogni istanza del servizio può avere più thread che elaborano i messaggi contemporaneamente. Per essere in grado di usare questa modalità di concorrenza, l'implementazione del servizio deve essere thread-safe.  
  
- <xref:System.ServiceModel.ConcurrencyMode.Reentrant>: Ogni istanza del servizio elabora uno messaggio alla volta ma accetta chiamate di operazioni rientranti. Il servizio accetta queste chiamate solo quando esegue chiamate in uscita tramite un oggetto client WCF.  
  
> [!NOTE]
>  Può essere difficile comprendere, scrivere correttamente e sviluppare codice che utilizza in modo sicuro più di un thread. Prima di utilizzare valori <xref:System.ServiceModel.ConcurrencyMode.Multiple> o <xref:System.ServiceModel.ConcurrencyMode.Reentrant> , verificare che il servizio sia progettato correttamente per queste modalità. Per altre informazioni, vedere <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A>.  
  
 L'uso della concorrenza è correlato alla modalità di istanza. Nelle <xref:System.ServiceModel.InstanceContextMode.PerCall> delle istanze, la concorrenza non è rilevante, perché ogni messaggio viene elaborato da un nuovo <xref:System.ServiceModel.InstanceContext> e, pertanto mai più di un thread è attivo nel <xref:System.ServiceModel.InstanceContext>.  
  
 Nell'esempio di codice seguente viene illustrata l'impostazione della proprietà <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A> su <xref:System.ServiceModel.ConcurrencyMode.Multiple>.  
  
```  
[ServiceBehavior(ConcurrencyMode=ConcurrencyMode.Multiple, InstanceContextMode = InstanceContextMode.Single)]   
public class CalculatorService : ICalculatorConcurrency   
{   
    ...  
}  
```  
  
## <a name="sessions-interact-with-instancecontext-settings"></a>Sessioni che interagiscono con impostazioni InstanceContext  
 L'interazione di sessioni e di <xref:System.ServiceModel.InstanceContext> , che dipendono dalla combinazione tra il valore dell'enumerazione <xref:System.ServiceModel.SessionMode> in un contratto e la proprietà <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> nell'implementazione del servizio, consente di controllare l'associazione tra canali e oggetti servizio specifici.  
  
 Nella tabella seguente viene mostrato il risultato di un canale in ingresso in cui le sessioni sono supportate o non supportate. Tale risultato è in funzione della combinazione dei valori delle proprietà <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A?displayProperty=nameWithType> e <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> del servizio.  
  
|Valore InstanceContextMode|<xref:System.ServiceModel.SessionMode.Required>|<xref:System.ServiceModel.SessionMode.Allowed>|<xref:System.ServiceModel.SessionMode.NotAllowed>|  
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|  
|PerCall|-Comportamento di canale con sessione: Una sessione e <xref:System.ServiceModel.InstanceContext> per ogni chiamata.<br />-Comportamento di canale senza sessione: Viene generata un'eccezione.|-Comportamento di canale con sessione: Una sessione e <xref:System.ServiceModel.InstanceContext> per ogni chiamata.<br />-Comportamento di canale senza sessione: Un <xref:System.ServiceModel.InstanceContext> per ogni chiamata.|-Comportamento di canale con sessione: Viene generata un'eccezione.<br />-Comportamento di canale senza sessione: Un <xref:System.ServiceModel.InstanceContext> per ogni chiamata.|  
|PerSession|-Comportamento di canale con sessione: Una sessione e <xref:System.ServiceModel.InstanceContext> per ogni canale.<br />-Comportamento di canale senza sessione: Viene generata un'eccezione.|-Comportamento di canale con sessione: Una sessione e <xref:System.ServiceModel.InstanceContext> per ogni canale.<br />-Comportamento di canale senza sessione: Un <xref:System.ServiceModel.InstanceContext> per ogni chiamata.|-Comportamento di canale con sessione: Viene generata un'eccezione.<br />-Comportamento di canale senza sessione: Un <xref:System.ServiceModel.InstanceContext> per ogni chiamata.|  
|Single|-Comportamento di canale con sessione: Una sessione e un solo <xref:System.ServiceModel.InstanceContext> per tutte le chiamate.<br />-Comportamento di canale senza sessione: Viene generata un'eccezione.|-Comportamento di canale con sessione: Una sessione e <xref:System.ServiceModel.InstanceContext> per il singleton creato o specificato dall'utente.<br />-Comportamento di canale senza sessione: Un <xref:System.ServiceModel.InstanceContext> per il singleton creato o specificato dall'utente.|-Comportamento di canale con sessione: Viene generata un'eccezione.<br />-Comportamento di canale senza sessione: Un <xref:System.ServiceModel.InstanceContext> per ogni singleton creato o per il singleton specificato dall'utente.|  
  
## <a name="see-also"></a>Vedere anche

- [Uso di sessioni](../../../../docs/framework/wcf/using-sessions.md)
- [Procedura: Creare un servizio che richiede sessioni](../../../../docs/framework/wcf/feature-details/how-to-create-a-service-that-requires-sessions.md)
- [Procedura: Controllare le istanze del servizio](../../../../docs/framework/wcf/feature-details/how-to-control-service-instancing.md)
- [Concorrenza](../../../../docs/framework/wcf/samples/concurrency.md)
- [Creazione di istanze](../../../../docs/framework/wcf/samples/instancing.md)
- [Sessione](../../../../docs/framework/wcf/samples/session.md)
