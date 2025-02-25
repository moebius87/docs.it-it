---
title: Modello a oggetti per la programmazione HTTP Web di WCF
ms.date: 03/30/2017
ms.assetid: ed96b5fc-ca2c-4b0d-bdba-d06b77c3cb2a
ms.openlocfilehash: f8bda6292506b64057dee006fa59b7723fa406b2
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64648401"
---
# <a name="wcf-web-http-programming-object-model"></a>Modello a oggetti per la programmazione HTTP Web di WCF
Il modello di programmazione HTTP di WCF WEB consente agli sviluppatori di esporre servizi Web Windows Communication Foundation (WCF) tramite richieste HTTP di base senza SOAP. Il modello di programmazione WCF WEB HTTP si basa il modello di estendibilità WCF esistente. Definisce le classi seguenti:  
  
 **Modello di programmazione:**  
  
- <xref:System.ServiceModel.Web.AspNetCacheProfileAttribute>  
  
- <xref:System.ServiceModel.Web.WebGetAttribute>  
  
- <xref:System.ServiceModel.Web.WebInvokeAttribute>  
  
- <xref:System.ServiceModel.Web.WebServiceHost>  
  
 **Infrastruttura del Dispatcher di canali e:**  
  
- <xref:System.ServiceModel.WebHttpBinding>  
  
- <xref:System.ServiceModel.Description.WebHttpBehavior>  
  
 **Classi di utilità e punti di estendibilità:**  
  
- <xref:System.UriTemplate>  
  
- <xref:System.UriTemplateTable>  
  
- <xref:System.ServiceModel.Dispatcher.QueryStringConverter>  
  
- <xref:System.ServiceModel.Dispatcher.WebHttpDispatchOperationSelector>  
  
## <a name="aspnetcacheprofileattribute"></a>AspNetCacheProfileAttribute  
 L'attributo <xref:System.ServiceModel.Web.AspNetCacheProfileAttribute>, quando applicato a un'operazione di servizio, indica il profilo della cache di output ASP.NET nel file di configurazione che deve essere usato per memorizzare nella cache le risposte dall'operazione nella cache di output ASP .NET. Questa proprietà accetta solo un parametro, il nome di profilo cache che specifica le impostazioni della cache nel file di configurazione.  
  
## <a name="webgetattribute"></a>WebGetAttribute  
 L'attributo <xref:System.ServiceModel.Web.WebGetAttribute> viene usato per contrassegnare un'operazione del servizio come operazione che risponde alle richieste HTTP GET. Si tratta di un comportamento di operazione passivo (i metodi <xref:System.ServiceModel.Description.IOperationBehavior> non eseguono alcuna operazione) che aggiunge metadati alla descrizione dell'operazione. L'applicazione di <xref:System.ServiceModel.Web.WebGetAttribute> non alcun effetto, a meno che alla raccolta dei comportamenti del servizio non venga aggiunto un comportamento che cerca questi metadati nella descrizione dell'operazione (in particolare, <xref:System.ServiceModel.Description.WebHttpBehavior>). L'attributo <xref:System.ServiceModel.Web.WebGetAttribute> accetta i parametri facoltativi indicati nella tabella seguente.  
  
|Parametro|Descrizione|  
|---------------|-----------------|  
|`BodyStyle`|Controlla se eseguire il wrapping delle richieste e delle risposte inviate e ricevute dall'operazione del servizio a cui è applicato l'attributo.|  
|`RequestFormat`|Controlla la modalità di formattazione dei messaggi di richiesta.|  
|`ResponseFormat`|Controlla la modalità di formattazione dei messaggi di risposta.|  
|`UriTemplate`|Specifica il modello di URI che controlla le richieste HTTP mappate all'operazione del servizio a cui è applicato l'attributo.|  
  
## <a name="webhttpbinding"></a>WebHttpBinding  
 La classe <xref:System.ServiceModel.WebHttpBinding> incorpora il supporto per XML, JSON e i dati binari non elaborati usando l'elemento <xref:System.ServiceModel.Channels.WebMessageEncodingBindingElement>. La classe è costituita da un oggetto <xref:System.ServiceModel.Channels.HttpsTransportBindingElement>, <xref:System.ServiceModel.Channels.HttpTransportBindingElement> e da un oggetto <xref:System.ServiceModel.WebHttpSecurity>. La classe <xref:System.ServiceModel.WebHttpBinding> è progettata per essere usata insieme a <xref:System.ServiceModel.Description.WebHttpBehavior>.  
  
