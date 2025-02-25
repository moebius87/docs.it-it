---
title: Configurazione del rilevamento per un flusso di lavoro
ms.date: 03/30/2017
ms.assetid: 905adcc9-30a0-4918-acd6-563f86db988a
ms.openlocfilehash: d8d3293fd2b271c0e1a00a1fca30d5ecd166df12
ms.sourcegitcommit: d8ebe0ee198f5d38387a80ba50f395386779334f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/05/2019
ms.locfileid: "66690573"
---
# <a name="configuring-tracking-for-a-workflow"></a>Configurazione del rilevamento per un flusso di lavoro

Un flusso di lavoro può essere eseguito in tre modi:

- Ospitato nell'oggetto <xref:System.ServiceModel.Activities.WorkflowServiceHost>

- Eseguito come un oggetto <xref:System.Activities.WorkflowApplication>

- Eseguito direttamente usando l'oggetto <xref:System.Activities.WorkflowInvoker>

A seconda dell'opzione di hosting del flusso di lavoro, un partecipante del rilevamento può essere aggiunto tramite codice o un file di configurazione. In questo argomento viene descritta la configurazione del rilevamento tramite l'aggiunta di un partecipante del rilevamento agli oggetti <xref:System.Activities.WorkflowApplication> e <xref:System.ServiceModel.Activities.WorkflowServiceHost> e viene illustrato come abilitare il rilevamento quando si usa l'oggetto <xref:System.Activities.WorkflowInvoker>.

## <a name="configuring-workflow-application-tracking"></a>Configurazione del rilevamento dell'applicazione flusso di lavoro

Un flusso di lavoro può essere eseguito usando la classe <xref:System.Activities.WorkflowApplication>. In questo argomento viene illustrata la configurazione del rilevamento per un'applicazione flusso di lavoro di [!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)] tramite l'aggiunta di un partecipante del rilevamento all'host del flusso di lavoro <xref:System.Activities.WorkflowApplication>. In questo caso, il flusso di lavoro viene eseguito come un'applicazione flusso di lavoro. Quest'ultima viene configurata tramite codice (anziché tramite un file di configurazione), ovvero un file con estensione exe indipendente che usa la classe <xref:System.Activities.WorkflowApplication>. Il partecipante del rilevamento viene aggiunto come estensione all'istanza <xref:System.Activities.WorkflowApplication>. Tale operazione viene eseguita aggiungendo l'oggetto <xref:System.Activities.Tracking.TrackingParticipant> alla raccolta di estensioni per l'istanza WorkflowApplication.

Per un'applicazione flusso di lavoro, è possibile aggiungere l'estensione di comportamento <xref:System.Activities.Tracking.EtwTrackingParticipant> come mostrato nel codice seguente.

```csharp
LogActivity activity = new LogActivity();

WorkflowApplication instance = new WorkflowApplication(activity);
EtwTrackingParticipant trackingParticipant =
    new EtwTrackingParticipant
{

        TrackingProfile = new TrackingProfile
           {
               Name = "SampleTrackingProfile",
               ActivityDefinitionId = "ProcessOrder",
               Queries = new WorkflowInstanceQuery
               {
                  States = { "*" }
              }
          }
       };
instance.Extensions.Add(trackingParticipant);
```

### <a name="configuring-workflow-service-tracking"></a>Configurazione del rilevamento del servizio del flusso di lavoro

Un flusso di lavoro può essere esposti come un servizio WCF quando ospitati nel <xref:System.ServiceModel.Activities.WorkflowServiceHost> host del servizio. <xref:System.ServiceModel.Activities.WorkflowServiceHost> è un'implementazione specifica di .NET ServiceHost per un servizio basato sul flusso di lavoro. Contenuto della sezione viene illustrato come configurare il rilevamento per un servizio flusso di lavoro di [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] in esecuzione nell'oggetto <xref:System.ServiceModel.Activities.WorkflowServiceHost>. Viene configurato tramite un file Web.config (per un servizio ospitato sul Web) o un file App.config (per un servizio ospitato in un'applicazione autonoma, ad esempio un'applicazione console) specificando un comportamento del servizio oppure, tramite codice, aggiungendo un comportamento specifico del rilevamento alla raccolta <xref:System.ServiceModel.Description.ServiceDescription.Behaviors%2A> per l'host del servizio.

