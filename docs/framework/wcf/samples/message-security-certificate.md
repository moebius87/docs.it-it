---
title: Certificato di sicurezza dei messaggi
ms.date: 03/30/2017
helpviewer_keywords:
- WS Security
ms.assetid: 909333b3-35ec-48f0-baff-9a50161896f6
ms.openlocfilehash: c439773449cdb3be4326707a0b408d08afd2c50f
ms.sourcegitcommit: c4e9d05644c9cb89de5ce6002723de107ea2e2c4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/19/2019
ms.locfileid: "65876708"
---
# <a name="message-security-certificate"></a>Certificato di sicurezza dei messaggi
In questo esempio viene illustrato come implementare un'applicazione che utilizza WS-Security con l'autenticazione del certificato X.509 v3 per il client e che richiede l'autenticazione del server utilizzando il certificato X.509 v3 del server. Questo esempio utilizza impostazioni predefinite tali che tutti i messaggi dell'applicazione tra client e server sono firmati e crittografati. In questo esempio si basa sul [WSHttpBinding](../../../../docs/framework/wcf/samples/wshttpbinding.md) ed è costituito da un programma di console client e una libreria di servizi ospitati da Internet Information Services (IIS). Il servizio implementa un contratto che definisce un modello di comunicazione richiesta/risposta.  
  
> [!NOTE]
>  La procedura di installazione e le istruzioni di compilazione per questo esempio si trovano alla fine di questo argomento.  
  
 Nell'esempio viene illustrato il controllo dell'autenticazione mediante la configurazione e il modo in cui ottenere l'identità del chiamante dal contesto di sicurezza, come illustrato nel codice di esempio seguente.  

```csharp
public class CalculatorService : ICalculator  
{  
    public string GetCallerIdentity()  
    {  
        // The client certificate is not mapped to a Windows identity by default.  
        // ServiceSecurityContext.PrimaryIdentity is populated based on the information  
        // in the certificate that the client used to authenticate itself to the service.  
        return ServiceSecurityContext.Current.PrimaryIdentity.Name;  
    }  
    ...  
}  
```  
  
 Il servizio espone un endpoint per la comunicazione con il servizio e un endpoint per esporre il documento WSDL del servizio utilizzando il protocollo WS-MetadataExchange, definito utilizzando il file di configurazione (Web.config). L'endpoint è costituito da un indirizzo, un'associazione e un contratto. L'associazione è configurata con una classe standard [ \<wsHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/wshttpbinding.md) elemento, che usa la sicurezza dei messaggi per impostazione predefinita. Questo esempio imposta l'attributo `clientCredentialType` su Certificato per richiedere l'autenticazione del client.  
  
```xml  
<system.serviceModel>  
    <protocolMapping>  
      <add scheme="http" binding="wsHttpBinding"/>  
    </protocolMapping>  
    <bindings>  
      <wsHttpBinding>  
        <!--   
        This configuration defines the security mode as Message and   
        the clientCredentialType as Certificate.  
        -->  
        <binding>  
          <security mode ="Message">  
            <message clientCredentialType="Certificate" />  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->  
    <behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <serviceMetadata httpGetEnabled="True"/>  
          <serviceDebug includeExceptionDetailInFaults="False" />  
          <!--   
        The serviceCredentials behavior allows one to define a service certificate.  
        A service certificate is used by the service to authenticate itself to its clients and to provide message protection.  
        This configuration references the "localhost" certificate installed during the setup instructions.  
        -->  
          <serviceCredentials>  
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />  
            <clientCertificate>  
              <!--   
            Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate   
            is in the user's Trusted People store, then it will be trusted without performing a  
            validation of the certificate's issuer chain. This setting is used here for convenience so that the   
            sample can be run without having to have certificates issued by a certification authority (CA).  
            This setting is less secure than the default, ChainTrust. The security implications of this   
            setting should be carefully considered before using PeerOrChainTrust in production code.   
            -->  
              <authentication certificateValidationMode="PeerOrChainTrust" />  
            </clientCertificate>  
          </serviceCredentials>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
```  
  
 Il comportamento specifica le credenziali del servizio utilizzate per l'autenticazione del servizio da parte del client. Il nome soggetto del certificato server è specificato nel `findValue` attributo la [ \<serviceCredentials >](../../../../docs/framework/configure-apps/file-schema/wcf/servicecredentials.md) elemento.  
  