## <a name="webinvokeattribute"></a>WebInvokeAttribute  
 L'attributo <xref:System.ServiceModel.Web.WebInvokeAttribute> è simile a <xref:System.ServiceModel.Web.WebGetAttribute>, ma viene usato per contrassegnare un'operazione del servizio come operazione che risponde alle richieste HTTP diverse da GET. Si tratta di un comportamento di operazione passivo (i metodi <xref:System.ServiceModel.Description.IOperationBehavior> non eseguono alcuna operazione) che aggiunge metadati alla descrizione dell'operazione. L'applicazione di <xref:System.ServiceModel.Web.WebInvokeAttribute> non alcun effetto, a meno che alla raccolta dei comportamenti del servizio non venga aggiunto un comportamento che cerca questi metadati nella descrizione dell'operazione (in particolare, <xref:System.ServiceModel.Description.WebHttpBehavior>).  
  
 L'attributo <xref:System.ServiceModel.Web.WebInvokeAttribute> accetta i parametri facoltativi indicati nella tabella seguente.  
  
|Parametro|Descrizione|  
|---------------|-----------------|  
|`BodyStyle`|Controlla se eseguire il wrapping delle richieste e delle risposte inviate e ricevute dall'operazione del servizio a cui è applicato l'attributo.|  
|`Method`|Specifica il metodo HTTP a cui è mappata l'operazione del servizio.|  
|`RequestFormat`|Controlla la modalità di formattazione dei messaggi di richiesta.|  
|`ResponseFormat`|Controlla la modalità di formattazione dei messaggi di risposta.|  
|`UriTemplate`|Specifica il modello di URI che controlla quali richieste GET vengono mappate all'operazione del servizio a cui è applicato l'attributo.|  
  