Per un servizio del flusso di lavoro ospitato nel <xref:System.ServiceModel.WorkflowServiceHost>, è possibile aggiungere i <xref:System.Activities.Tracking.EtwTrackingParticipant> usando il <`behavior`> elemento in un file di configurazione, come illustrato nell'esempio seguente.

```xml
<behaviors>
   <serviceBehaviors>
        <behavior>
          <etwTracking profileName="Sample Tracking Profile" />
        </behavior>
   </serviceBehaviors>
<behaviors>
```

In alternativa, per un servizio del flusso di lavoro ospitato nell'oggetto <xref:System.ServiceModel.WorkflowServiceHost>, è possibile aggiungere l'estensione di comportamento <xref:System.Activities.Tracking.EtwTrackingParticipant> tramite il codice. Per aggiungere un partecipante del rilevamento personalizzato, creare una nuova estensione di comportamento e aggiungerla all'oggetto <xref:System.ServiceModel.ServiceHost> come mostrato nel codice di esempio seguente.

> [!NOTE]
> Se si desidera visualizzare codice di esempio che illustra come creare un elemento di comportamento personalizzato che aggiunge un partecipante del rilevamento personalizzato, vedere la [rilevamento](./samples/tracking.md) esempi.

```csharp
ServiceHost svcHost = new ServiceHost(typeof(WorkflowService), new
                                 Uri("http://localhost:8001/Sample"));
EtwTrackingBehavior trackingBehavior =
    new EtwTrackingBehavior
    {
        ProfileName = "Sample Tracking Profile"
    };
svcHost.Description.Behaviors.Add(trackingBehavior);
svcHost.Open();
```

Il partecipante del rilevamento viene aggiunto all'host del servizio del flusso di lavoro come estensione al comportamento.

Nel codice di esempio seguente viene mostrato come leggere un profilo di rilevamento dal file di configurazione.

```csharp
TrackingProfile GetProfile(string profileName, string displayName)
        {
            TrackingProfile trackingProfile = null;
            TrackingSection trackingSection = (TrackingSection)WebConfigurationManager.GetSection("system.serviceModel/tracking");
            if (trackingSection == null)
            {
                return null;
            }

            if (profileName == null)
            {
                profileName = "";
            }

            //Find the profile with the specified profile name in the list of profile found in config
            var match = from p in new List<TrackingProfile>(trackingSection.TrackingProfiles)
                        where (p.Name == profileName) && ((p.ActivityDefinitionId == displayName) || (p.ActivityDefinitionId == "*"))
                        select p;

            if (match.Count() == 0)
            {
                //return an empty profile
                trackingProfile = new TrackingProfile()
                {
                    ActivityDefinitionId = displayName
                };

            }
            else
            {
                trackingProfile = match.First();
            }

            return trackingProfile;
```

In questo codice di esempio viene illustrato come aggiungere un profilo di rilevamento a un host del flusso di lavoro.

```csharp
WorkflowServiceHost workflowServiceHost = serviceHostBase as WorkflowServiceHost;
if (null != workflowServiceHost)
{
    string workflowDisplayName = workflowServiceHost.Activity.DisplayName;
    TrackingProfile trackingProfile = GetProfile(this.profileName, workflowDisplayName);
    workflowServiceHost.WorkflowExtensions.Add(()  => new EtwTrackingParticipant {
        TrackingProfile = trackingProfile
    });
 }
```

