---
title: Codifica MTOM
ms.date: 03/30/2017
ms.assetid: 820e316f-4ee1-4eb5-ae38-b6a536e8a14f
ms.openlocfilehash: fda14ec04515f36467bbaa5077d904866e3d8013
ms.sourcegitcommit: c4e9d05644c9cb89de5ce6002723de107ea2e2c4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/19/2019
ms.locfileid: "65876367"
---
# <a name="mtom-encoding"></a>Codifica MTOM
In questo esempio viene illustrato come utilizzare la codifica dei messaggi MTOM (Message Transmission Optimization Mechanism, meccanismo di ottimizzazione della trasmissione dei messaggi) con un WSHttpBinding. MTOM è un meccanismo per trasmettere grandi allegati binari con messaggi SOAP come byte non elaborati, lasciando spazio a messaggi più piccoli.  
  
> [!IMPORTANT]
>  È possibile che gli esempi siano già installati nel computer. Verificare la directory seguente (impostazione predefinita) prima di continuare.  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  Se questa directory non esiste, andare al [Windows Communication Foundation (WCF) e gli esempi di Windows Workflow Foundation (WF) per .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) per scaricare tutti i Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] esempi. Questo esempio si trova nella directory seguente.  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\WS\MTOM`  
  
 Per impostazione predefinita, WSHttpBinding invia e riceve messaggi come XML di testo normale. Per abilitare l'invio e la ricezione di messaggi MTOM, impostare l'attributo `messageEncoding` sulla configurazione dell'associazione (come nel codice di esempio seguente) o direttamente sull'associazione utilizzando la proprietà `MessageEncoding`. Il servizio o client ora possono inviare e ricevere messaggi MTOM.  
  
```xml  
<wsHttpBinding>  
  <binding name="WSHttpBinding_IUpload" messageEncoding="Mtom" />  
</wsHttpBinding>  
```  
  
 Il codificatore MTOM può ottimizzare matrici di byte e flussi. In questo esempio, l'operazione utilizza un parametro `Stream` e può pertanto essere ottimizzata.  

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
  public interface IUpload  
  {  
      [OperationContract]  
      int Upload(Stream data);  
  }  
```
  
 Il contratto scelto per questo esempio trasmette dati binari al servizio e riceve il numero di byte caricato come valore restituito. Quando il servizio viene installato e il client viene eseguito, esso stampa il numero 1000 che indica che tutti i 1000 byte sono stati ricevuti. Il resto dell'output elenca le dimensioni del messaggio ottimizzate e non ottimizzate per vari payload.  
  
```  
Output:  
1000  
  
Text encoding with a 100 byte payload: 433  
MTOM encoding with a 100 byte payload: 912  
  
Text encoding with a 1000 byte payload: 1633  
MTOM encoding with a 1000 byte payload: 2080  
  
Text encoding with a 10000 byte payload: 13633  
MTOM encoding with a 10000 byte payload: 11080  
  
Text encoding with a 100000 byte payload: 133633  
MTOM encoding with a 100000 byte payload: 101080  
  
Text encoding with a 1000000 byte payload: 1333633  
MTOM encoding with a 1000000 byte payload: 1001080  
  
Press <ENTER> to terminate client.  
```  
  
 Lo scopo di MTOM è ottimizzare la trasmissione di grandi payload binari. L'invio di un messaggio SOAP con MTOM crea un sovraccarico evidente per i piccoli payload binari, ma consente grandi risparmi quando essi sono di centinaia di byte. La ragione di ciò è che XML di testo normale codifica dati binari utilizzando Base 64 che richiede quattro caratteri per ogni tre byte aumentando la dimensione dei dati di un terzo. MTOM è in grado di trasmettere dati binari come byte non elaborati, risparmiando i tempi di codifica/decodifica e quindi consentendo di trasmettere messaggi più piccoli. La soglia di alcune migliaia di byte è piccola se confrontata alle dimensioni dei documenti commerciali di oggi e a quelle delle fotografie digitali.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Per impostare, compilare ed eseguire l'esempio  
  
1. Installare ASP.NET 4.0 usando il comando seguente.  
  
    ```  
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
    ```  
  
2. Assicurarsi di avere eseguito il [monouso procedura di installazione per gli esempi di Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
3. Per compilare l'edizione in C# o Visual Basic .NET della soluzione, seguire le istruzioni in [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
4. Per eseguire l'esempio in una configurazione singola o tra computer, seguire le istruzioni in [esegue gli esempi di Windows Communication Foundation](../../../../docs/framework/wcf/samples/running-the-samples.md).  
