---
title: Blocco della sicurezza delle informazioni personali
ms.date: 03/30/2017
ms.assetid: c44fb338-9527-4dd0-8607-b8787d15acb4
ms.openlocfilehash: 18095845db094f0911578816e6bacdb7f3776156
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64664777"
---
# <a name="pii-security-lockdown"></a>Blocco della sicurezza delle informazioni personali
In questo esempio viene illustrato come controllare diverse funzionalità correlate alla sicurezza di un servizio Windows Communication Foundation (WCF) da:  
  
- Crittografando informazioni riservate nel file di configurazione di un servizio.  
  
- Bloccando gli elementi nel file di configurazione in modo che le sottodirectory annidate del servizio non possano eseguire l'override delle impostazioni.  
  
- Controllando la registrazione delle informazioni personali (PII) nei log di traccia e dei messaggi.  
  
> [!IMPORTANT]
>  È possibile che gli esempi siano già installati nel computer. Verificare la directory seguente (impostazione predefinita) prima di continuare.  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  Se questa directory non esiste, andare al [Windows Communication Foundation (WCF) e gli esempi di Windows Workflow Foundation (WF) per .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) per scaricare tutti i Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] esempi. Questo esempio si trova nella directory seguente.  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\SecurityLockdown`  
  
## <a name="discussion"></a>Discussione  
 Queste funzionalità possono essere usate separatamente o tutte insieme per controllare gli aspetti della sicurezza di un servizio. Non è una guida completa alla protezione di un servizio WCF.  
  
 I file di configurazione di .NET Framework possono contenere informazioni riservate, ad esempio stringhe di connessione per connettersi ai database. Negli scenari condivisi, ospitati da Web, potrebbe essere auspicabile crittografare queste informazioni nel file di configurazione per un servizio, in modo che i dati contenuti all'interno del file di configurazione siano protetti dagli osservatori esterni. Nella versione 2.0 e successive di .NET Framework è possibile crittografare parti del file di configurazione usando la DPAPI (Windows Data Protection API) o il provider di crittografia RSA. L'aspnet_regiis.exe che usa DPAPI o RSA è in grado di crittografare parti selezionate di un file di configurazione.  
  
 Negli scenari ospitati da Web è possibile che alcuni servizi siano all'interno di sottodirectory di altri servizi. La semantica predefinita per determinare i valori di configurazione consente ai file di configurazione che si trovano nelle directory annidate di eseguire l'override dei valori di configurazione della directory padre. In alcune situazioni questo può essere inaccettabile per molteplici ragioni. Supporta la configurazione del servizio WCF il blocco dei valori di configurazione in modo che la configurazione annidata genera eccezioni quando viene eseguito un servizio annidato utilizzando l'override di valori di configurazione.  
  
 In questo esempio viene illustrato come controllare la registrazione di informazioni personali nei registri di traccia e dei messaggi, come il nome utente e la password. Per impostazione predefinita, la registrazione di informazioni personali note è disattivata. Tuttavia in alcune situazioni la registrazione delle informazioni personali può essere importante per eseguire il debug di un'applicazione. In questo esempio si basa sul [introduttiva](../../../../docs/framework/wcf/samples/getting-started-sample.md). Questo esempio usa inoltre la registrazione di traccia e dei messaggi. Per altre informazioni, vedere la [traccia e registrazione dei messaggi](../../../../docs/framework/wcf/samples/tracing-and-message-logging.md) esempio.  
  
## <a name="encrypting-configuration-file-elements"></a>Crittografia degli elementi del file di configurazione  
 Per motivi di sicurezza in un ambiente di hosting Web condiviso, potrebbe essere auspicabile crittografare alcuni elementi di configurazione, ad esempio le stringhe di connessione al database che potrebbero contenere informazioni riservate. Un elemento di configurazione può essere crittografato usando lo strumento aspnet_regiis.exe situato nella cartella .NET Framework, ad esempio %WINDIR%\Micrsoft.NET\Framework\v4.0.20728.  
  
#### <a name="to-encrypt-the-values-in-the-appsettings-section-in-webconfig-for-the-sample"></a>Per crittografare i valori nella sezione appSettings di Web.config per l'esempio  
  
1. Aprire un prompt dei comandi tramite Start -> Esegui... Digitare `cmd` e fare clic su **OK**.  
  
2. Spostarsi alla directory .NET Framework corrente eseguendo il comando seguente: `cd %WINDIR%\Microsoft.NET\Framework\v4.0.20728`.  
  
3. Crittografare le impostazioni di configurazione appSettings nella cartella Web.config eseguendo il comando seguente: `aspnet_regiis -pe "appSettings" -app "/servicemodelsamples" -prov "DataProtectionConfigurationProvider"`.  
  
 Altre informazioni sulla crittografia delle sezioni del file di configurazione sono reperibile leggendo una procedura su DPAPI nella configurazione di ASP.NET ([Building Secure ASP.NET Applications: L'autenticazione, autorizzazione e comunicazioni protette](https://go.microsoft.com/fwlink/?LinkId=95137)) e procedure su RSA nella configurazione di ASP.NET ([How To: Crittografare le sezioni di configurazione in ASP.NET 2.0 utilizzando RSA](https://go.microsoft.com/fwlink/?LinkId=95138)).  
  
## <a name="locking-configuration-file-elements"></a>Blocco degli elementi del file di configurazione  
 Negli scenari ospitati da Web è possibile che alcuni servizi siano all'interno di sottodirectory di altri servizi. In queste situazioni, i valori di configurazione per il servizio nella sottodirectory vengono calcolati esaminando i valori in Machine.config e unendoli successivamente con i file Web.config delle directory padre, facendo scorrere verso il basso l'albero di directory e unendo infine il file Web.config alla directory che contiene il servizio. Il comportamento predefinito della maggior parte degli elementi di configurazione è di consentire che i file di configurazione presenti nelle sottodirectory eseguano l'override dei valori impostati nelle directory padre. In alcune situazioni potrebbe essere auspicabile impedire che i file di configurazione presenti nelle sottodirectory eseguano l'override dei valori impostati nella configurazione della directory padre.  
  
 .NET Framework fornisce un modo per bloccare gli elementi del file di configurazione in modo che le configurazioni che eseguono l'override degli elementi di configurazione bloccati generino eccezioni in fase di esecuzione.  
  
 Un elemento di configurazione può essere bloccato specificando l'attributo `lockItem` per un nodo nel file di configurazione. Per bloccare ad esempio il nodo CalculatorServiceBehavior nel file di configurazione in modo che i servizi calcolatrice nei file di configurazione annidati non possano modificare il comportamento, è possibile usare la configurazione seguente.  
  
```xml  
<configuration>  
   <system.serviceModel>  
      <behaviors>   
          <serviceBehaviors>   
             <behavior name="CalculatorServiceBehavior" lockItem="true">   
               <serviceMetadata httpGetEnabled="True"/>   
               <serviceDebug includeExceptionDetailInFaults="False" />   
             </behavior>   
          </serviceBehaviors>   
       </behaviors>   
    </system.serviceModel>  
