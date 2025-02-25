---
title: Creazione di associazioni definite dall'utente
ms.date: 03/30/2017
helpviewer_keywords:
- user-defined bindings [WCF]
ms.assetid: c4960675-d701-4bc9-b400-36a752fdd08b
ms.openlocfilehash: e1e776a42ca63e4b862e307cbcae1bab2847d0ca
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64587302"
---
# <a name="creating-user-defined-bindings"></a>Creazione di associazioni definite dall'utente
Esistono diversi modi per creare associazioni non fornite dal sistema:  
  
- Creare un'associazione personalizzata, basata sulla classe <xref:System.ServiceModel.Channels.CustomBinding>, che è un contenitore riempito con elementi di associazione. L'associazione personalizzata viene quindi aggiunta a un endpoint del servizio. È possibile creare l'associazione personalizzata a livello di programmazione o in un file di configurazione dell'applicazione. Per usare un elemento di associazione da un file di configurazione dell'applicazione, è necessario che l'elemento di associazione estenda <xref:System.ServiceModel.Configuration.BindingElementExtensionElement>. Per altre informazioni sulle associazioni personalizzate, vedere [associazioni personalizzate](../../../../docs/framework/wcf/extending/custom-bindings.md) e <xref:System.ServiceModel.Channels.CustomBinding>.  
  
- È possibile creare una classe derivata da un'associazione standard. È, ad esempio, possibile derivare una classe da <xref:System.ServiceModel.WSHttpBinding> ed eseguire l'override del metodo <xref:System.ServiceModel.Channels.CustomBinding.CreateBindingElements%2A> per ottenere gli elementi di associazione e inserire un elemento di associazione personalizzato o stabilire un particolare valore per la protezione.  
  
- È possibile creare un nuovo tipo <xref:System.ServiceModel.Channels.Binding> per controllare completamente l'intera implementazione dell'associazione.  
  
## <a name="the-order-of-binding-elements"></a>Ordine degli elementi di associazione  
 Ogni elemento di associazione rappresenta una fase di elaborazione durante l'invio o la ricezione di messaggi. In fase di esecuzione, gli elementi di associazione creano i canali e i listener necessari per generare stack di canali in uscita e in ingresso.  
  
 Esistono tre tipi principali di elementi di associazione: Associazione di elementi, gli elementi di associazione di codifica e gli elementi di associazione di trasporto di protocollo.  
  
 Elementi di associazione di protocollo: rappresentano passaggi di elaborazione di livello superiore che agiscono sui messaggi. I canali e i listener creati da questi elementi di associazione possono aggiungere, rimuovere o modificare il contenuto del messaggio. Una determinata associazione può avere un numero arbitrario di elementi di associazione di protocollo, ognuno dei quali eredita da <xref:System.ServiceModel.Channels.BindingElement>. Windows Communication Foundation (WCF) include diversi elementi di associazione di protocollo, tra cui il <xref:System.ServiceModel.Channels.ReliableSessionBindingElement> e il <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement>.  
  
 Elementi di associazione di codifica: rappresentano trasformazioni tra un messaggio e una codifica pronti per la trasmissione in rete. Le associazioni WCF tipiche includono esattamente un elemento di associazione di codifica. Tra gli esempi di elementi di associazione di codifica sono inclusi <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement>, <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement> e <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>. Se non viene specificato alcun elemento di associazione di codifica per un'associazione, viene usata la codifica predefinita. L'impostazione predefinita corrisponde a "testo", se il trasporto è HTTP, altrimenti a "binaria".  
  
 Elementi di associazione del trasporto: rappresentano la trasmissione di un messaggio di codifica su un protocollo di trasporto. Le associazioni WCF tipiche includono esattamente un elemento dell'associazione di trasporto, che eredita da <xref:System.ServiceModel.Channels.TransportBindingElement>. Tra gli esempi di elementi di associazione del trasporto sono inclusi <xref:System.ServiceModel.Channels.TcpTransportBindingElement>, <xref:System.ServiceModel.Channels.HttpTransportBindingElement> e <xref:System.ServiceModel.Channels.NamedPipeTransportBindingElement>.  
  
 Quando si creano nuove associazioni, l'ordine degli elementi di associazione aggiunti è importante. Aggiungere sempre gli elementi di associazione nell'ordine seguente:  
  
|Livello|Opzioni|Obbligatorio|  
|-----------|-------------|--------------|  
|Flusso transazioni|<xref:System.ServiceModel.Channels.TransactionFlowBindingElement?displayProperty=nameWithType>|No|  
|Affidabilità|<xref:System.ServiceModel.Channels.ReliableSessionBindingElement?displayProperty=nameWithType>|No|  
|Sicurezza|<xref:System.ServiceModel.Channels.SecurityBindingElement?displayProperty=nameWithType>|No|  
|Duplex composito|<xref:System.ServiceModel.Channels.CompositeDuplexBindingElement?displayProperty=nameWithType>|No|  
|Codifica|Testo, binario, MTOM, personalizzata|Sì*|  
|Trasporto|TCP, named pipe, HTTP, HTTPS, MSMQ, personalizzato|Yes|  
  
 * Poiché è necessaria per ogni associazione, se non è specificata alcuna codifica, WCF aggiunge una codifica predefinita per l'utente. L'impostazione predefinita è Text/XML per i trasporti HTTP e HTTPS e Binary per gli altri trasporti.  
  