## <a name="uritemplate"></a>UriTemplate  
 La classe <xref:System.UriTemplate> consente di definire un set di URI strutturalmente simili. I modelli sono composti di due parti, un percorso e una query. Un percorso è costituito da una serie di segmenti delimitati da una barra (/). Ogni segmento può avere un valore letterale, un valore della variabile (scritto tra parentesi graffe [{], vincolato in base al contenuto di un solo segmento) o un carattere jolly (scritto come asterisco [\*], che corrisponde a "il resto del percorso"), che devono essere usati in fine del percorso. L'espressione di query può essere interamente omessa. Se presente, specifica una serie non ordinata di coppie nome/valore. Gli elementi dell'espressione di query possono essere coppie letterali (? x = 2) o coppie variabili (? x = {*valore*}). Non è consentito usare valori non abbinati. <xref:System.UriTemplate> viene utilizzata internamente dal modello di programmazione di WCF WEB HTTP per eseguire il mapping di URI o gruppi di URI specifici a operazioni del servizio.  
  
## <a name="uritemplatetable"></a>UriTemplateTable  
 La classe <xref:System.UriTemplateTable> rappresenta un set associativo di oggetti <xref:System.UriTemplate> associati a un oggetto scelto dallo sviluppatore. Consente di confrontare gli URI (Uniform Resource Identifier) candidati con i modelli del set e recuperare i dati associati ai modelli corrispondenti. <xref:System.UriTemplateTable> viene utilizzata internamente dal modello di programmazione di WCF WEB HTTP per eseguire il mapping di URI o gruppi di URI specifici a operazioni del servizio.  
  
## <a name="webservicehost"></a>WebServiceHost  
 <xref:System.ServiceModel.Web.WebServiceHost> estende <xref:System.ServiceModel.ServiceHost> per semplificare l'host di un servizio Web non SOAP. Se <xref:System.ServiceModel.Web.WebServiceHost> non trova endpoint nella descrizione del servizio, crea automaticamente un endpoint predefinito all'indirizzo di base del servizio. Quando viene creato un endpoint HTTP predefinito, <xref:System.ServiceModel.Web.WebServiceHost> disabilita anche la pagina della Guida HTTP e la funzionalità WSDL (Web Services Description Language) GET, in modo che l'endpoint dei metadati non interferisce con l'endpoint HTTP predefinito.  L'oggetto <xref:System.ServiceModel.Web.WebServiceHost> assicura inoltre che tutti gli endpoint che usano <xref:System.ServiceModel.WebHttpBinding> siano collegati all'oggetto <xref:System.ServiceModel.Description.WebHttpBehavior> necessario. Infine, <xref:System.ServiceModel.Web.WebServiceHost> configura automaticamente l'associazione dell'endpoint per usare le impostazioni di sicurezza IIS (Internet Information Services) associate in caso di uso in una directory virtuale protetta.  
  
## <a name="webservicehostfactory"></a>WebServiceHostFactory  
 La classe <xref:System.ServiceModel.Activation.WebServiceHostFactory> viene usata per creare un <xref:System.ServiceModel.Web.WebServiceHost> in modo dinamico, quando un servizio viene ospitato in IIS (Internet Information Services) o WAS (Windows Process Activation Service). A differenza dei servizi indipendenti, in cui l'applicazione host crea un'istanza di <xref:System.ServiceModel.Web.WebServiceHost>, i servizi ospitati in IIS o WAS usano questa classe per creare l'oggetto <xref:System.ServiceModel.Web.WebServiceHost> per il servizio. Il metodo <xref:System.ServiceModel.Activation.WebServiceHostFactory.CreateServiceHost%28System.Type%2CSystem.Uri%5B%5D%29> viene chiamato quando viene ricevuta una richiesta in ingresso per il servizio.  
  
## <a name="webhttpbehavior"></a>WebHttpBehavior  
 La classe <xref:System.ServiceModel.Description.WebHttpBehavior> fornisce, tra gli altri, i formattatori e i selettori di operazioni necessari per il supporto del servizio web a livello di modello di servizio. La classe viene implementata come un comportamento dell'endpoint (usato insieme con <xref:System.ServiceModel.WebHttpBinding>) e consente di specificare formattatori e selettori di operazioni per ogni endpoint, consentendo alla stessa implementazione del servizio di esporre endpoint SOAP e POX.  
  
### <a name="extending-webhttpbehavior"></a>Estensione di WebHttpBehavior  
 <xref:System.ServiceModel.Description.WebHttpBehavior> è estendibile usando diversi metodi virtuali: <xref:System.ServiceModel.Description.WebHttpBehavior.GetOperationSelector%28System.ServiceModel.Description.ServiceEndpoint%29>, <xref:System.ServiceModel.Description.WebHttpBehavior.GetReplyClientFormatter%28System.ServiceModel.Description.OperationDescription%2CSystem.ServiceModel.Description.ServiceEndpoint%29>, <xref:System.ServiceModel.Description.WebHttpBehavior.GetRequestClientFormatter%28System.ServiceModel.Description.OperationDescription%2CSystem.ServiceModel.Description.ServiceEndpoint%29>, <xref:System.ServiceModel.Description.WebHttpBehavior.GetReplyDispatchFormatter%28System.ServiceModel.Description.OperationDescription%2CSystem.ServiceModel.Description.ServiceEndpoint%29> e <xref:System.ServiceModel.Description.WebHttpBehavior.GetRequestDispatchFormatter%28System.ServiceModel.Description.OperationDescription%2CSystem.ServiceModel.Description.ServiceEndpoint%29>. Gli sviluppatori possono derivare una classe da <xref:System.ServiceModel.Description.WebHttpBehavior> ed eseguire l'override di questi metodi per personalizzare il comportamento predefinito.  
  
 L'oggetto <xref:System.ServiceModel.Description.WebScriptEnablingBehavior> è un esempio di estensione di <xref:System.ServiceModel.Description.WebHttpBehavior>. <xref:System.ServiceModel.Description.WebScriptEnablingBehavior> Abilita endpoint Windows Communication Foundation (WCF) ricevere richieste HTTP da un client AJAX ASP.NET basati su browser. Il [servizio AJAX con HTTP POST](../../../../docs/framework/wcf/samples/ajax-service-using-http-post.md) è riportato un esempio dell'uso di questo punto di estendibilità.  
  
> [!WARNING]
>  Quando si usa <xref:System.ServiceModel.Description.WebScriptEnablingBehavior>, non è supportato l'uso di <xref:System.UriTemplate> all'interno di attributi <xref:System.ServiceModel.Web.WebGetAttribute> o <xref:System.ServiceModel.Web.WebInvokeAttribute>.  
  
## <a name="webhttpdispatchoperationselector"></a>WebHttpDispatchOperationSelector  
 La classe <xref:System.ServiceModel.Dispatcher.WebHttpDispatchOperationSelector> usa le classi <xref:System.UriTemplate> e <xref:System.UriTemplateTable> per inviare chiamate alle operazioni del servizio.  
  
## <a name="compatibility"></a>Compatibilità  
 Il modello di programmazione HTTP di WCF WEB non utilizza messaggi basati su SOAP e pertanto non supporta WS-* protocolli. È tuttavia possibile esporre lo stesso contratto tramite due endpoint diversi, di cui uno usa SOAP e l'altro no. Vedere [Procedura: Esporre un contratto a client SOAP e Web](../../../../docs/framework/wcf/feature-details/how-to-expose-a-contract-to-soap-and-web-clients.md) per un esempio.  
  
## <a name="security"></a>Sicurezza  
 Poiché il modello di programmazione HTTP di WCF WEB non supporta WS-* protocolli l'unico modo per proteggere un servizio Web basato sul modello di programmazione HTTP di WCF WEB consiste nell'esporre il servizio tramite SSL. Per altre informazioni sull'impostazione di configurazione di SSL con [!INCLUDE[iisver](../../../../includes/iisver-md.md)] vedere [come implementare SSL in IIS](https://go.microsoft.com/fwlink/?LinkId=131613)  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.ServiceModel.WebHttpBinding>
- <xref:System.ServiceModel.Web.WebGetAttribute>
- <xref:System.ServiceModel.Web.WebInvokeAttribute>
- <xref:System.ServiceModel.Description.WebHttpBehavior>
- <xref:System.ServiceModel.Dispatcher.WebHttpDispatchOperationSelector>
- [Panoramica del modello di programmazione HTTP Web di WCF](../../../../docs/framework/wcf/feature-details/wcf-web-http-programming-model-overview.md)