</configuration>  
```  
  
 Il blocco degli elementi di configurazione può essere eseguito a livello più specifico. È possibile specificare un elenco di elementi come valore per gli oggetti `lockElements` per bloccare un set di elementi all'interno di una raccolta di sottoelementi. È possibile specificare un elenco di attributi come valore agli `lockAttributes` per bloccare un gruppo di attributi all'interno di un elemento. È possibile bloccare un'intera raccolta di elementi o di attributi, eccetto un elenco selezionato, specificando gli `lockAllElementsExcept` o gli attributi `lockAllAttributesExcept` su un nodo.  
  
## <a name="pii-logging-configuration"></a>Configurazione della registrazione di informazioni personali  
 La registrazione di informazioni personali viene controllata da due opzioni: un'impostazione a livello di computer situata in Machine.config che consente a un amministratore del computer di autorizzare o negare la registrazione di informazioni personali e un'impostazione dell'applicazione che consente a un amministratore dell'applicazione di modificare le autorizzazioni di registrazione di informazioni personali per ogni origine nei file Web.config o App.config.  
  
 L'impostazione a livello di computer viene controllata impostando `enableLoggingKnownPii` su `true` o `false`, nell'elemento `machineSettings` in Machine.config. Ad esempio, gli elementi seguenti consentono alle applicazioni di attivare la registrazione delle informazioni personali.  
  
```xml  
<configuration>  
    <system.serviceModel>  
        <machineSettings enableLoggingKnownPii="true" />  
    </system.serviceModel>  
