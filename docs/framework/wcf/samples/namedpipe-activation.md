---
title: Attivazione di NamedPipe
ms.date: 03/30/2017
ms.assetid: f3c0437d-006c-442e-bfb0-6b29216e4e29
ms.openlocfilehash: 3e6084e8334eddc16b115cc1199819c6ab637666
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "62051845"
---
# <a name="namedpipe-activation"></a>Attivazione di NamedPipe

In questo esempio viene dimostrato l'hosting di un servizio che usa WAS (Windows Process Activation Service) per attivare un servizio che comunica su named pipe. In questo esempio si basa sul [Guida introduttiva](../../../../docs/framework/wcf/samples/getting-started-sample.md) e richiede [!INCLUDE[wv](../../../../includes/wv-md.md)] per l'esecuzione.

> [!NOTE]
> La procedura di installazione e le istruzioni di compilazione per questo esempio si trovano alla fine di questo argomento.

> [!IMPORTANT]
> È possibile che gli esempi siano già installati nel computer. Verificare la directory seguente (impostazione predefinita) prima di continuare.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se questa directory non esiste, andare al [Windows Communication Foundation (WCF) e gli esempi di Windows Workflow Foundation (WF) per .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) per scaricare tutti i Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] esempi. Questo esempio si trova nella directory seguente.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Hosting\WASHost\NamedPipeActivation`

## <a name="sample-details"></a>Dettagli dell'esempio

L'esempio è costituito da un programma console client (.exe) e una libreria di servizi (.dll) ospitati in un processo di lavoro attivato dal servizio di attivazione dei processi di Windows (WAS). L'attività del client è visibile nella finestra della console.

Il servizio implementa un contratto che definisce un modello di comunicazione richiesta/risposta. Il contratto è definito dall'interfaccia `ICalculator`, che espone operazioni matematiche (somma, sottrazione, moltiplicazione e divisione), come illustrato nel codice di esempio seguente.

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface ICalculator
{
    [OperationContract]
    double Add(double n1, double n2);
    [OperationContract]
    double Subtract(double n1, double n2);
    [OperationContract]
    double Multiply(double n1, double n2);
    [OperationContract]
    double Divide(double n1, double n2);
}
```

Il client esegue richieste sincrone a una determinata operazione matematica e l'implementazione del servizio calcola e restituisce il risultato appropriato.

```csharp
// Service class that implements the service contract.
public class CalculatorService : ICalculator
{
    public double Add(double n1, double n2)
    {
        return n1 + n2;
    }
    public double Subtract(double n1, double n2)
    {
        return n1 - n2;
    }
    public double Multiply(double n1, double n2)
    {
        return n1 * n2;
    }
    public double Divide(double n1, double n2)
    {
        return n1 / n2;
    }
}
```

L'esempio usa un'associazione `netNamedPipeBinding` modificata senza sicurezza. L'associazione è specificata nei file di configurazione per il client e il servizio. Il tipo di associazione per il servizio è specificato nell'attributo `binding` dell'elemento endpoint come mostrato nella configurazione di esempio seguente.

Se si desidera usare un'associazione named pipe protetta, impostare la modalità di sicurezza del server sul tipo desiderato ed eseguire di nuovo svcutil.exe sul client per ottenere un file di configurazione client aggiornato.

```xml
<system.serviceModel>
        <services>
            <service name="Microsoft.ServiceModel.Samples.CalculatorService"
               behaviorConfiguration="CalculatorServiceBehavior">

        <!-- This endpoint is exposed at the base address provided by host: net.pipe://localhost/servicemodelsamples/service.svc  -->
        <endpoint address=""
                  binding="netNamedPipeBinding"
                  bindingConfiguration="Binding1"
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />
        <!-- the mex endpoint is exposed at net.pipe://localhost/servicemodelsamples/service.svc/mex -->
        <endpoint address="mex"
                  binding="mexNamedPipeBinding"
                  contract="IMetadataExchange" />
      </service>
        </services>
        <bindings>
            <netNamedPipeBinding>
                <binding name="Binding1" >
                    <security mode = "None">
                    </security>
                </binding >
            </netNamedPipeBinding>
        </bindings>

    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->
    <behaviors>
      <serviceBehaviors>
        <behavior name="CalculatorServiceBehavior">
          <serviceMetadata />
          <serviceDebug includeExceptionDetailInFaults="False" />
        </behavior>
      </serviceBehaviors>
    </behaviors>

  </system.serviceModel>
```

Le informazioni endpoint del client sono configurate come illustrato nel codice di esempio seguente.