```xml  
<!--For debugging purposes, set the includeExceptionDetailInFaults attribute to true.-->  
<behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <serviceMetadata httpGetEnabled="True"/>  
          <serviceDebug includeExceptionDetailInFaults="False" />  
          <!--   
        The serviceCredentials behavior allows one to define a service certificate.  
        A service certificate is used by the service to authenticate itself to its clients and to provide message protection.  
        This configuration references the "localhost" certificate installed during the setup instructions.  
        -->  
          <serviceCredentials>  
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />  
            <clientCertificate>  
              <!--   
            Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate   
            is in the user's Trusted People store, then it will be trusted without performing a  
            validation of the certificate's issuer chain. This setting is used here for convenience so that the   
            sample can be run without having to have certificates issued by a certification authority (CA).  
            This setting is less secure than the default, ChainTrust. The security implications of this   
            setting should be carefully considered before using PeerOrChainTrust in production code.   
            -->  
              <authentication certificateValidationMode="PeerOrChainTrust" />  
            </clientCertificate>  
          </serviceCredentials>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
```  
  
 La configurazione dell'endpoint client è costituita da un indirizzo assoluto per l'endpoint del servizio, l'associazione e il contratto. L'associazione client viene configurata con modalità di sicurezza e autenticazione appropriate. Quando si esegue l'esempio tra più computer, assicurarsi che l'indirizzo dell'endpoint del servizio venga modificato di conseguenza.  
  
```xml  
<system.serviceModel>  
    <client>  
      <!-- Use a behavior to configure the client certificate to present to the service. -->  
      <endpoint address="http://localhost/servicemodelsamples/service.svc" binding="wsHttpBinding" bindingConfiguration="Binding1" behaviorConfiguration="ClientCertificateBehavior" contract="Microsoft.Samples.Certificate.ICalculator"/>  
    </client>  
  
    <bindings>  
      <wsHttpBinding>  
        <!--   
        This configuration defines the security mode as Message and   
        the clientCredentialType as Certificate.  
        -->  
        <binding name="Binding1">  
          <security mode="Message">  
            <message clientCredentialType="Certificate"/>  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
...  
</system.serviceModel>  
```  
  
 L'implementazione client può impostare il certificato tramite il file di configurazione o tramite codice. Nell'esempio seguente viene illustrato come impostare il certificato in un file di configurazione.  
  
```xml  
<system.serviceModel>  
  ...  
  
<behaviors>  
      <endpointBehaviors>  
        <behavior name="ClientCertificateBehavior">  
          <!--   
        The clientCredentials behavior allows one to define a certificate to present to a service.  
        A certificate is used by a client to authenticate itself to the service and provide message integrity.  
        This configuration references the "client.com" certificate installed during the setup instructions.  
        -->  
          <clientCredentials>  
            <clientCertificate findValue="client.com" storeLocation="CurrentUser" storeName="My" x509FindType="FindBySubjectName"/>  
            <serviceCertificate>  
              <!--   
            Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate   
            is in the user's Trusted People store, then it will be trusted without performing a  
            validation of the certificate's issuer chain. This setting is used here for convenience so that the   
            sample can be run without having to have certificates issued by a certificate authority (CA).  
            This setting is less secure than the default, ChainTrust. The security implications of this   
            setting should be carefully considered before using PeerOrChainTrust in production code.   
            -->  
              <authentication certificateValidationMode="PeerOrChainTrust"/>  
            </serviceCertificate>  
          </clientCredentials>  
        </behavior>  
      </endpointBehaviors>  
    </behaviors>  
  
</system.serviceModel>  
```  
  
 Nell'esempio seguente viene illustrato come chiamare il servizio nel programma.  