</configuration>  
```  
  
> [!NOTE]
>  Il file Machine.config ha un percorso predefinito: %WINDIR%\Microsoft.NET\Framework\v2.0.50727\CONFIG.  
  
 Se l'attributo `enableLoggingKnownPii` non è presente in Machine.config, la registrazione delle informazioni personali non è consentita.  
  
 È possibile abilitare la registrazione delle informazioni personali in un'applicazione impostando l'attributo `logKnownPii` dell'elemento di origine su `true` o `false` nei file Web.config o App.config. Ad esempio, gli elementi seguenti abilitano la registrazione delle informazioni personali per la registrazione di traccia e dei messaggi.  
  
```xml  
<configuration>  
    <system.diagnostics>  
        <sources>  
            <source name="System.ServiceModel.MessageLogging" logKnownPii="true">  
                <listeners>   
                ...   
                </listeners>  
            </source>  
            <source name="System.ServiceModel" switchValue="Verbose, ActivityTracing">  
            <listeners>  
        ...   
            </listeners>  
            </source>  
        </sources>  
    </system.diagnostics>  
</configuration>  
```  
  
 Se l'attributo `logKnownPii` non è stato specificato, le informazioni personali non vengono registrate.  
  
 Le informazioni personali vengono registrate solo se `enableLoggingKnownPii` è impostato su `true` e `logKnownPii` è impostato su `true`.  
  
> [!NOTE]
>  System.Diagnostics ignora tutti gli attributi di tutte le origini tranne la prima elencata nel file di configurazione. Aggiungendo l'attributo `logKnownPii` alla seconda origine nel file di configurazione non ha alcun effetto.  
  
> [!IMPORTANT]
>  Per eseguire questo esempio è necessario modificare manualmente Machine.config. È necessario fare attenzione nel modificare Machine.config, visto che valori o sintassi errati possono impedire l'esecuzione di tutte le applicazioni .NET Framework.  
  
 È inoltre possibile crittografare gli elementi del file di configurazione usando DPAPI e RSA. Per altre informazioni, vedere i collegamenti che seguono.  
  
- [Creazione di applicazioni ASP.NET protette: L'autenticazione, autorizzazione e comunicazioni protette](https://go.microsoft.com/fwlink/?LinkId=95137)  
  
- [Procedura: Crittografare le sezioni di configurazione in ASP.NET 2.0 utilizzando RSA](https://go.microsoft.com/fwlink/?LinkId=95138)  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Per impostare, compilare ed eseguire l'esempio  
  
1. Assicurarsi di avere eseguito il [monouso procedura di installazione per gli esempi di Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Modificare Machine.config per impostare l'attributo `enableLoggingKnownPii` su `true`, aggiungendo i nodi padre se necessario.  
  
3. Per compilare l'edizione in C# o Visual Basic .NET della soluzione, seguire le istruzioni in [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
4. Per eseguire l'esempio in una configurazione singola o tra computer, seguire le istruzioni in [esegue gli esempi di Windows Communication Foundation](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
#### <a name="to-clean-up-the-sample"></a>Per eseguire la pulizia dell'esempio  
  
1. Modificare Machine.config per impostare l'attributo `enableLoggingKnownPii` su `false`.  
  
## <a name="see-also"></a>Vedere anche

- [Esempi di monitoraggio di AppFabric](https://go.microsoft.com/fwlink/?LinkId=193959)
