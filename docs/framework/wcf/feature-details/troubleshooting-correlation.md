---
title: Risoluzione dei problemi relativi alla correlazione
ms.date: 03/30/2017
ms.assetid: 98003875-233d-4512-a688-4b2a1b0b5371
ms.openlocfilehash: fecfaf7374823bb19a4ad3d7f6cb2dbbdf139703
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61932821"
---
# <a name="troubleshooting-correlation"></a>Risoluzione dei problemi relativi alla correlazione
La correlazione viene utilizzata per correlare i messaggi del servizio flusso di lavoro l'uno all'altro e all'istanza del flusso di lavoro corretta, ma se non viene configurata correttamente, i messaggi non verranno ricevuti e le applicazioni non funzioneranno in modo appropriato. In questo argomento viene fornita una panoramica dei metodi che consentono di risolvere i problemi relativi alla correlazione e vengono inoltre descritti alcuni dei problemi comuni che possono verificarsi durante l'utilizzo della correlazione.

## <a name="handle-the-unknownmessagereceived-event"></a>Gestire l'evento UnknownMessageReceived
 L'evento <xref:System.ServiceModel.ServiceHostBase.UnknownMessageReceived> si verifica quando un messaggio sconosciuto viene ricevuto da un servizio, inclusi i messaggi che non possono essere correlati a un'istanza esistente. Per i servizi indipendenti, questo evento può essere gestito nell'applicazione host.

```csharp
host.UnknownMessageReceived += delegate(object sender, UnknownMessageReceivedEventArgs e)
{
    Console.WriteLine("Unknown Message Received:");
    Console.WriteLine(e.Message);
};
```

 Per i servizi ospitati sul Web, questo evento può essere gestito derivando una classe da <xref:System.ServiceModel.Activities.Activation.WorkflowServiceHostFactory> ed eseguendo l'override del metodo <xref:System.ServiceModel.Activities.Activation.WorkflowServiceHostFactory.CreateWorkflowServiceHost%2A>.

```csharp
class CustomFactory : WorkflowServiceHostFactory
{
    protected override WorkflowServiceHost CreateWorkflowServiceHost(Activity activity, Uri[] baseAddresses)
    {
        // Create the WorkflowServiceHost.
        WorkflowServiceHost host = new WorkflowServiceHost(activity, baseAddresses);

        // Handle the UnknownMessageReceived event.
        host.UnknownMessageReceived += delegate(object sender, UnknownMessageReceivedEventArgs e)
        {
            Console.WriteLine("Unknown Message Received:");
            Console.WriteLine(e.Message);
        };

        return host;
    }
}
```

 Questo oggetto <xref:System.ServiceModel.Activities.Activation.WorkflowServiceHostFactory> personalizzato può essere quindi specificato nel file `svc` per il servizio.

```
<% @ServiceHost Language="C#" Service="OrderServiceWorkflow" Factory="CustomFactory" %>
```

 Il richiamo di questo gestore consente di recuperare il messaggio tramite la proprietà <xref:System.ServiceModel.UnknownMessageReceivedEventArgs.Message%2A> di <xref:System.ServiceModel.UnknownMessageReceivedEventArgs> e sarà simile al messaggio seguente:

```Output
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Header>
    <To s:mustUnderstand="1" xmlns="http://schemas.microsoft.com/ws/2005/05/addressing/none">http://localhost:8080/OrderService</To>
    <Action s:mustUnderstand="1" xmlns="http://schemas.microsoft.com/ws/2005/05/addressing/none">http://tempuri.org/IService/AddItem</Action>
  </s:Header>
  <s:Body>
    <AddItem xmlns="http://tempuri.org/">
      <Item>Books</Item>
    </AddItem>
  </s:Body>
</s:Envelope>
```

 L'ispezione dei messaggi inviati al gestore <xref:System.ServiceModel.ServiceHostBase.UnknownMessageReceived> può fornire indicazioni sui motivi della mancata correlazione del messaggio a un'istanza del servizio flusso di lavoro.