```csharp
// Create a client.  
CalculatorClient client = new CalculatorClient();  
  
// Call the GetCallerIdentity service operation.  
Console.WriteLine(client.GetCallerIdentity());  
...  
//Closing the client gracefully closes the connection and cleans up resources.  
client.Close();  
```
  
 Quando si esegue l'esempio, le richieste e le risposte dell'operazione vengono visualizzate nella finestra della console client. Premere INVIO nella finestra del client per arrestare il client.  
  
```  
CN=client.com  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
Press <ENTER> to terminate client.  
```  
  
 Il file batch Setup.bat incluso negli esempi relativi alla sicurezza dei messaggi consente di configurare client e server con certificati attinenti per eseguire un'applicazione ospitata che richiede sicurezza basata su certificati. Il file batch può essere eseguito in tre modalità. Per l'esecuzione in singoli computer, digitare **Setup. bat** in un prompt dei comandi sviluppatore per Visual Studio; per la modalità servizio, digitare **Setup. bat service**; e per la modalità client digitare **Setup. bat client**. Utilizzare le modalità client e server quando si esegue l'esempio tra più computer. Per altre informazioni, vedere la procedura di configurazione alla fine di questo argomento. Di seguito viene fornita una breve panoramica delle varie sezioni dei file batch in modo che possano essere modificate per l'esecuzione nella configurazione appropriata:  
  
- Creazione del certificato del client.  
  
     La riga seguente nel file batch crea il certificato client. Il nome client specificato viene utilizzato nel nome del soggetto del certificato creato. Il certificato viene inserito nell'archivio `My` nel percorso `CurrentUser`.  
  
    ```bat
    echo ************  
    echo making client cert  
    echo ************  
    makecert.exe -sr CurrentUser -ss MY -a sha1 -n CN=%CLIENT_NAME% -sky exchange -pe  
    ```  
  
- Installazione del certificato client nell'archivio certificati attendibili del server.  
  
     La riga seguente nel file batch copia il certificato client nell'archivio TrustedPeople del server in modo che il server possa prendere le decisioni pertinenti in tema di attendibilità. Affinché un certificato installato nell'archivio TrustedPeople sia considerato attendibile da un servizio Windows Communication Foundation (WCF), la modalità di convalida del certificato client deve essere impostata su `PeerOrChainTrust` o `PeerTrust`. Per informazioni sull'esecuzione di questa operazione mediante un file di configurazione, vedere l'esempio di configurazione del servizio precedente.  
  
    ```bat
    echo ************  
    echo copying client cert to server's LocalMachine store  
    echo ************  
    certmgr.exe -add -r CurrentUser -s My -c -n %CLIENT_NAME% -r LocalMachine -s TrustedPeople   
    ```  
  
- Creazione del certificato server.  
  
     Le righe seguenti del file batch Setup.bat creano il certificato server da usare.  
  
    ```bat
    echo ************  
    echo Server cert setup starting  
    echo %SERVER_NAME%  
    echo ************  
    echo making server cert  
    echo ************  
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe  
    ```  
  
     La variabile %SERVER_NAME% specifica il nome del server. Il certificato viene archiviato nell'archivio LocalMachine. Se il file batch Setup. bat viene eseguito con un argomento di servizio (ad esempio, **Setup. bat service**) il % nome_server % contiene il nome di dominio completo del computer. In caso contrario, viene usata l'impostazione predefinita localhost.  
  