## <a name="creating-a-new-binding-element"></a>Creazione di un nuovo elemento di associazione  
 Oltre ai tipi derivati da <xref:System.ServiceModel.Channels.BindingElement> che vengono messe a disposizione da WCF, è possibile creare elementi dell'associazione. Questo consente di personalizzare la modalità di creazione dello stack di associazioni e di specificare i componenti in esso inclusi creando un proprio <xref:System.ServiceModel.Channels.BindingElement> che può essere composto con gli altri tipi forniti dal sistema nello stack.  
  
 Ad esempio, se si implementa un `LoggingBindingElement` che offre la possibilità di registrare il messaggio in un database, è necessario posizionare tale elemento sopra un canale di trasporto nello stack di canali. In questo caso, l'applicazione crea un'associazione personalizzata che compone `LoggingBindingElement` con `TcpTransportBindingElement`, come nell'esempio seguente.  
  
```csharp  
Binding customBinding = new CustomBinding(  
  new LoggingBindingElement(),   
  new TcpTransportBindingElement()  
);  
```  
  
 La modalità di scrittura del nuovo elemento di associazione dipende dalla funzionalità esatta. Uno degli esempi, [trasporto: UDP](../../../../docs/framework/wcf/samples/transport-udp.md), fornisce una descrizione dettagliata di come implementare un tipo di elemento di associazione.  
  
## <a name="creating-a-new-binding"></a>Creazione di una nuova associazione  
 Un elemento di associazione creato dall'utente può essere usato in due modi. Nella sezione precedente è stato illustrato il primo modo, ovvero tramite un'associazione personalizzata. Un'associazione personalizzata consente all'utente di creare una propria associazione basata su un set arbitrario di elementi di associazione, inclusi quelli creati dall'utente.  
  
 Se si usa l'associazione in più di un'applicazione, creare la propria associazione ed estendere <xref:System.ServiceModel.Channels.Binding>. Questo evita di creare manualmente un'associazione personalizzata ogni volta che si desidera usarla. Un'associazione definita dall'utente consente di definire il comportamento dell'associazione e includere elementi di associazione definiti dall'utente. E risulta *preinserito*: non è necessario ricreare l'associazione ogni volta che si usi.  
  
 Un'associazione definita dall'utente deve implementare almeno il metodo <xref:System.ServiceModel.Channels.Binding.CreateBindingElements%2A> e la proprietà <xref:System.ServiceModel.Channels.Binding.Scheme%2A>.  
  
 Il metodo <xref:System.ServiceModel.Channels.Binding.CreateBindingElements%2A> restituisce una nuova classe <xref:System.ServiceModel.Channels.BindingElementCollection> contenente gli elementi di associazione per l'associazione. La raccolta è ordinata e deve contenere prima gli elementi di associazione di protocollo, seguiti dall'elemento di associazione di codifica, seguito dall'elemento di associazione del trasporto. Quando si usano gli elementi di associazione fornita dal sistema WCF, è necessario seguire l'elemento di associazione specificate nella regole di ordinamento [associazioni personalizzate](../../../../docs/framework/wcf/extending/custom-bindings.md). Questa raccolta non deve mai fare riferimento a oggetti a cui si fa riferimento nella classe di associazioni definite dall'utente; gli autori delle associazioni devono pertanto restituire un `Clone()` di <xref:System.ServiceModel.Channels.BindingElementCollection> in ogni chiamata a <xref:System.ServiceModel.Channels.Binding.CreateBindingElements%2A>.  
  
 La proprietà <xref:System.ServiceModel.Channels.Binding.Scheme%2A> rappresenta lo schema URI del protocollo di trasporto in uso nell'associazione. Ad esempio, il *WSHttpBinding* e il *NetTcpBinding* restituiscono "http" e "NET. TCP" dalle rispettive <xref:System.ServiceModel.Channels.Binding.Scheme%2A> proprietà.  
  
 Per un elenco completo delle proprietà e dei metodi facoltativi per le associazioni definite dall'utente, vedere <xref:System.ServiceModel.Channels.Binding>.  
  
### <a name="example"></a>Esempio  
 In questo esempio viene implementata un'associazione di profilo in `SampleProfileUdpBinding`, che deriva da <xref:System.ServiceModel.Channels.Binding>. Il `SampleProfileUdpBinding` contiene fino a quattro elementi di associazione: uno creato dall'utente `UdpTransportBindingElement`; e tre forniti dal sistema: `TextMessageEncodingBindingElement`, `CompositeDuplexBindingElement`, e `ReliableSessionBindingElement`.  
  