## <a name="use-tracking-to-monitor-the-progress-of-the-workflow"></a>Utilizzare il rilevamento per monitorare lo stato di avanzamento del flusso di lavoro
 Il rilevamento consente di monitorare lo stato di avanzamento del flusso di lavoro. Per impostazione predefinita, vengono generati record di rilevamento per eventi del ciclo di vita di flusso del lavoro, eventi del ciclo di vita delle attività, propagazione degli errori e ripresa dei segnalibri. Record di rilevamento personalizzati possono inoltre essere generati da attività personalizzate. Durante la risoluzione dei problemi relativi alla correlazione, i record di rilevamento delle attività, di ripresa dei segnalibri e di propagazione degli errori costituiscono gli elementi più utili. I record di rilevamento delle attività possono essere utilizzati per determinare lo stato di avanzamento corrente del flusso di lavoro e consentono di identificare l'attività di messaggistica attualmente in attesa dei messaggi. I record di ripresa dei segnalibri sono utili perché indicano la ricezione di un messaggio da parte del flusso di lavoro, mentre i record di propagazione degli errori rendono disponibile un record degli errori presenti nel flusso di lavoro. Per abilitare il rilevamento, specificare l'oggetto <xref:System.Activities.Tracking.TrackingParticipant> desiderato nella proprietà <xref:System.ServiceModel.Activities.WorkflowServiceHost.WorkflowExtensions%2A> di <xref:System.ServiceModel.Activities.WorkflowServiceHost>. Nell'esempio seguente, il `ConsoleTrackingParticipant` (dal [rilevamento personalizzato](../../../../docs/framework/windows-workflow-foundation/samples/custom-tracking.md) esempio) viene configurata tramite il profilo di rilevamento predefinito.

