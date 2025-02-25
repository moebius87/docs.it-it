---
title: Traccia ETW
ms.date: 03/30/2017
ms.assetid: ac99a063-e2d2-40cc-b659-d23c2f783f92
ms.openlocfilehash: a5c2f173978f514aa4627caa476a595d8d45d4f9
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "62051923"
---
# <a name="etw-tracing"></a>Traccia ETW
In questo esempio viene illustrato come implementare la traccia End-to-End (E2E) utilizzando il sistema Event Tracing for Windows (ETW) e il `ETWTraceListener` fornito in questo esempio. L'esempio è basato sul [introduttiva](../../../../docs/framework/wcf/samples/getting-started-sample.md) e comprende la traccia ETW.  
  
> [!NOTE]
>  La procedura di installazione e le istruzioni di compilazione per questo esempio si trovano alla fine di questo argomento.  
  
 In questo esempio si presuppone che abbia familiarità con [traccia e registrazione dei messaggi](../../../../docs/framework/wcf/samples/tracing-and-message-logging.md).  
  
 Ogni origine di traccia nel modello di traccia <xref:System.Diagnostics> può essere dotata di più listener di traccia che determinano dove e come tracciare i dati. Il tipo di listener definisce il formato della registrazione dei dati di traccia. Nell'esempio di codice seguente viene illustrato come aggiungere il listener alla configurazione.  
  
```xml  
<system.diagnostics>  
    <sources>  
        <source name="System.ServiceModel"   
             switchValue="Verbose,ActivityTracing"  
             propagateActivity="true">  
            <listeners>  
                <add type=  
                   "System.Diagnostics.DefaultTraceListener"  
                   name="Default">  
                   <filter type="" />  
                </add>  
                <add name="ETW">  
                    <filter type="" />  
                </add>  
            </listeners>  
        </source>  
    </sources>  
    <sharedListeners>  
        <add type=  
            "Microsoft.ServiceModel.Samples.EtwTraceListener, ETWTraceListener"  
            name="ETW" traceOutputOptions="Timestamp">  
            <filter type="" />  
       </add>  
    </sharedListeners>  
</system.diagnostics>  
```  
  
 Prima di utilizzare questo listener, è necessario avviare una sessione di traccia ETW. Questa sessione può essere avviata utilizzando Logman.exe o Tracelog.exe. Questo esempio comprende un file SetupETW.bat che può essere utilizzato per configurare la sessione di traccia ETW e un file CleanupETW.bat che può essere utilizzato per chiudere la sessione e completare il file di log.  
  
> [!NOTE]
>  La procedura di installazione e le istruzioni di compilazione per questo esempio si trovano alla fine di questo argomento. Per altre informazioni su questi strumenti, vedere <https://go.microsoft.com/fwlink/?LinkId=56580>  
  
 Quando si utilizza ETWTraceListener, le tracce vengono registrate in file .etl binari. Se la traccia ServiceModel è attivata, tutte le tracce generate vengono visualizzate nello stesso file. Uso [strumento Service Trace Viewer (SvcTraceViewer.exe)](../../../../docs/framework/wcf/service-trace-viewer-tool-svctraceviewer-exe.md) per la visualizzazione dei file di log con estensione etl e. svclog. Il visualizzatore crea una visualizzazione end-to-end del sistema che rende possibile tracciare un messaggio dall'origine alla destinazione e al punto di utilizzo.  
  
 Il listener di traccia ETW supporta i log circolari. Per abilitare questa funzionalità, passare a **avviare**, **eseguito** e il tipo `cmd` per avviare una console dei comandi. Nel comando seguente, sostituire il parametro `<logfilename>` con il nome del file di log.  
  
```  
logman create trace Wcf -o <logfilename> -p "{411a0819-c24b-428c-83e2-26b41091702e}" -f bincirc -max 1000  
```  
  
 Le opzioni `-f` e `-max` sono facoltative. Specificano rispettivamente il formato circolare binario e la dimensione massima del log di 1000 MB. L'opzione `-p` viene utilizzata per specificare il provider di traccia. Nell'esempio `"{411a0819-c24b-428c-83e2-26b41091702e}"` è il GUID per un l'esempio di provider ETW XML.  
  
 Per avviare la sessione, digitare il comando seguente:  
  
```  
Logman start Wcf  
```  
  
 Una volta completato il log, è possibile interrompere la sessione con il comando seguente.  
  
```  
Logman stop Wcf  
```  
  
 Questo processo genera log circolari binari da elaborare con uno strumento a scelta, inclusi [strumento Service Trace Viewer (SvcTraceViewer.exe)](../../../../docs/framework/wcf/service-trace-viewer-tool-svctraceviewer-exe.md) o Tracerpt.  
  
 È anche possibile esaminare i [traccia circolare](../../../../docs/framework/wcf/samples/circular-tracing.md) esempio per altre informazioni su un listener alternativo per eseguire la registrazione circolare.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Per impostare, compilare ed eseguire l'esempio  
  
1. Assicurarsi di avere eseguito il [monouso procedura di installazione per gli esempi di Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Per compilare la soluzione, seguire le istruzioni riportate in [Building Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
    > [!NOTE]
    >  Per utilizzare i comandi RegisterProvider.bat, SetupETW.bat e CleanupETW.bat comandi, è necessario aver eseguito l'accesso con un account Administrator locale. Se si utilizza [!INCLUDE[wv](../../../../includes/wv-md.md)] o versione successiva, sarà inoltre necessario eseguire il prompt dei comandi con privilegi elevati. A tale scopo, fare doppio clic sull'icona del prompt, quindi fare clic su **Esegui come amministratore**.  
  
3. Prima di eseguire l'esempio, eseguire RegisterProvider.bat nel client e nel server. In questo modo viene configurato il file ETWTracingSampleLog.etl risultante per generare tracce che possono essere lette da Service Trace Viewer. Questo file si trova nella cartella C:\log. Se questa cartella non esiste, deve essere creata o non verranno generate tracce. Eseguire quindi SetupETW.bat sui computer client e server per avviare la sessione di traccia ETW. Il file SetupETW.bat si trova nella cartella CS\Client.  
  
4. Per eseguire l'esempio in una configurazione singola o tra computer, seguire le istruzioni in [esegue gli esempi di Windows Communication Foundation](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
5. Una volta completato l'esempio, eseguire CleanupETW.bat per completare la creazione del file ETWTracingSampleLog.etl.  
  
6. Aprire il file ETWTracingSampleLog.etl da Service Trace Viewer. Verrà richiesto di salvare il file binario formattato come un file .svclog.  
  
7. In Service Trace Viewer aprire il file .svclog creato per visualizzare le tracce ETW e ServiceModel.  
  
> [!IMPORTANT]
>  È possibile che gli esempi siano già installati nel computer. Verificare la directory seguente (impostazione predefinita) prima di continuare.  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  Se questa directory non esiste, andare al [Windows Communication Foundation (WCF) e gli esempi di Windows Workflow Foundation (WF) per .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) per scaricare tutti i Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] esempi. Questo esempio si trova nella directory seguente.  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\AnalyticTrace`  
  
## <a name="see-also"></a>Vedere anche

- [Esempi di monitoraggio di AppFabric](https://go.microsoft.com/fwlink/?LinkId=193959)
