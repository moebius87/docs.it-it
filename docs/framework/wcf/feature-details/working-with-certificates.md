---
title: Utilizzo dei certificati
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- certificates [WCF]
ms.assetid: 6ffb8682-8f07-4a45-afbb-8d2487e9dbc3
ms.openlocfilehash: 2122213e69512b06d1328272740ac4ffdc36c1eb
ms.sourcegitcommit: 155012a8a826ee8ab6aa49b1b3a3b532e7b7d9bd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2019
ms.locfileid: "66483043"
---
# <a name="working-with-certificates"></a>Utilizzo dei certificati
Per programmare le funzionalità di sicurezza di Windows Communication Foundation (WCF) in genere si usano i certificati digitali X.509. In particolare, questi certificati vengono usati per autenticare client e server, nonché per crittografare e firmare digitalmente i messaggi. Questo argomento fornisce una breve descrizione delle funzionalità relative ai certificati digitali X.509 e illustra come usarle in WCF. Questo argomento contiene inoltre i collegamenti agli argomenti che trattano questi concetti in modo più dettagliato o che descrivono come eseguire attività comuni tramite l'uso di WCF e dei certificati.  
  
 In breve, un certificato digitale è un componente dell'*infrastruttura a chiave pubblica* (PKI, Public Key Infrastructure). Questa infrastruttura è un sistema di certificati digitali, autorità di certificazione e altre autorità di registrazione che verificano e autenticano la validità di ogni parte coinvolta in una transazione elettronica basata sull'uso di crittografia a chiave pubblica. Ogni certificato viene rilasciato da un'autorità di certificazione e presenta un set di campi che contengono determinati dati, ad esempio *soggetto* (l'entità alla quale viene rilasciato il certificato), date di validità (il periodo di validità del certificato), autorità emittente (l'entità che ha emesso il certificato) e chiave pubblica. In WCF, ognuna di queste proprietà viene elaborata come un'attestazione <xref:System.IdentityModel.Claims.Claim>. Esistono due tipi di attestazioni: di identità e di diritto. Per altre informazioni sui certificati X.509, vedere [Certificati di chiave pubblica X.509](/windows/desktop/SecCertEnroll/about-x-509-public-key-certificates). Per altre informazioni sulle attestazioni e l'autorizzazione in WCF, vedere [Gestione di attestazioni e autorizzazioni con il modello di identità](managing-claims-and-authorization-with-the-identity-model.md). Per altre informazioni sull'implementazione di un'infrastruttura PKI, vedere [PKI aziendale con Windows Server 2012 R2 Active Directory certificato Services](https://blogs.technet.microsoft.com/yungchou/2013/10/21/enterprise-pki-with-windows-server-2012-r2-active-directory-certificate-services-part-1-of-2/).  
  
 La funzione principale di un certificato è consentire l'autenticazione dell'identità del proprietario del certificato presso altre entità. Un certificato contiene la *chiave pubblica*del proprietario, mentre il proprietario conserva la chiave privata. La chiave pubblica può essere utilizzata per crittografare i messaggi inviati al proprietario del certificato. In quanto unico detentore della chiave privata, solo il proprietario è in grado di decrittografare questi messaggi.  
  
 I certificati devono essere rilasciati da un autorità di certificazione, che spesso è un'emittente di certificati di terze parti. Nei domini Windows è disponibile un'autorità di certificazione utilizzabile per rilasciare certificati ai computer appartenenti al dominio.  
  
## <a name="viewing-certificates"></a>Visualizzazione dei certificati  
 Quando si utilizzano i certificati, spesso è necessario visualizzarli e analizzarne le proprietà. A tale scopo è possibile ricorrere allo snap-in Microsoft Management Console (MMC), uno strumento di facile utilizzo. Per altre informazioni, vedere [Procedura: Visualizzare i certificati con lo Snap-in MMC](how-to-view-certificates-with-the-mmc-snap-in.md).  
  
## <a name="certificate-stores"></a>Archivi certificati  
 I certificati vengono memorizzati in appositi archivi. Sono disponibili due posizioni principali di archiviazione, ognuna delle quali è composta da più sottoarchivi. Gli amministratori di un computer possono visualizzare entrambi gli archivi principali mediante lo snap-in MMC. Gli altri utenti, invece, sono in grado di visualizzare soltanto l'archivio utente corrente.  
  
- **Archivio locale del computer**. Questo file contiene i certificati utilizzati dai processi del computer, ad esempio ASP.NET. Utilizzare questa posizione per archiviare i certificati utilizzati per l'autenticazione del server presso i client.  
  
- **Archivio dell'utente corrente**. questa posizione in genere viene utilizzata dalle applicazioni interattive per memorizzare i certificati relativi all'utente corrente del computer. Se si crea un'applicazione client, utilizzare questa posizione per archiviare i certificati utilizzati per l'autenticazione degli utenti presso un servizio.  
  
 Ognuno di questi due archivi è composto da più sottoarchivi. Di seguito sono elencati alcuni dei sottoarchivi più importanti quando si programma in WCF:  
  
- **Autorità di certificazione radice attendibili**. i certificati contenuti in questo archivio possono essere utilizzati per creare una catena di certificati che consente di risalire a un certificato di autorità di certificazione di questo archivio.  
  
    > [!IMPORTANT]
    >  Il computer locale considera implicitamente attendibile qualsiasi certificato posizionato in questo archivio, anche se il certificato non è stato rilasciato da un autorità di certificazione di terze parti attendibile. È pertanto necessario garantire che questo archivio contenga soltanto certificati rilasciati da emittenti completamente attendibili, nonché comprendere quali problemi possono verificarsi qualora questa indicazione non venga rispettata.  
  
- **Personale**. questo archivio viene utilizzato per i certificati associati a un utente di un computer. In genere questo archivio viene utilizzato per i certificati rilasciati mediante uno dei certificati di autorità di certificazione contenuti nell'archivio Autorità di certificazione radice disponibile nell'elenco locale. In alternativa, questo archivio può contenere certificati autocertificati ritenuti attendibili da un'applicazione.  
  
 Per altre informazioni sugli archivi certificati, vedere [Archivi certificati](/windows/desktop/secauthn/certificate-stores).  
  
### <a name="selecting-a-store"></a>Scelta di un archivio  
 La scelta della posizione in cui archiviare un certificato dipende dalla modalità di esecuzione del servizio o del client. In particolare, la scelta si basa sulle regole di carattere generale seguenti:  
  
- Se il servizio WCF è ospitato in un servizio Windows, usare l'archivio del **computer locale**. Si noti che per memorizzare certificati in questo archivio è necessario disporre dei privilegi di amministratore.  
  
- Se il servizio o il client è un'applicazione in esecuzione con un account utente, usare l'archivio **utente corrente**.  
  
### <a name="accessing-stores"></a>Accesso agli archivi  
 Analogamente alle cartelle di un computer, anche gli archivi vengono protetti tramite gli elenchi di controllo di accesso (ACL, Access Control List). Quando si crea un servizio ospitato da Internet Information Services (IIS), il processo di ASP.NET viene eseguito con l'account ASP.NET. Tale account deve essere autorizzato ad accedere all'archivio contenente i certificati utilizzati da un servizio. Ogni archivio principale viene protetto mediante un ACL predefinito, che tuttavia può essere modificato. Se si crea un ruolo a parte per accedere a un archivio, a tale ruolo è necessario concedere l'autorizzazione di accesso. Per informazioni su come modificare l'elenco di accesso usando lo strumento WinHttpCertConfig.exe, vedere [come: Creare certificati temporanei da usare durante lo sviluppo](how-to-create-temporary-certificates-for-use-during-development.md). Per altre informazioni sull'uso di certificati client in IIS, vedere la pagina relativa alla [procedura per chiamare un servizio Web tramite un certificato client per l'autenticazione in un'applicazione Web ASP.NET](https://support.microsoft.com/en-us/help/901183/how-to-call-a-web-service-by-using-a-client-certificate-for-authentica).  
  
## <a name="chain-trust-and-certificate-authorities"></a>Catena di trust e autorità di certificazione  
 I certificati vengono creati in una gerarchia dove ogni singolo certificato è collegato alla CA che ha emesso il certificato. Questo collegamento è per il certificato della CA. L'autorità di certificazione del certificato quindi i collegamenti all'autorità di certificazione che ha emesso il certificato della CA originale. Questo processo si ripete fino a raggiungere il certificato della CA radice. Il certificato della CA radice è considerato naturalmente attendibile.  
  
 I certificati digitali vengono usati per autenticare un'entità basandosi su una *catena di trust*. Lo snap-in MMC consente di visualizzare la catena di qualsiasi certificato. A tale scopo, fare doppio clic sul certificato desiderato e quindi sulla scheda **Percorso certificato**. Per altre informazioni sull'importazione di catene di certificati per autorità di certificazione, vedere [come: Specificare la catena di certificati dell'autorità utilizzata per verificare le firme](specify-the-certificate-authority-chain-verify-signatures-wcf.md).  
  
> [!NOTE]
>  Qualsiasi emittente può essere definito come un'autorità radice attendibile. A tale scopo, aggiungere il certificato di tale emittente nell'archivio Autorità di certificazione radice attendibili.  
  
### <a name="disabling-chain-trust"></a>Disabilitazione della catena di trust  
 Quando si crea un nuovo servizio è possibile che si utilizzi un certificato che non è stato rilasciato tramite un certificato radice attendibile oppure che il certificato emittente non sia contenuto nell'archivio Autorità di certificazione radice attendibili. In questo caso, e solo per scopi di sviluppo, è possibile disabilitare temporaneamente il meccanismo che verifica la catena di trust di un certificato. A tale scopo, impostare la proprietà `CertificateValidationMode` su `PeerTrust` oppure su `PeerOrChainTrust`. Queste modalità specificano rispettivamente che il certificato può essere autocertificato (trust peer) o appartenere a una catena di trust. Questa proprietà può essere impostata in una qualsiasi delle classi seguenti:  
  
|Classe|Proprietà|  
|-----------|--------------|  
|<xref:System.ServiceModel.Security.X509ClientCertificateAuthentication>|<xref:System.ServiceModel.Security.X509ClientCertificateAuthentication.CertificateValidationMode%2A?displayProperty=nameWithType>|  
|<xref:System.ServiceModel.Security.X509PeerCertificateAuthentication>|<xref:System.ServiceModel.Security.X509PeerCertificateAuthentication.CertificateValidationMode%2A?displayProperty=nameWithType>|  
|<xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication>|<xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication.CertificateValidationMode%2A?displayProperty=nameWithType>|  
|<xref:System.ServiceModel.Security.IssuedTokenServiceCredential>|<xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CertificateValidationMode%2A?displayProperty=nameWithType>|  
  
 La proprietà può anche essere impostata in configurazione. Gli elementi seguenti vengono utilizzati per specificare la modalità di convalida:  
  
- [\<authentication>](../../../../docs/framework/configure-apps/file-schema/wcf/authentication-of-servicecertificate-element.md)  
  
- [\<peerAuthentication>](../../../../docs/framework/configure-apps/file-schema/wcf/peerauthentication-element.md)  
  
- [\<messageSenderAuthentication>](../../../../docs/framework/configure-apps/file-schema/wcf/messagesenderauthentication-element.md)  
  
## <a name="custom-authentication"></a>Autenticazione personalizzata  
 La proprietà `CertificateValidationMode` consente inoltre di personalizzare la modalità di autenticazione dei certificati. Per impostazione predefinita, il livello è impostato su `ChainTrust`. Per usare il valore <xref:System.ServiceModel.Security.X509CertificateValidationMode.Custom> è necessario impostare anche l'attributo `CustomCertificateValidatorType` sull'assembly e sul tipo usati per convalidare il certificato. Per creare un validator personalizzato è necessario ereditare una classe dalla classe astratta <xref:System.IdentityModel.Selectors.X509CertificateValidator>.  
  
 Quando si crea un autenticatore personalizzato è fondamentale eseguire l'override del metodo <xref:System.IdentityModel.Selectors.X509CertificateValidator.Validate%2A>. Per un esempio di autenticazione personalizzata, vedere l'esempio [Validator del certificato X.509](../../../../docs/framework/wcf/samples/x-509-certificate-validator.md). Per altre informazioni, vedere [Credenziale personalizzata e convalida della credenziale](../../../../docs/framework/wcf/extending/custom-credential-and-credential-validation.md).  
  
## <a name="using-the-powershell-new-selfsignedcertificate-cmdlet-to-build-a-certificate-chain"></a>Usare il Cmdlet di Powershell New-SelfSignedCertificate per compilare una catena di certificati  
 Il cmdlet di Powershell New-SelfSignedCertificate crea certificati X.509 e coppie di chiavi pubblica/chiave private. La chiave privata può essere salvata su disco e quindi utilizzata per rilasciare e firmare nuovi certificati, simulando in questo modo una gerarchia di certificati concatenati. Il cmdlet è progettato per l'utilizzo solo come supporto durante lo sviluppo di servizi e non dovrà mai essere usato per creare i certificati per distribuzione vera e propria. Quando si sviluppa un servizio WCF, utilizzare la procedura seguente per compilare una catena di trust con il cmdlet New-SelfSignedCertificate.  
  
#### <a name="to-build-a-chain-of-trust-with-the-new-selfsignedcertificate-cmdlet"></a>Per compilare una catena di trust con il cmdlet New-SelfSignedCertificate  
  
1. Creare un certificato (autofirmato) dell'autorità radice temporanea usando il cmdlet New-SelfSignedCertificate. Salvare la chiave privata su disco.  
  
2. Utilizzare il nuovo certificato per rilasciare un altro certificato contenente la chiave pubblica.  
  
3. Importare il certificato dell'autorità radice nell'archivio Autorità di certificazione radice attendibili.  
  
4. Per istruzioni dettagliate, vedere [come: Creare certificati temporanei da usare durante lo sviluppo](how-to-create-temporary-certificates-for-use-during-development.md).  
  
## <a name="which-certificate-to-use"></a>Scelta del certificato da utilizzare  
 Quando si utilizzano i certificati spesso sorgono dubbi su quale certificato scegliere e sul motivo per cui sceglierlo. La soluzione a questi dubbi varia a seconda che la programmazione riguardi un client o un servizio. Le informazioni seguenti rappresentano una linea guida generale e non forniscono una risposta esauriente a questi dubbi.  
  
### <a name="service-certificates"></a>Certificati di servizio  
 Lo scopo principale dei certificati di servizio è autenticare il server presso i client. Uno dei primi controlli svolti durante l'autenticazione di un server presso un client consiste nel confrontare il valore del campo **Soggetto** con l'Uniform Resource Identifier (URI) usato per contattare il servizio. In particolare, è necessario che i DNS di entrambi corrispondano fra loro. Ad esempio, se l'URI del servizio è `http://www.contoso.com/endpoint/` il **Subject** campo deve contenere anche il valore `www.contoso.com`.  
  
 Si noti che il campo può contenere più valori, ognuno avente un prefisso iniziale che ne indica il valore. In genere, l'inizializzazione è "CN" per il nome comune, ad esempio, `CN = www.contoso.com`. È inoltre possibile che il campo **Soggetto** sia vuoto. In tal caso, il campo **Nome alternativo soggetto** può contenere il valore **Nome DNS**.  
  
 Si noti inoltre che il valore del campo **Scopi designati** del certificato deve includere un valore appropriato, ad esempio "Autenticazione server" o "Autenticazione client".  
  
### <a name="client-certificates"></a>Certificati client  
 Anziché essere rilasciati da un'autorità di certificazione di terze parti, in genere i certificati client vengono memorizzati da un'autorità radice nell'archivio Personale dell'utente corrente. Il campo Scopi designati in questo caso viene impostato su "Autenticazione client". Un client può utilizzare un certificato client quando è necessario eseguire l'autenticazione reciproca.  
  
## <a name="online-revocation-and-offline-revocation"></a>Revoca online e revoca offline  
  
### <a name="certificate-validity"></a>Validità del certificato  
 Ogni certificato è valido solo per un determinato periodo di tempo, detto *periodo di validità*. Il periodo di validità è definito in base ai campi **Valido dal** e **Valido fino al** di un certificato X.509. Durante l'autenticazione questi due campi vengono verificati per stabilire se il certificato è nel periodo di validità.  
  
### <a name="certificate-revocation-list"></a>Elenco di revoche di certificati (CRL, Certificate Revocation List)  
 Durante il periodo di validità, l'autorità di certificazione può revocare un certificato in qualsiasi momento. Ciò può verificarsi per vari motivi, ad esempio se la chiave privata del certificato viene compromessa.  
  
 In questo caso, tutti i certificati appartenenti alle catene di trust aventi origine dal certificato revocato sono anch'essi considerati non validi e durante le procedure di autenticazione vengono considerati non attendibili. Per determinare quali certificati sono stati revocati, ogni emittente pubblica un *elenco di revoche di certificati* (CRL, Certificate Revocation List) dotato di timestamp e datestamp. Questo elenco può essere controllato tramite la revoca online oppure la revoca offline impostando la proprietà `RevocationMode` o la proprietà `DefaultRevocationMode` delle classi seguenti su uno dei valori dell'enumerazione <xref:System.Security.Cryptography.X509Certificates.X509RevocationMode>: <xref:System.ServiceModel.Security.X509ClientCertificateAuthentication>, <xref:System.ServiceModel.Security.X509PeerCertificateAuthentication>, <xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication> e <xref:System.ServiceModel.Security.IssuedTokenServiceCredential>. Il valore predefinito di tutte le proprietà è `Online`.  
  
 La modalità può inoltre essere impostata nella configurazione tramite l'attributo `revocationMode` sia di [\<authentication>](../../../../docs/framework/configure-apps/file-schema/wcf/authentication-of-clientcertificate-element.md) (di [\<serviceBehaviors>](../../../../docs/framework/configure-apps/file-schema/wcf/servicebehaviors.md)) che di [\<authentication>](../../../../docs/framework/configure-apps/file-schema/wcf/authentication-of-clientcertificate-element.md) (di [\<endpointBehaviors>](../../../../docs/framework/configure-apps/file-schema/wcf/endpointbehaviors.md)).  
  
## <a name="the-setcertificate-method"></a>Metodo SetCertificate  
 In WCF spesso occorre specificare un certificato o un set di certificati che un servizio o un client deve usare per autenticare, crittografare o firmare digitalmente un messaggio. Questa operazione può essere eseguita a livello di programmazione mediante il metodo `SetCertificate` di varie classi che rappresentano i certificati X.509. Le classi seguenti utilizzano il metodo `SetCertificate` per specificare un certificato.  
  
|Classe|Metodo|  
|-----------|------------|  
|<xref:System.ServiceModel.Security.PeerCredential>|<xref:System.ServiceModel.Security.PeerCredential.SetCertificate%2A>|  
|<xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential>|<xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A>|  
|<xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential>|<xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential.SetCertificate%2A>|  
|<xref:System.ServiceModel.Security.X509CertificateInitiatorServiceCredential>|  
|<xref:System.ServiceModel.Security.X509CertificateInitiatorServiceCredential.SetCertificate%2A>|  
  
 Il metodo `SetCertificate` si basa sulla definizione di una posizione di archivio, di un archivio, di un tipo "di ricerca" (ovvero il parametro `x509FindType` )" che specifica un campo del certificato e un valore da ricercare nel campo. Ad esempio, nel codice seguente viene creata un'istanza della classe <xref:System.ServiceModel.ServiceHost> e viene usato il metodo `SetCertificate` per impostare il certificato del servizio usato per autenticare il servizio presso i client.  
  
 [!code-csharp[c_WorkingWithCertificates#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_workingwithcertificates/cs/source.cs#1)]
 [!code-vb[c_WorkingWithCertificates#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_workingwithcertificates/vb/source.vb#1)]  
  
### <a name="multiple-certificates-with-the-same-value"></a>Certificati aventi lo stesso valore  
 Un archivio può contenere più certificati aventi lo stesso nome del soggetto. Ciò significa che se si imposta il tipo `x509FindType` su <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindBySubjectName> o <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindBySubjectDistinguishedName> e più certificati dispongono dello stesso valore, viene generata un'eccezione. Infatti, non esiste alcun modo per distinguere quale certificato è richiesto. Per risolvere questo problema, impostare la proprietà `x509FindType` su <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindByThumbprint>. Il campo dell'identificazione personale ("Thumbprint") contiene infatti un valore univoco che può essere utilizzato per individuare un certificato specifico all'interno di un archivio. Tuttavia, ciò comporta uno svantaggio: se il certificato viene revocato o rinnovato, il metodo `SetCertificate` ha esito negativo poiché in questi casi l'identificazione personale viene rispettivamente eliminata o alterata. Oppure, se il certificato non è più valido, l'autenticazione ha esito negativo. Per risolvere questo problema è possibile impostare il parametro `x590FindType` su <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindByIssuerName> e specificare quindi nome dell'emittente. Se non è richiesto alcun emittente specifico, è anche possibile impostare uno degli altri valori dell'enumerazione <xref:System.Security.Cryptography.X509Certificates.X509FindType>, ad esempio <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindByTimeValid>.  
  
## <a name="certificates-in-configuration"></a>Impostazione dei certificati in configurazione  
 I certificati possono anche essere impostati in configurazione. Se si crea un servizio, le credenziali, compresi i certificati, vengono specificate in [\<serviceBehaviors>](../../../../docs/framework/configure-apps/file-schema/wcf/servicebehaviors.md). Quando si programma un client, i certificati vengono specificati in [\<endpointBehaviors>](../../../../docs/framework/configure-apps/file-schema/wcf/endpointbehaviors.md).  
  
## <a name="mapping-a-certificate-to-a-user-account"></a>Mapping di un certificato a un account utente  
 Una funzionalità di IIS e di Active Directory è la possibilità di eseguire il mapping di un certificato a un account utente di Windows. Per altre informazioni sulla funzionalità, vedere la pagina relativa al [mapping dei certificati agli account utente](https://go.microsoft.com/fwlink/?LinkId=88917).  
  
 Per altre informazioni sull'uso della funzionalità di mapping di Active Directory, vedere [Mapping di certificati client con il mapping del servizio directory](https://go.microsoft.com/fwlink/?LinkId=88918).  
  
 Se questa funzionalità è abilitata è possibile impostare la proprietà <xref:System.ServiceModel.Security.X509ClientCertificateAuthentication.MapClientCertificateToWindowsAccount%2A> della classe <xref:System.ServiceModel.Security.X509ClientCertificateAuthentication> su `true`. Nella configurazione è possibile impostare su `true` l'attributo `mapClientCertificateToWindowsAccount` dell'elemento [\<authentication>](../../../../docs/framework/configure-apps/file-schema/wcf/authentication-of-servicecertificate-element.md), come mostrato nel codice seguente.  
  
```xml  
<serviceBehaviors>  
 <behavior name="MappingBehavior">  
  <serviceCredentials>  
   <clientCertificate>  
    <authentication certificateValidationMode="None" mapClientCertificateToWindowsAccount="true" />  
   </clientCertificate>  
  </serviceCredentials>  
 </behavior>  
</serviceBehaviors>  
```  
  
 L'esecuzione del mapping di un certificato X.509 al token che rappresenta un account utente di Windows è considerata un'elevazione dei privilegi perché, una volta eseguito tale mapping, il token di Windows può essere utilizzato per accedere alle risorse protette. Pertanto, il criterio del dominio richiede che il certificato X.509 sia conforme al proprio criterio prima di eseguire il mapping. Il pacchetto di sicurezza *SChannel* applica questo requisito.  
  
 Quando si usa [!INCLUDE[netfx35_long](../../../../includes/netfx35-long-md.md)] o versioni successive, WCF garantisce che il certificato sia conforme al criterio di dominio prima che venga mappato a un account di Windows.  
  
 Nella prima versione di WCF il mapping viene eseguito senza prendere in considerazione il criterio del dominio. È pertanto possibile che le applicazioni che funzionano correttamente con la prima versione presentano problemi se il mapping viene abilitato e il certificato X.509 non soddisfa il criterio del dominio.  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.ServiceModel.Channels>
- <xref:System.ServiceModel.Security>
- <xref:System.ServiceModel>
- <xref:System.Security.Cryptography.X509Certificates.X509FindType>
- [Protezione di servizi e client](../../../../docs/framework/wcf/feature-details/securing-services-and-clients.md)
