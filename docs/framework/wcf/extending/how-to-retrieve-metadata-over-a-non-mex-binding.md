---
title: "Procedura: Recuperare metadati attraverso un'associazione non MEX"
ms.date: 03/30/2017
ms.assetid: 2292e124-81b2-4317-b881-ce9c1ec66ecb
ms.openlocfilehash: 4a127e3e2283050018705c85606bd7c03c36de8b
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61766779"
---
# <a name="how-to-retrieve-metadata-over-a-non-mex-binding"></a>Procedura: Recuperare metadati attraverso un'associazione non MEX
In questo argomento viene illustrato come recuperare metadati da un endpoint MEX attraverso un'associazione non MEX. Il codice in questo esempio si basa sul [Endpoint di metadati protetto personalizzato](../../../../docs/framework/wcf/samples/custom-secure-metadata-endpoint.md) esempio.  
  
### <a name="to-retrieve-metadata-over-a-non-mex-binding"></a>Per recuperare metadati attraverso un'associazione non MEX  
  
1. Determinare l'associazione utilizzata dall'endpoint MEX. Per i servizi Windows Communication Foundation (WCF), è possibile determinare l'associazione MEX accedendo al file di configurazione del servizio. In questo caso, l'associazione MEX è definita nella configurazione del servizio seguente:  
  
    ```xml  
    <services>  
        <service name="Microsoft.ServiceModel.Samples.CalculatorService"  
                behaviorConfiguration="CalculatorServiceBehavior">  
         <!-- Use the base address provided by the host. -->  
         <endpoint address=""  
           binding="wsHttpBinding"  
           bindingConfiguration="Binding2"  
           contract="Microsoft.ServiceModel.Samples.ICalculator" />  
         <endpoint address="mex"  
           binding="wsHttpBinding"  
           bindingConfiguration="Binding1"  
           contract="IMetadataExchange" />  
         </service>  
     </services>  
     <bindings>  
       <wsHttpBinding>  
         <binding name="Binding1">  
           <security mode="Message">  
             <message clientCredentialType="Certificate" />  
           </security>  
         </binding>  
         <binding name="Binding2">  
           <reliableSession inactivityTimeout="00:01:00" enabled="true" />  
           <security mode="Message">  
             <message clientCredentialType="Certificate" />  
           </security>  
         </binding>  
       </wsHttpBinding>  
     </bindings>  
    ```  
  
2. Nel file di configurazione client, configurare la stessa associazione personalizzata. Qui il client definisce anche un comportamento `clientCredentials` per fornire un certificato da utilizzare per l'autenticazione al servizio quando vengono richiesti metadati dall'endpoint MEX. Quando si utilizza Svcutil.exe per richiedere metadati su un'associazione personalizzata, è necessario aggiungere la configurazione dell'endpoint MEX al file di configurazione per Svcutil.exe (Svcutil.exe.config) e il nome della configurazione dell'endpoint deve corrispondere allo schema URI dell'indirizzo dell'endpoint MEX, come illustrato nel codice seguente:  
  
    ```xml  
    <system.serviceModel>  
      <client>  
        <endpoint name="http"  
                  binding="wsHttpBinding"  
                  bindingConfiguration="Binding1"  
                  behaviorConfiguration="ClientCertificateBehavior"  
                  contract="IMetadataExchange" />  
      </client>  
      <bindings>  
        <wsHttpBinding>  
          <binding name="Binding1">  
            <security mode="Message">  
              <message clientCredentialType="Certificate"/>  
            </security>  
          </binding>  
        </wsHttpBinding>  
      </bindings>  
      <behaviors>  
        <endpointBehaviors>  
          <behavior name="ClientCertificateBehavior">  
            <clientCredentials>  
              <clientCertificate findValue="client.com" storeLocation="CurrentUser" storeName="My" x509FindType="FindBySubjectName" />  
              <serviceCertificate>  
                <authentication certificateValidationMode="PeerOrChainTrust" />  
              </serviceCertificate>  
            </clientCredentials>  
          </behavior>  
        </endpointBehaviors>  
      </behaviors>    
    </system.serviceModel>  
    ```  
  
3. Creare un `MetadataExchangeClient` e chiamare `GetMetadata`. Per eseguire questa operazione, è possibile specificare l'associazione personalizzata nella configurazione oppure nel codice:  
  
    ```  
    // The custom binding is specified in configuration.  
    EndpointAddress mexAddress = new EndpointAddress("http://localhost:8000/ServiceModelSamples/Service/mex");  
  
    MetadataExchangeClient mexClient = new MetadataExchangeClient("http");  
    mexClient.ResolveMetadataReferences = true;  
    MetadataSet mexSet = mexClient.GetMetadata(mexAddress);  
  
    // The custom binding is specified in code.  
    // Specify the Metadata Exchange binding and its security mode.  
    WSHttpBinding mexBinding = new WSHttpBinding(SecurityMode.Message);  
    mexBinding.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;  
  
    // Create a MetadataExchangeClient and set the certificate details.  
    MetadataExchangeClient mexClient = new MetadataExchangeClient(mexBinding);  
    mexClient.SoapCredentials.ClientCertificate.SetCertificate(  
        StoreLocation.CurrentUser, StoreName.My,  
        X509FindType.FindBySubjectName, "client.com");  
    mexClient.SoapCredentials.ServiceCertificate.Authentication.  
        CertificateValidationMode =  
        X509CertificateValidationMode.PeerOrChainTrust;  
    mexClient.SoapCredentials.ServiceCertificate.SetDefaultCertificate(  
        StoreLocation.CurrentUser, StoreName.TrustedPeople,  
        X509FindType.FindBySubjectName, "localhost");  
    MetadataExchangeClient mexClient2 = new MetadataExchangeClient(customBinding);  
    mexClient2.ResolveMetadataReferences = true;  
    MetadataSet mexSet2 = mexClient2.GetMetadata(mexAddress);  
    ```  
  
4. Creare `WsdlImporter` e chiamare `ImportAllEndpoints`, come illustrato nel codice seguente.  
  
    ```  
    WsdlImporter importer = new WsdlImporter(mexSet);  
    ServiceEndpointCollection endpoints = importer.ImportAllEndpoints();  
    ```  
  
5. A questo punto, si ha una raccolta di endpoint del servizio. Per altre informazioni sull'importazione di metadati, vedere [come: Importare metadati negli endpoint servizio](../../../../docs/framework/wcf/feature-details/how-to-import-metadata-into-service-endpoints.md).  
  
## <a name="see-also"></a>Vedere anche

- [Metadati](../../../../docs/framework/wcf/feature-details/metadata.md)