```csharp
host.WorkflowExtensions.Add(new ConsoleTrackingParticipant());
```

 Un partecipante del rilevamento, quale ConsoleTrackingParticipant, è utile per i servizi flusso di lavoro indipendenti che dispongono di una finestra della console. Per un servizio ospitato sul Web, è necessario usare un partecipante del rilevamento che registri le informazioni di rilevamento in un archivio durevole, ad esempio l'elemento predefinito <xref:System.Activities.Tracking.EtwTrackingParticipant>, oppure un partecipante del rilevamento personalizzato che registra le informazioni in un file.

 Per altre informazioni sul rilevamento e configurazione del rilevamento per un servizio del flusso di lavoro ospitato sul Web, vedere [flusso di lavoro di rilevamento e traccia](../../../../docs/framework/windows-workflow-foundation/workflow-tracking-and-tracing.md), [configurazione del rilevamento per un flusso di lavoro](../../../../docs/framework/windows-workflow-foundation/configuring-tracking-for-a-workflow.md)e il [ Rilevamento &#91;esempi di WF&#93; ](../../../../docs/framework/windows-workflow-foundation/samples/tracking.md) campioni.

## <a name="use-wcf-tracing"></a>Utilizzare la funzionalità di traccia di WCF
 La funzionalità di traccia di WCF consente di tracciare il flusso di messaggi da e verso un servizio flusso di lavoro. Queste informazioni di traccia sono utili per risolvere i problemi relativi alla correlazione, in particolare per la correlazione basata sul contenuto. Per abilitare la traccia, specificare i listener di traccia desiderati nella sezione `system.diagnostics` del file `web.config` se il servizio flusso di lavoro è ospitato sul Web oppure nel file `app.config` se il servizio flusso di lavoro è indipendente. Per includere il contenuto dei messaggi nel file di traccia, specificare `true` per `logEntireMessage` nell'elemento `messageLogging` nella sezione `diagnostics` di `system.serviceModel`. Nell'esempio seguente le informazioni di analisi, incluso il contenuto dei messaggi, sono configurate in modo da essere scritte in un file denominato `service.svclog`.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.diagnostics>
    <sources>
      <source name="System.ServiceModel" switchValue="Information" propagateActivity="true">
        <listeners>
          <add name="corr"/>
        </listeners>
      </source>
      <source name="System.ServiceModel.MessageLogging">
        <listeners>
          <add name="corr"/>
        </listeners>
      </source>
    </sources>

    <sharedListeners>
      <add name="corr" type="System.Diagnostics.XmlWriterTraceListener" initializeData="c:\logs\service.svclog">
      </add>
    </sharedListeners>
  </system.diagnostics>

  <system.serviceModel>
    <diagnostics>
      <messageLogging logEntireMessage="true" logMalformedMessages="false"
         logMessagesAtServiceLevel="false" logMessagesAtTransportLevel="true" maxSizeOfMessageToLog="2147483647">
      </messageLogging>
    </diagnostics>
  </system.serviceModel>
</configuration>
```

 Per visualizzare le informazioni di traccia che sono inclusa nello `service.svclog`, il [strumento Service Trace Viewer (SvcTraceViewer.exe)](../../../../docs/framework/wcf/service-trace-viewer-tool-svctraceviewer-exe.md) viene usato. Questa soluzione è particolarmente utile per risolvere i problemi relativi alla correlazione basata sul contenuto, in quanto consente di visualizzare il contenuto del messaggio e di vedere esattamente ciò che viene passato, nonché di determinarne la corrispondenza a <xref:System.ServiceModel.CorrelationQuery> per la correlazione basata sul contenuto. Per ulteriori informazioni sulla traccia di WCF, vedere [strumento Service Trace Viewer (SvcTraceViewer.exe)](../../../../docs/framework/wcf/service-trace-viewer-tool-svctraceviewer-exe.md), [Configuring Tracing](../../../../docs/framework/wcf/diagnostics/tracing/configuring-tracing.md), e [utilizzando traccia per risolvere i problemi dell'applicazione](../../../../docs/framework/wcf/diagnostics/tracing/using-tracing-to-troubleshoot-your-application.md).

## <a name="common-context-exchange-correlation-issues"></a>Problemi comuni relativi alla correlazione di scambio del contesto
 Per determinati tipi di correlazione, è necessario che venga associato un tipo specifico di associazione affinché la correlazione funzioni correttamente. Alcuni esempi includono la correlazione request/reply che richiede un'associazione bidirezionale, quale <xref:System.ServiceModel.BasicHttpBinding>, e la correlazione di scambio del contesto per la quale è necessaria un'associazione basata sul contesto, quale <xref:System.ServiceModel.BasicHttpContextBinding>. La maggior parte delle associazioni supporta operazioni bidirezionali, pertanto questo non rappresenta un problema comune per la correlazione request/reply, mentre è disponibile solo un numero limitato di associazioni basate sul contesto, tra cui <xref:System.ServiceModel.BasicHttpContextBinding>, <xref:System.ServiceModel.WSHttpContextBinding> e <xref:System.ServiceModel.NetTcpContextBinding>. Se non viene utilizzata una di queste associazioni, la chiamata iniziale a un servizio flusso di lavoro avrà esito positivo, ma le chiamate successive non riusciranno generando l'oggetto <xref:System.ServiceModel.FaultException>.

```Output
There is no context attached to the incoming message for the service
and the current operation is not marked with "CanCreateInstance = true".
In order to communicate with this service check whether the incoming binding
supports the context protocol and has a valid context initialized.
```

 Le informazioni sul contesto utilizzate per la correlazione del contesto possono essere restituite da <xref:System.ServiceModel.Activities.SendReply> all'attività <xref:System.ServiceModel.Activities.Receive> che inizializza la correlazione del contesto in caso di utilizzo di un'operazione bidirezionale oppure essere specificate dal chiamante se l'operazione è unidirezionale. Se il contesto non viene inviato dal chiamante o non viene restituito dal servizio flusso di lavoro, verrà restituito lo stesso oggetto <xref:System.ServiceModel.FaultException> descritto in precedenza quando viene richiamata un'operazione successiva.

## <a name="common-request-reply-correlation-issues"></a>Problemi comuni relativi alla correlazione request/reply
 Correlazione request / reply viene usata con un <xref:System.ServiceModel.Activities.Receive> / <xref:System.ServiceModel.Activities.SendReply> coppia per implementare un'operazione bidirezionale in un servizio del flusso di lavoro e con un <xref:System.ServiceModel.Activities.Send> / <xref:System.ServiceModel.Activities.ReceiveReply> coppia che richiama un'operazione bidirezionale in un altro Web servizio. Quando si richiama un'operazione bidirezionale in un servizio WCF, il servizio può essere una tradizionale servizio WCF basato su codice imperativo o può essere un servizio del flusso di lavoro. Per utilizzare la correlazione request/reply, è necessario utilizzare un'associazione bidirezionale, ad esempio <xref:System.ServiceModel.BasicHttpBinding>, e che le operazioni siano bidirezionali.

 Se il servizio del flusso di lavoro esegue operazioni bidirezionali in parallelo o la sovrapposizione <xref:System.ServiceModel.Activities.Receive> / <xref:System.ServiceModel.Activities.SendReply> oppure <xref:System.ServiceModel.Activities.Send> / <xref:System.ServiceModel.Activities.ReceiveReply> gestione coppie, quindi la correlazione implicita fornita da <xref:System.ServiceModel.Activities.WorkflowServiceHost>potrebbe non essere sufficiente, specialmente negli scenari intensivo, e i messaggi non possono essere indirizzati in modo corretto. Per evitare che si verifichi questo problema, è consigliabile specificare sempre in modo esplicito un <xref:System.ServiceModel.Activities.CorrelationHandle> quando si utilizza la correlazione request/reply. Quando si usa la **SendAndReceiveReply** e **ReceiveAndSendReply** modelli nella sezione messaggi della **della casella degli strumenti** nella finestra di progettazione del flusso di lavoro, un <xref:System.ServiceModel.Activities.CorrelationHandle> è configurato in modo esplicito per impostazione predefinita. Durante la compilazione di un flusso di lavoro tramite codice, l'oggetto <xref:System.ServiceModel.Activities.CorrelationHandle> viene specificato nella proprietà <xref:System.ServiceModel.Activities.Receive.CorrelationInitializers%2A> della prima attività nella coppia. Nell'esempio seguente un'attività <xref:System.ServiceModel.Activities.Receive> viene configurata con una proprietà <xref:System.ServiceModel.Activities.CorrelationInitializer.CorrelationHandle%2A> esplicita specificata in <xref:System.ServiceModel.Activities.RequestReplyCorrelationInitializer>.

```csharp
Variable<CorrelationHandle> RRHandle = new Variable<CorrelationHandle>();

Receive StartOrder = new Receive
{
    CanCreateInstance = true,
    ServiceContractName = OrderContractName,
    OperationName = "StartOrder",
    CorrelationInitializers =
    {
        new RequestReplyCorrelationInitializer
        {
            CorrelationHandle = RRHandle
        }
    }
};

SendReply ReplyToStartOrder = new SendReply
{
    Request = StartOrder,
    Content = ... // Contains the return value, if any.
};

// Construct a workflow using StartOrder and ReplyToStartOrder.
```

 La persistenza non è consentita tra un <xref:System.ServiceModel.Activities.Receive> / <xref:System.ServiceModel.Activities.SendReply> coppia o un' <xref:System.ServiceModel.Activities.Send> / <xref:System.ServiceModel.Activities.ReceiveReply> coppia. Viene creata un'area di non persistenza che dura fino a quando non vengono completate entrambe le attività. Se un'attività, ad esempio un'attività di ritardo si trova in quest'area di non persistenza e determina che il flusso di lavoro diventi inattivo, tale flusso di lavoro non verrà conservato anche se l'host è configurato per rendere persistenti i flussi di lavoro quando diventano inattivi. Se un'attività, ad esempio un'attività Persist tenta di eseguire la persistenza in modo esplicito nell'area di non persistenza, viene generata un'eccezione irreversibile, il flusso di lavoro viene interrotto e al chiamante viene restituita un'eccezione <xref:System.ServiceModel.FaultException>. Il messaggio di eccezione irreversibile è "System. InvalidOperationException: Rendere persistenti le attività non possono essere contenute blocchi di non persistenza. ". Questa eccezione non viene restituita al chiamante ma può essere osservata se il rilevamento è abilitato. Il messaggio dell'eccezione <xref:System.ServiceModel.FaultException> restituita al chiamante è "WorkflowInstance '5836145b-7da2-49d0-a052-a49162adeab6' è stata completata. Impossibile eseguire l'operazione".

 Per altre informazioni sulla correlazione request / reply, vedere [Request / Reply](../../../../docs/framework/wcf/feature-details/request-reply-correlation.md).

## <a name="common-content-correlation-issues"></a>Problemi comuni relativi alla correlazione di contenuto
 La correlazione basata sul contenuto viene utilizzata quando un servizio flusso di lavoro riceve più messaggi e una parte dei dati inclusi nei messaggi scambiati identifica l'istanza desiderata. La correlazione basata sul contenuto utilizza questi dati nel messaggio, ad esempio un numero cliente o un ID dell'ordine, per indirizzare messaggi all'istanza del flusso di lavoro corretta. Contenuto della sezione vengono descritti alcuni problemi comuni che possono verificarsi durante l'utilizzo della correlazione basata sul contenuto.

### <a name="ensure-the-identifying-data-is-unique"></a>Assicurarsi che i dati di identificazione siano univoci
 Per i dati usati per l'identificazione dell'istanza viene generato un hash in una chiave di correlazione. È necessario verificare che i dati utilizzati per la correlazione siano univoci. In caso contrario, potrebbero verificarsi conflitti nella chiave con hash ed è possibile che i messaggi vengano indirizzati in modo errato. Una correlazione basata esclusivamente su un nome di cliente può ad esempio generare un conflitto, poiché possono esistere più clienti con lo stesso nome. Non utilizzare i due punti (:) come parte dei dati utilizzati per correlare il messaggio, in quanto vengono già utilizzati per delimitare il valore e la chiave della query del messaggio per formattare la stringa per la quale verrà generato un hash. Se si utilizza la persistenza, verificare che i dati di identificazione correnti non siano stati utilizzati da un'istanza persistente precedente. La disabilitazione temporanea della persistenza può contribuire a identificare il problema. La funzionalità di traccia di WCF può essere utilizzata per visualizzare la chiave di correlazione calcolata ed essere utile per eseguire il debug di questo tipo di problema.

### <a name="race-conditions"></a>Condizioni di traccia
 Tra la ricezione di un messaggio da parte del servizio e il momento in cui la correlazione viene effettivamente inizializzata si verifica un breve intervallo, durante il quale i messaggi seguenti verranno ignorati. Se un servizio flusso di lavoro inizializza la correlazione basata sul contenuto tramite dati passati dal client su un'operazione unidirezionale e il chiamante invia immediatamente messaggi successivi, questi messaggi verranno ignorati durante questo intervallo. È possibile evitare questo problema utilizzando un'operazione bidirezionale per inizializzare la correlazione o un oggetto <xref:System.ServiceModel.Activities.TransactedReceiveScope>.

### <a name="correlation-query-issues"></a>Problemi relativi alle query di correlazione
 Le query di correlazione consentono di specificare i dati di un messaggio da utilizzare per la correlazione del messaggio stesso. Questi dati vengono specificati tramite una query XPath. Se a un servizio non vengono inviati messaggi anche se tutto sembra corretto, una strategia per la risoluzione dei problemi consiste nel specificare un valore letterale corrispondente al valore dei dati del messaggio anziché una query XPath. Per specificare un valore letterale, utilizzare la funzione `string`. Nell'esempio seguente viene configurato un oggetto <xref:System.ServiceModel.MessageQuerySet> per l'utilizzo del valore letterale `11445` per `OrderId` e la query XPath viene impostata come commento.

```csharp
MessageQuerySet = new MessageQuerySet
{
    {
        "OrderId",
        //new XPathMessageQuery("sm:body()/tempuri:StartOrderResponse/tempuri:OrderId")
        new XPathMessageQuery("string('11445')")
    }
}
```

 Se una query XPath è configurata in modo non corretto in modo che nessun dato di correlazione viene recuperato, viene restituito un errore con messaggio analogo al seguente: "Una query di correlazione ha restituito un set di risultati vuoto. Assicurarsi che le query di correlazione dell'endpoint siano configurate correttamente". Un metodo rapido per risolvere questo problema consiste nel sostituire la query XPath con un valore letterale come descritto nella sezione precedente. Questo problema può verificarsi se si usa il generatore delle query XPath nel **Aggiungi inizializzatori di correlazione** oppure **definizione di CorrelatesOn** finestre di dialogo e il servizio del flusso di lavoro utilizza contratti di messaggio. Nell'esempio seguente viene definita una classe dei contratto di messaggio.

```csharp
[MessageContract]
public class AddItemMessage
{
    [MessageHeader]
    public string CartId;

    [MessageBodyMember]
    public string Item;
}
```

 Questo contratto di messaggio viene utilizzato da un'attività <xref:System.ServiceModel.Activities.Receive> in un flusso di lavoro. Il `CartId` nell'intestazione del messaggio viene utilizzato per correlare il messaggio all'istanza corretta. Se la query XPath che recupera il `CartId` viene creata utilizzando le finestre di dialogo di correlazione nella finestra di progettazione del flusso di lavoro, viene generata la seguente query XPath errata.

```
sm:body()/xg0:AddItemMessage/xg0:CartId
```

 Questa query XPath sarebbe corretta se l'attività <xref:System.ServiceModel.Activities.Receive> utilizzasse i parametri relativi ai dati, ma dal momento che utilizza un contratto di messaggio, non è corretta. La seguenti query XPath è la query XPath corretta per recuperare il `CartId` dall'intestazione.

```
sm:header()/tempuri:CartId
```

Questo può essere confermato esaminando il corpo del messaggio.

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Header>
    <Action s:mustUnderstand="1" xmlns="http://schemas.microsoft.com/ws/2005/05/addressing/none">http://tempuri.org/IService/AddItem</Action>
    <h:CartId xmlns:h="http://tempuri.org/">80c95b41-c98d-4660-a6c1-99412206e54c</h:CartId>
  </s:Header>
  <s:Body>
    <AddItemMessage xmlns="http://tempuri.org/">
      <Item>Books</Item>
    </AddItemMessage>
  </s:Body>
</s:Envelope>
```

Nell'esempio riportato di seguito viene illustrata un'attività <xref:System.ServiceModel.Activities.Receive> configurata per un'operazione `AddItem` che utilizza il contratto di messaggio precedente per ricevere i dati. La query XPath è configurata correttamente.

```xaml
<Receive CorrelatesWith="[CCHandle] OperationName="AddItem" ServiceContractName="p:IService">
  <Receive.CorrelatesOn>
    <XPathMessageQuery x:Key="key1">
      <XPathMessageQuery.Namespaces>
        <ssx:XPathMessageContextMarkup>
          <x:String x:Key="xg0">http://schemas.datacontract.org/2004/07/MessageContractWFService</x:String>
        </ssx:XPathMessageContextMarkup>
      </XPathMessageQuery.Namespaces>sm:header()/tempuri:CartId</XPathMessageQuery>
  </Receive.CorrelatesOn>
  <ReceiveMessageContent DeclaredMessageType="m:AddItemMessage">
    <p1:OutArgument x:TypeArguments="m:AddItemMessage">[AddItemMessage]</p1:OutArgument>
  </ReceiveMessageContent>
</Receive>
```