```xml
<system.serviceModel>

    <client>
      <endpoint name=""
                          address="net.pipe://localhost/servicemodelsamples/service.svc"
                          binding="netNamedPipeBinding"
                          bindingConfiguration="Binding1"
                          contract="Microsoft.ServiceModel.Samples.ICalculator" />
    </client>

    <bindings>

      <!--  Following is the expanded configuration section for a NetNamedPipeBinding.
            Each property is configured with the default value. -->

      <netNamedPipeBinding>
        <binding name="Binding1"
                         maxBufferSize="65536"
                         maxConnections="10">
          <security mode = "None">
          </security>
        </binding >

      </netNamedPipeBinding>
    </bindings>

  </system.serviceModel>
```

Quando si esegue l'esempio, le richieste e le risposte dell'operazione vengono visualizzate nella finestra della console client. Premere INVIO nella finestra del client per arrestare il client.

```console
Add(100,15.99) = 115.99
Subtract(145,76.54) = 68.46
Multiply(9,81.25) = 731.25
Divide(22,7) = 3.14285714285714

Press <ENTER> to terminate client.
```

### <a name="to-set-up-build-and-run-the-sample"></a>Per impostare, compilare ed eseguire l'esempio

1. Assicurarsi che [!INCLUDE[iisver](../../../../includes/iisver-md.md)] sia installato. Per l'attivazione WAS è necessario l'uso di [!INCLUDE[iisver](../../../../includes/iisver-md.md)].

2. Assicurarsi di avere eseguito il [monouso procedura di installazione per gli esempi di Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).

    Inoltre, è necessario installare i componenti di attivazione non HTTP di WCF:

    1. Fare clic sul pulsante **Start** e scegliere **Pannello di controllo**.

    2. Selezionare **programmi e funzionalità**.

    3. Fare clic su **attivare i componenti di Windows o disattivare**.

    4. Espandere la **Microsoft .NET Framework 3.0** nodo e selezionare il **attivazione Non HTTP di Windows Communication Foundation** funzionalità.

3. Configurare il servizio WAS (Windows Process Activation Service) per supportare l'attivazione di named pipe.

    Per maggiore praticità, i due passaggi seguenti vengono implementati in un file batch di nome AddNetPipeSiteBinding.cmd situato nella directory degli esempi.

    1. Per supportare l'attivazione net.pipe, è innanzitutto necessario associare il sito Web predefinito al protocollo net.pipe. A tale fine, usare appcmd.exe, installato con il set di strumenti di gestione di IIS 7.0. Da un prompt dei comandi con privilegi elevati (amministratore), eseguire il comando seguente:

        ```
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site"
        -+bindings.[protocol='net.pipe',bindingInformation='*']
        ```

        > [!NOTE]
        > Questo comando è una singola riga di testo.

        Questo comando aggiunge un'associazione di sito net.pipe al sito Web predefinito.

    2. Anche se tutte le applicazioni all'interno di un sito condividono un'associazione net.pipe comune, ciascuna di esse può abilitare il supporto net.pipe individualmente. Per abilitare net.pipe per l'applicazione /servicemodelsamples, eseguire il comando seguente da un prompt dei comandi con privilegi elevati.

        ```
        %windir%\system32\inetsrv\appcmd.exe set app "Default Web Site/servicemodelsamples" /enabledProtocols:http,net.pipe
        ```

        > [!NOTE]
        > Questo comando è una singola riga di testo.

        Questo comando consente di accedervi usando sia l'applicazione /servicemodelsamples `http://localhost/servicemodelsamples` e `net.tcp://localhost/servicemodelsamples`.

4. Per compilare l'edizione in C# o Visual Basic .NET della soluzione, seguire le istruzioni in [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md).

5. Rimuovere l'associazione del sito net.pipe aggiunta per questo esempio.

    Per maggiore praticità, i due passaggi seguenti vengono implementati in un file batch denominato RemoveNetPipeSiteBinding.cmd posto nella directory degli esempi:

    1. Rimuovere net.tcp dall'elenco dei protocolli abilitati eseguendo il comando seguente da una finestra del prompt dei comandi con privilegi elevati.

        ```
        %windir%\system32\inetsrv\appcmd.exe set app "Default Web Site/servicemodelsamples" /enabledProtocols:http
        ```

        > [!NOTE]
        > Questo comando deve essere immesso come una sola riga del testo.

    2. Rimuovere l'associazione del sito net.tcp eseguendo il comando seguente da un prompt dei comandi con privilegi elevati.

        ```
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site" --bindings.[protocol='net.pipe',bindingInformation='*']
        ```

        > [!NOTE]
        > Questo comando deve essere digitato come una singola riga di testo.

## <a name="see-also"></a>Vedere anche

- [Hosting di AppFabric e salvataggio permanente](https://docs.microsoft.com/previous-versions/appfabric/ff383418(v=azure.10))