- Installazione del certificato server nell'archivio certificati attendibili del client.  
  
     La riga seguente copia il certificato server nell'archivio Persone attendibili del client. Questo passaggio è necessario perché certificati generati da Makecert.exe non sono considerati implicitamente attendibili dal sistema client. Se è già disponibile un certificato impostato come radice in un certificato radice client attendibile, ad esempio un certificato rilasciato da Microsoft, il popolamento dell'archivio certificati client con il certificato server non è necessario.  
  
    ```  
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople  
    ```  
  
- Concessione delle autorizzazioni sulla chiave privata del certificato.  
  
     Le righe seguenti nel file Setup. bat rendono il certificato server archiviato nell'archivio LocalMachine accessibile all'account del processo di lavoro ASP.NET.  
  
    ```bat
    echo ************  
    echo setting privileges on server certificates  
    echo ************  
    for /F "delims=" %%i in ('"%ProgramFiles%\ServiceModelSampleTools\FindPrivateKey.exe" My LocalMachine -n CN^=%SERVER_NAME% -a') do set PRIVATE_KEY_FILE=%%i  
    set WP_ACCOUNT=NT AUTHORITY\NETWORK SERVICE  
    (ver | findstr /C:"5.1") && set WP_ACCOUNT=%COMPUTERNAME%\ASPNET  
    echo Y|cacls.exe "%PRIVATE_KEY_FILE%" /E /G "%WP_ACCOUNT%":R  
    iisreset  
    ```  
  
    > [!NOTE]
    >  Se si usa una versione in lingua diversa dall'inglese (Stati Uniti) Stati Uniti di Windows, è necessario modificare il file Setup.bat e sostituire il nome dell'account "NT AUTHORITY\NETWORK SERVICE" con l'equivalente locale.  
  
> [!NOTE]
>  Gli strumenti utilizzati in questo file batch si trovano in C:\Program Files\Microsoft Visual Studio 8\Common7\tools o C:\Program Files\Microsoft SDKs\Windows\v6 .0 \bin. Una di queste directory deve essere nel percorso di sistema. Se si è installato Visual Studio, il modo più semplice per ottenere questa directory nel percorso è aprire il prompt dei comandi per gli sviluppatori per Visual Studio. Fare clic su **avviare**, quindi selezionare **tutti i programmi**, **Visual Studio 2012**, **strumenti**. In questo prompt dei comandi i percorsi adatti sono già configurati. In caso contrario, sarà necessario aggiungere manualmente la directory appropriata al percorso.  
  
> [!IMPORTANT]
>  È possibile che gli esempi siano già installati nel computer. Verificare la directory seguente (impostazione predefinita) prima di continuare.  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  Se questa directory non esiste, andare al [Windows Communication Foundation (WCF) e gli esempi di Windows Workflow Foundation (WF) per .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) per scaricare tutti i Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] esempi. Questo esempio si trova nella directory seguente.  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\WS\MessageSecurity`  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Per impostare, compilare ed eseguire l'esempio  
  
1. Assicurarsi di avere eseguito il [monouso procedura di installazione per gli esempi di Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Per compilare l'edizione in C# o Visual Basic .NET della soluzione, seguire le istruzioni in [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
### <a name="to-run-the-sample-on-the-same-computer"></a>Per eseguire l'esempio nello stesso computer  
  
1. Aprire un prompt dei comandi per gli sviluppatori per Visual Studio con privilegi di amministratore ed eseguire Setup. bat dalla cartella di installazione dell'esempio. In questo modo vengono installati tutti i certificati necessari per l'esecuzione dell'esempio.  
  
    > [!NOTE]
    >  Il file batch Setup. bat è progettato per essere eseguito da un prompt dei comandi di per gli sviluppatori per Visual Studio. e richiede che la variabile di ambiente PATH punti alla directory in cui è installato SDK. Questa variabile di ambiente viene impostata automaticamente in un prompt dei comandi di per gli sviluppatori per Visual Studio (2010).  
  
2. Verificare l'accesso al servizio usando un browser immettendo l'indirizzo `http://localhost/servicemodelsamples/service.svc`.  
  