```csharp
public override BindingElementCollection CreateBindingElements()  
{     
    BindingElementCollection bindingElements = new BindingElementCollection();  
    if (ReliableSessionEnabled)  
    {  
        bindingElements.Add(session);  
        bindingElements.Add(compositeDuplex);  
    }  
    bindingElements.Add(encoding);  
    bindingElements.Add(transport);  
    return bindingElements.Clone();  
}  
```  
  
## <a name="security-restrictions-with-duplex-contracts"></a>Restrizioni di sicurezza con i contratti duplex  
 Non tutti gli elementi di associazione sono reciprocamente compatibili. In particolare, esistono alcune restrizioni sugli elementi di associazione di sicurezza, se usati con contratti duplex.  
  
### <a name="one-shot-security"></a>Protezione monofase  
 È possibile implementare la sicurezza "monofase", in cui tutte le credenziali di sicurezza necessarie vengono inviate in un singolo messaggio, impostando il `negotiateServiceCredential` attributo del \<messaggio > elemento di configurazione da `false`.  
  
 L'autenticazione monofase non funziona con i contratti duplex.  
  
 Per i contratti request/reply, l'autenticazione monofase funziona solo se lo stack di associazioni sotto l'elemento di associazione di sicurezza supporta la creazione di istanze di <xref:System.ServiceModel.Channels.IRequestChannel> o <xref:System.ServiceModel.Channels.IRequestSessionChannel>.  
  
 Per i contratti unidirezionali, l'autenticazione monofase funziona se lo stack di associazioni sotto l'elemento di associazione di sicurezza supporta la creazione di istanze di <xref:System.ServiceModel.Channels.IRequestChannel>, <xref:System.ServiceModel.Channels.IRequestSessionChannel>, <xref:System.ServiceModel.Channels.IOutputChannel> o <xref:System.ServiceModel.Channels.IOutputSessionChannel>.  
  
### <a name="cookie-mode-security-context-tokens"></a>Token del contesto di sicurezza in modalità cookie  
 I token del contesto di sicurezza in modalità cookie non possono essere usati con contratti duplex.  
  
 Per i contratti request/reply, i token del contesto di sicurezza in modalità cookie funzionano solo se lo stack di associazioni sotto l'elemento di associazione di sicurezza supporta la creazione di istanze di <xref:System.ServiceModel.Channels.IRequestChannel> o <xref:System.ServiceModel.Channels.IRequestSessionChannel>.  
  
 Per i contratti unidirezionali, i token del contesto di sicurezza in modalità cookie funzionano se lo stack di associazioni sotto l'elemento di associazione di sicurezza supporta la creazione di istanze di <xref:System.ServiceModel.Channels.IRequestChannel> o <xref:System.ServiceModel.Channels.IRequestSessionChannel>.  
  
### <a name="session-mode-security-context-tokens"></a>Token del contesto di sicurezza in modalità sessione  
 I token del contesto di sicurezza (SCT) in modalità sessione funzionano per i contratti duplex se lo stack di associazioni sotto l'elemento di associazione di sicurezza supporta la creazione di istanze di <xref:System.ServiceModel.Channels.IDuplexChannel> o <xref:System.ServiceModel.Channels.IDuplexSessionChannel>.  
  
 I token del contesto di sicurezza (SCT) in modalità sessione funzionano per i contratti request/reply se lo stack di associazioni sotto l'elemento di associazione di sicurezza supporta la creazione di istanze di <xref:System.ServiceModel.Channels.IDuplexChannel>, <xref:System.ServiceModel.Channels.IDuplexSessionChannel>, <xref:System.ServiceModel.Channels.IRequestChannel> o <xref:System.ServiceModel.Channels.IRequestSessionChannel>.  
  
 I token del contesto di sicurezza (SCT) in modalità sessione funzionano per i contratti unidirezionali se lo stack di associazioni sotto l'elemento di associazione di sicurezza supporta la creazione di istanze di <xref:System.ServiceModel.Channels.IDuplexChannel>, <xref:System.ServiceModel.Channels.IDuplexSessionChannel>, <xref:System.ServiceModel.Channels.IRequestChannel> o <xref:System.ServiceModel.Channels.IRequestSessionChannel>.  
  
## <a name="deriving-from-a-standard-binding"></a>Derivazione da un'associazione standard  
 Invece di creare una classe di associazioni completamente nuova, è possibile estendere una delle associazioni fornite dal sistema esistenti. Analogamente al caso precedente, è necessario eseguire l'override del metodo <xref:System.ServiceModel.Channels.Binding.CreateBindingElements%2A> e della proprietà <xref:System.ServiceModel.Channels.Binding.Scheme%2A>.  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.ServiceModel.Channels.Binding>
- [Associazioni personalizzate](../../../../docs/framework/wcf/extending/custom-bindings.md)