> [!NOTE]
> Per altre informazioni sui profili di tracciamento, fare riferimento a [profili di rilevamento](https://go.microsoft.com/fwlink/?LinkId=201310).

### <a name="configuring-tracking-using-workflowinvoker"></a>Configurazione del rilevamento tramite WorkflowInvoker

Per configurare il rilevamento per un flusso di lavoro eseguito usando l'oggetto <xref:System.Activities.WorkflowInvoker>, aggiungere il provider del rilevamento come estensione a un'istanza <xref:System.Activities.WorkflowInvoker>. Esempio di codice seguente proviene il [rilevamento personalizzato](./samples/custom-tracking.md) esempio.

```csharp
WorkflowInvoker invoker = new WorkflowInvoker(BuildSampleWorkflow());
invoker.Extensions.Add(customTrackingParticipant);
invoker.Invoke();
```

### <a name="viewing-tracking-records-in-event-viewer"></a>Visualizzazione dei record di rilevamento in Visualizzatore eventi

Esistono due log del Visualizzatore eventi di particolare interesse per il rilevamento dell'esecuzione di WF: il log analitico e il log debug. Entrambi si trovano all'interno di Microsoft&#124;Windows&#124;nodo di Server applicazioni-applicazioni. I log presenti in questa sezione contengono gli eventi relativi una singola applicazione piuttosto che gli eventi che interessano l'intero sistema.

Gli eventi di traccia di debug vengono scritti nel log di debug. Per raccogliere gli eventi di traccia di debug di WF nel Visualizzatore eventi, abilitare il log di debug.

1. Per aprire il Visualizzatore eventi, fare clic su **avviare**, quindi fare clic su **eseguire.** Nella finestra di dialogo Esegui, digitare `eventvwr`.

2. Nella finestra di dialogo Visualizzatore eventi, espandere la **registri applicazioni e servizi** nodo.

3. Espandere la **Microsoft**, **Windows**, e **Server applicazioni-applicazioni** nodi.

4. Fare doppio clic il **Debug** nodo sotto il **Server applicazioni-applicazioni** nodo e selezionare **Attiva registro**.

5. Eseguire l'applicazione abilitata per il rilevamento per generare gli eventi di rilevamento.

6. Fare doppio clic il **Debug** nodo e selezionare **Aggiorna.** Gli eventi di traccia verranno visualizzati nel riquadro centrale.

In WF 4 è presente un partecipante del rilevamento mediante il quale vengono scritti i record di rilevamento in una sessione ETW (Event Tracing for Windows). Il partecipante del rilevamento ETW viene configurato con un profilo di rilevamento per sottoscrivere i record di rilevamento. Quando il rilevamento è abilitato, vengono generati record di rilevamento di errori su ETW. Gli eventi di rilevamento ETW (in un intervallo da 100 a 113) corrispondenti agli eventi di rilevamento generati dal partecipante del rilevamento ETW vengono scritti nel log analitico.

Per visualizzare i record di rilevamento, attenersi alla seguente procedura.

1. Per aprire il Visualizzatore eventi, fare clic su **avviare**, quindi fare clic su **eseguire.** Nella finestra di dialogo Esegui, digitare `eventvwr`.

2. Nella finestra di dialogo Visualizzatore eventi, espandere la **registri applicazioni e servizi** nodo.

3. Espandere la **Microsoft**, **Windows**, e **Server applicazioni-applicazioni** nodi.

4. Fare doppio clic il **analitico** nodo sotto il **Server applicazioni-applicazioni** nodo e selezionare **Attiva registro**.

5. Eseguire l'applicazione abilitata per il rilevamento per generare record di rilevamento.

6. Fare doppio clic il **analitico** nodo e selezionare **Aggiorna.** I record di rilevamento dovrebbero essere visibili nel riquadro centrale.

L'immagine seguente mostra gli eventi di rilevamento nel Visualizzatore eventi:

![Screenshot della finestra che mostra Visualizzatore eventi i record di rilevamento.](./media/configuring-tracking-for-a-workflow/tracking-event-viewer.png)

### <a name="registering-an-application-specific-provider-id"></a>Registrazione di un ID provider specifico dell'applicazione

Se gli eventi devono essere scritti in un registro applicazioni specifico, attenersi alla seguente procedura per registrare il nuovo manifesto del provider.

1. Dichiarare l'ID provider nel file di configurazione dell'applicazione.

    ```xml
    <system.serviceModel>
        <diagnostics etwProviderId="2720e974-9fe9-477a-bb60-81fe3bf91eec"/>
    </system.serviceModel>
    ```

2. Copiare il file manifesto da %windir%\Microsoft.NET\Framework\\\<versione più recente di [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)]> \Microsoft.Windows.ApplicationServer.Applications.man in un percorso temporaneo e rinominarlo Microsoft.Windows.ApplicationServer.Applications_Provider1.man

3. Modificare il GUID del file manifesto con il nuovo GUID.

    ```xml
    <provider name="Microsoft-Windows-Application Server-Applications" guid="{2720e974-9fe9-477a-bb60-81fe3bf91eec}"
    ```

4. Modificare il nome del provider se non si desidera disinstallare il provider predefinito.

    ```xml
    <provider name="Microsoft-Windows-Application Server-Applications" guid="{2720e974-9fe9-477a-bb60-81fe3bf91eec}"
    ```