3. Avviare Client.exe da \client\bin. L'attività del client viene visualizzata nella finestra dell'applicazione console.  
  
4. Se il client e il servizio non è in grado di comunicare, vedere [suggerimenti per la risoluzione dei problemi per gli esempi di WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
### <a name="to-run-the-sample-across-computers"></a>Per eseguire l'esempio tra più computer  
  
1. Creare una directory sul computer del servizio. Creare un'applicazione virtuale denominata servicemodelsamples per questa directory usando lo strumento di gestione di Internet Information Services (IIS).  
  
2. Copiare i file del programma del servizio da \inetpub\wwwroot\servicemodelsamples nella directory virtuale sul computer del servizio. Assicurarsi di copiare i file nella sottodirectory \bin e copiare inoltre i file Setup.bat, Cleanup.bat e ImportClientCert.bat nel computer del servizio.  
  
3. Creare una directory sul client del servizio per i file binari del client.  
  
4. Copiare i file di programma del client nella directory del client sul computer relativo e i file Setup.bat, Cleanup.bat e ImportServiceCert.bat nel client.  
  
5. Nel server, eseguire **Setup. bat service** in un prompt dei comandi sviluppatori per Visual Studio con privilegi di amministratore. In esecuzione **Setup. bat** con il **servizio** argomento consente di creare un certificato di servizio con il nome di dominio completo del computer ed esportare il certificato di servizio in un file denominato CER.  
  
6. Modificare Web. config per riflettere il nuovo nome del certificato (nelle `findValue` attributo la [ \<serviceCertificate >](../../../../docs/framework/configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md)) che corrisponde al nome di dominio completo del computer.  
  
7. Copiare il file Service.cer dalla directory del servizio nella directory del client sul computer relativo.  
  
8. Nel client, eseguire **Setup. bat client** in un prompt dei comandi sviluppatori per Visual Studio con privilegi di amministratore. In esecuzione **Setup. bat** con il **client** argomento crea un certificato client denominato client.com ed Esporta il certificato client in un file denominato CER.  
  
9. Nel file Client.exe.config presente nel computer client modificare il valore dell'indirizzo della definizione dell'endpoint in base al nuovo indirizzo del servizio. Tale operazione viene eseguita sostituendo localhost con il nome di dominio completo del server.  
  
10. Copiare il file Client.cer dalla directory del client nella directory del servizio sul server.  
  
11. Sul client, eseguire Importservicecert. bat in un prompt dei comandi di per gli sviluppatori per Visual Studio con privilegi amministrativi. In questo modo viene importato il certificato del servizio dal file Service.cer nell'archivio CurrentUser - TrustedPeople.  
  
12. Nel server, eseguire importclientcert. bat in un prompt dei comandi di per gli sviluppatori per Visual Studio con privilegi amministrativi. In questo modo viene importato il certificato del client dal file Client.cer nell'archivio LocalMachine - TrustedPeople.  
  
13. Sul computer client avviare Client.exe da una finestra del prompt dei comandi. Se il client e il servizio non è in grado di comunicare, vedere [suggerimenti per la risoluzione dei problemi per gli esempi di WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
### <a name="to-clean-up-after-the-sample"></a>Per eseguire la pulizia dopo l'esempio  
  
- Eseguire Cleanup.bat nella cartella degli esempi dopo che l'esempio è stato completato.  
  
    > [!NOTE]
    >  Questo script non rimuove i certificati del servizio da un client quando si esegue l'esempio tra più computer. Se è stato eseguito gli esempi di Windows Communication Foundation (WCF) che utilizzano certificati in più computer, assicurarsi di cancellare i certificati del servizio che sono stati installati nell'archivio CurrentUser - TrustedPeople. A tale scopo, usare il comando seguente: `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>` Ad esempio: `certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com`.  