5. Se è stato modificato il nome del provider nel passaggio precedente, modificare i nomi dei canali nel file manifesto in base al nuovo nome del provider.

    ```xml
    <channel name="Microsoft-Windows-Application Server-Applications_Provider1/Admin" chid="ADMIN_CHANNEL" symbol="ADMIN_CHANNEL" type="Admin" enabled="false" isolation="Application" message="$(string.MICROSOFT_WINDOWS_APPLICATIONSERVER_APPLICATIONS.channel.ADMIN_CHANNEL.message)" />
    <channel name="Microsoft-Windows-Application Server-Applications_Provider1/Operational" chid="OPERATIONAL_CHANNEL" symbol="OPERATIONAL_CHANNEL" type="Operational" enabled="false" isolation="Application" message="$(string.MICROSOFT_WINDOWS_APPLICATIONSERVER_APPLICATIONS.channel.OPERATIONAL_CHANNEL.message)" />
    <channel name="Microsoft-Windows-Application Server-Applications_Provider1/Analytic" chid="ANALYTIC_CHANNEL" symbol="ANALYTIC_CHANNEL" type="Analytic" enabled="false" isolation="Application" message="$(string.MICROSOFT_WINDOWS_APPLICATIONSERVER_APPLICATIONS.channel.ANALYTIC_CHANNEL.message)" />
    <channel name="Microsoft-Windows-Application Server-Applications_Provider1/Debug" chid="DEBUG_CHANNEL" symbol="DEBUG_CHANNEL" type="Debug" enabled="false" isolation="Application" message="$(string.MICROSOFT_WINDOWS_APPLICATIONSERVER_APPLICATIONS.channel.DEBUG_CHANNEL.message)" />
    <channel name="Microsoft-Windows-Application Server-Applications_Provider1/Perf" chid="PERF_CHANNEL" symbol="PERF_CHANNEL" type="Analytic" enabled="false" isolation="Application" message="$(string.MICROSOFT_WINDOWS_APPLICATIONSERVER_APPLICATIONS.channel.PERF_CHANNEL.message)" />
    ```

6. Generare la DLL di risorse attenendosi ai passaggi seguenti.

    1. Installare Windows SDK. il SDK di Windows include il compilatore di messaggi ([mc.exe](https://go.microsoft.com/fwlink/?LinkId=184606)) e del compilatore di risorse ([rc.exe](https://go.microsoft.com/fwlink/?LinkId=184605)).

    2. In un prompt dei comandi di Windows SDK, eseguire mc.exe nel nuovo file manifesto.

        ```console
        mc.exe Microsoft.Windows.ApplicationServer.Applications_Provider1.man
        ```

    3. Eseguire rc.exe nel file di risorse generato nel passaggio precedente.

        ```console
        rc.exe  Microsoft.Windows.ApplicationServer.Applications_Provider1.rc
        ```

    4. Creare un file cs vuoto denominato NewProviderReg.cs.

    5. Creare una DLL di risorse usando il compilatore C#.

        ```console
        csc /target:library /win32res:Microsoft.Windows.ApplicationServer.Applications_Provider1.res NewProviderReg.cs /out:Microsoft.Windows.ApplicationServer.Applications_Provider1.dll
        ```

    6. Modificare il nome della dll risorse e messaggi nel file manifesto da `Microsoft.Windows.ApplicationServer.Applications.Provider1.man` per il nuovo nome della dll.

        ```xml
        <provider name="Microsoft-Windows-Application Server-Applications_Provider1" guid="{2720e974-9fe9-477a-bb60-81fe3bf91eec}" symbol="Microsoft_Windows_ApplicationServer_ApplicationEvents" resourceFileName="<dll directory>\Microsoft.Windows.ApplicationServer.Applications_Provider1.dll" messageFileName="<dll directory>\Microsoft.Windows.ApplicationServer.Applications_Provider1.dll">
        ```

    7. Uso [wevtutil](https://go.microsoft.com/fwlink/?LinkId=184608) per registrare il manifesto.

        ```console
        wevtutil im Microsoft.Windows.ApplicationServer.Applications_Provider1.man
        ```

## <a name="see-also"></a>Vedere anche

- [Monitoraggio dell'infrastruttura di App di Windows Server](https://go.microsoft.com/fwlink/?LinkId=201273)
- [Monitoraggio delle applicazioni con App Fabric](https://go.microsoft.com/fwlink/?LinkId=201275)
