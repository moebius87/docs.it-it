---
title: Specifica del manifesto del provider
ms.date: 03/30/2017
ms.assetid: bb450b47-8951-4f99-9350-26f05a4d4e46
ms.openlocfilehash: 0f3eaa73a26c3f8519e1c168ab2e2968ed4ab28d
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64641171"
---
# <a name="provider-manifest-specification"></a>Specifica del manifesto del provider
Questa sezione illustra come un provider dell'archivio dati può supportare i tipi e le funzioni di tale archivio.  
  
 I servizi di entità funzionano indipendentemente da un provider dell'archivio dati specifico, tuttavia consentono comunque a un provider di dati di definire in modo esplicito l'interazione di modelli, mapping e query con un archivio dati sottostante. Su un piano prettamente pratico, i servizi di entità potrebbero essere destinati solo a un provider di dati o a un archivio dati specifico.  
  
 I tipi supportati dal provider sono supportati direttamente o indirettamente dal database sottostante. Tali tipi no sono necessariamente i tipi di archivio, bensì i tipi usati dal provider per supportare [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]. I tipi di archivio/provider vengono descritti in termini EDM (Entity Data Model).  
  
 I tipi di parametri e i tipi restituiti per le funzioni supportate dall'archivio dati vengono specificati in termini EDM.  
  
## <a name="requirements"></a>Requisiti  
 [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)] e l'archivio dati devono essere in grado di passare i dati da e verso tipi noti senza troncamento o perdita di dati.  
  
 Il manifesto del provider deve essere caricabile dagli strumenti in fase di progettazione senza che sia necessario aprire una connessione all'archivio dati.  
  
 Il [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)] avviene sensibili, ma potrebbe non essere nell'archivio dati sottostante. Quando nel manifesto vengono definiti e usati gli elementi di EDM, ad esempio identificatori e nomi dei tipi, è necessario che venga usata la distinzione tra maiuscole e minuscole di [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]. Se nel manifesto del provider vengono visualizzati elementi dell'archivio dati per cui potrebbe essere rilevata la distinzione tra maiuscole e minuscole, è necessario mantenere tale distinzione nel manifesto del provider.  
  
 [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)] richiede un manifesto del provider per tutti i provider di dati. Se si prova a usare un provider che non dispone di un provider del manifesto con il [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)], si otterrà un errore.  
  
 Nella tabella seguente vengono descritti i tipi di eccezioni che verrebbero generati da [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)] quando l'interazione del provider genera eccezioni:  
  
|Problema|Eccezione|  
|-----------|---------------|  
|Il provider non supporta GetProviderManifest in DbProviderServices.|ProviderIncompatibleException|  
|Manifesto del provider mancante: il provider restituisce `null` se si prova a recuperare il manifesto del provider.|ProviderIncompatibleException|  
|Manifesto del provider non valido: il provider restituisce un XML non valido se si prova a recuperare il manifesto del provider.|ProviderIncompatibleException|  
  
## <a name="scenarios"></a>Scenari  
 Un provider deve supportare gli scenari seguenti:  
  
### <a name="writing-a-provider-with-symmetric-type-mapping"></a>Scrittura di un provider con mapping dei tipi simmetrico  
 È possibile scrivere un provider per il [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)] in cui ogni tipo di archivio esegue il mapping a un solo tipo EDM, indipendentemente dalla direzione del mapping. Per un tipo di provider che presenta un mapping molto semplice che corrisponde a un tipo EDM, è possibile usare una soluzione simmetrica, in quanto il sistema dei tipi è semplice o corrisponde ai tipi EDM.  
  
 È possibile usare la semplicità del dominio e produrre un manifesto del provider dichiarativo statico.  
  
 Scrivere un file XML con due sezioni:  
  
- Un elenco di tipi di provider espresso in termini di "controparte EDM" di una funzione o di un tipo di archivio. I tipi di archivio presentano tipi EDM della controparte. Le funzioni di archivio presentano funzioni EDM corrispondenti. Ad esempio, varchar è un tipo SQL Server ma il tipo EDM corrispondente è stringa.  
  
- Un elenco di funzioni supportate dal provider in cui i tipi di parametri e i tipi restituiti sono espressi in termini EDM.  
  
### <a name="writing-a-provider-with-asymmetric-type-mapping"></a>Scrittura di un provider con mapping dei tipi asimmetrico  
 Quando si scrive un provider dell'archivio dati per [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)], è possibile che per alcuni tipi il mapping da EDM al tipo di provider sia diverso da quello dal provider al tipo EDM. Ad esempio, PrimitiveTypeKind.String EDM potrebbe essere mappato a nvarchar (4000) sul provider, mentre nvarchar (4000) viene mappato a PrimitiveTypeKind.String(MaxLength=4000) EDM.  
  
 Scrivere un file XML con due sezioni:  
  
- Un elenco di tipi di provider espresso in termini EDM e definire il mapping per entrambe le direzioni: EDM a provider e i provider a EDM.  
  
- Un elenco di funzioni supportate dal provider in cui i tipi di parametri e i tipi restituiti sono espressi in termini EDM.  
  
## <a name="provider-manifest-discoverability"></a>Individuabilità del manifesto del provider  
 Il manifesto viene usato indirettamente da diversi tipi di componenti nei servizi di entità, ad esempio strumenti o query, ma più direttamente viene usato dai metadati tramite l'uso del caricatore di metadati dell'archivio dati.  
  
 ![dfb3d02b&#45;7a8c&#45;4d51&#45;ac5a&#45;a73d8aa145e6](../../../../../docs/framework/data/adonet/ef/media/dfb3d02b-7a8c-4d51-ac5a-a73d8aa145e6.gif "dfb3d02b-7a8c-4d51-ac5a-a73d8aa145e6")  
  
 È tuttavia possibile che un provider specificato supporti archivi diversi o versioni diverse dello stesso archivio. È necessario pertanto che per ogni archivio dati supportato dal provider sia indicato un manifesto diverso.  
  
### <a name="provider-manifest-token"></a>Token del manifesto del provider  
 Quando viene aperta una connessione a un archivio dati, il provider può richiedere informazioni tramite query per ottenere il manifesto appropriato. Questa operazione potrebbe non essere possibile negli scenari offline in cui le informazioni di connessione non sono disponibili oppure quando non è possibile eseguire la connessione all'archivio. Identificare il manifesto tramite l'attributo `ProviderManifestToken` dell'elemento `Schema` nel file con estensione ssdl. Non esiste un formato obbligatorio per questo attributo. Il provider sceglie le informazioni minime necessarie che consentono di identificare un manifesto senza dover aprire una connessione all'archivio.  
  
 Ad esempio:  
  
```xml  
<Schema Namespace="Northwind" Provider="System.Data.SqlClient" ProviderManifestToken="2005" xmlns:edm="http://schemas.microsoft.com/ado/2006/04/edm/ssdl" xmlns="http://schemas.microsoft.com/ado/2006/04/edm/ssdl">  
```  
  
## <a name="provider-manifest-programming-model"></a>Modello di programmazione del manifesto del provider  
 I provider derivano da <xref:System.Data.Common.DbXmlEnabledProviderManifest>, che consente loro di specificare in modo dichiarativo i propri manifesti. Nell'illustrazione seguente viene mostrata la gerarchia di classi di un provider:  
  
 ![None](../../../../../docs/framework/data/adonet/ef/media/d541eba3-2ee6-4cd1-88f5-89d0b2582a6c.gif "d541eba3-2ee6-4cd1-88f5-89d0b2582a6c")  
  
### <a name="discoverability-api"></a>API di individuabilità  
 Il manifesto del provider viene caricato dal caricatore dei metadati dell'archivio (StoreItemCollection) tramite una connessione all'archivio dati o un token del manifesto del provider.  
  
#### <a name="using-a-data-store-connection"></a>Uso di una connessione all'archivio dati  
 Quando è disponibile la connessione all'archivio dati, chiamare DbProvderServices.GetProviderManifestToken per restituire il token passato al metodo GetProviderManifest, che restituisce DbProviderManifest. Tale metodo funge da delegato per l'implementazione del provider di GetDbProviderManifestToken.  
  
```  
public string GetProviderManifestToken(DbConnection connection);  
public DbProviderManifest GetProviderManifest(string manifestToken);  
```  
  
#### <a name="using-a-provider-manifest-token"></a>Uso di un token del manifesto del provider  
 Per lo scenario offline il token viene scelto dalla rappresentazione SSDL. SSDL consente di specificare un oggetto ProviderManifestToken (vedere [elemento Schema (SSDL)](/ef/ef6/modeling/designer/advanced/edmx/ssdl-spec#schema-element-ssdl) per altre informazioni). Se ad esempio non è possibile aprire una connessione, in SSDL è disponibile un token del manifesto del provider che specifica informazioni sul manifesto.  
  
```  
public DbProviderManifest GetProviderManifest(string manifestToken);  
```  
  
### <a name="provider-manifest-schema"></a>Schema del manifesto del provider  
 Lo schema di informazioni definito per ogni provider contiene le informazioni statiche che devono essere usate dai metadati:  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<xs:schema elementFormDefault="qualified"  
   xmlns:xs="http://www.w3.org/2001/XMLSchema"  
   targetNamespace="http://schemas.microsoft.com/ado/2006/04/edm/providermanifest"  
   xmlns:pm="http://schemas.microsoft.com/ado/2006/04/edm/providermanifest">  
  
  <xs:element name="ProviderManifest">  
    <xs:complexType>  
      <xs:sequence>  
        <xs:element name="Types" type="pm:TTypes" minOccurs="1" maxOccurs="1" />  
        <xs:element name="Functions" type="pm:TFunctions" minOccurs="0" maxOccurs="1"/>  
      </xs:sequence>  
      <xs:attribute name="Namespace" type="xs:string" use="required"/>  
    </xs:complexType>  
  </xs:element>  
  <xs:complexType name="TVersion">  
    <xs:attribute name="Major" type="xs:int" use="required" />  
    <xs:attribute name="Minor" type="xs:int" use="required" />  
    <xs:attribute name="Build" type="xs:int" use="required" />  
    <xs:attribute name="Revision" type="xs:int" use="required" />  
  </xs:complexType>  
  
  <xs:complexType name="TIntegerFacetDescription">  
    <xs:attribute name="Minimum" type="xs:int" use="optional" />  
    <xs:attribute name="Maximum" type="xs:int" use="optional" />  
    <xs:attribute name="DefaultValue" type="xs:int" use="optional" />  
    <xs:attribute name="Constant" type="xs:boolean" default="false" />  
  </xs:complexType>  
  
  <xs:complexType name="TBooleanFacetDescription">  
    <xs:attribute name="DefaultValue" type="xs:boolean" use="optional" />  
    <xs:attribute name="Constant" type="xs:boolean" default="true" />  
  </xs:complexType>  
  
  <xs:complexType name="TDateTimeFacetDescription">  
    <xs:attribute name="Constant" type="xs:boolean" default="false" />  
  </xs:complexType>  
  
  <xs:complexType name="TFacetDescriptions">  
    <xs:choice maxOccurs="unbounded">  
      <xs:element name="Precision" minOccurs="0" maxOccurs="1" type="pm:TIntegerFacetDescription"/>  
      <xs:element name="Scale" minOccurs="0" maxOccurs="1" type="pm:TIntegerFacetDescription"/>  
      <xs:element name="MaxLength" minOccurs="0" maxOccurs="1" type="pm:TIntegerFacetDescription"/>  
      <xs:element name="Unicode" minOccurs="0" maxOccurs="1" type="pm:TBooleanFacetDescription"/>  
      <xs:element name="FixedLength" minOccurs="0" maxOccurs="1" type="pm:TBooleanFacetDescription"/>  
    </xs:choice>  
  </xs:complexType>  
  
  <xs:complexType name="TType">  
    <xs:sequence>  
      <xs:element name="FacetDescriptions" type="pm:TFacetDescriptions" minOccurs="0" maxOccurs="1"/>  
    </xs:sequence>  
    <xs:attribute name="Name" type="xs:string" use="required"/>  
    <xs:attribute name="PrimitiveTypeKind" type="pm:TPrimitiveTypeKind" use="required" />  
  </xs:complexType>  
  
  <xs:complexType name="TTypes">  
    <xs:sequence>  
      <xs:element name="Type" type="pm:TType" minOccurs="0" maxOccurs="unbounded"/>  
    </xs:sequence>  
  </xs:complexType>  
  
  <xs:attributeGroup name="TFacetAttribute">  
    <xs:attribute name="Precision" type="xs:int" use="optional"/>  
    <xs:attribute name="Scale" type="xs:int" use="optional"/>  
    <xs:attribute name="MaxLength" type="xs:int" use="optional"/>  
    <xs:attribute name="Unicode" type="xs:boolean" use="optional"/>  
    <xs:attribute name="FixedLength" type="xs:boolean" use="optional"/>  
  </xs:attributeGroup>  
  
  <xs:complexType name="TFunctionParameter">  
    <xs:attribute name="Name" type="xs:string" use="required" />  
    <xs:attribute name="Type" type="xs:string" use="required" />  
    <xs:attributeGroup ref="pm:TFacetAttribute" />  
    <xs:attribute name="Mode" type="pm:TParameterDirection" use="required" />  
  </xs:complexType>  
  
  <xs:complexType name="TReturnType">  
    <xs:attribute name="Type" type="xs:string" use="required" />  
    <xs:attributeGroup ref="pm:TFacetAttribute" />  
  </xs:complexType>  
  
  <xs:complexType name="TFunction">  
    <xs:choice minOccurs="0" maxOccurs ="unbounded">  
      <xs:element name ="ReturnType" type="pm:TReturnType" minOccurs="0" maxOccurs="1" />  
      <xs:element name="Parameter" type="pm:TFunctionParameter" minOccurs="0" maxOccurs="unbounded"/>  
    </xs:choice>  
    <xs:attribute name="Name" type="xs:string" use="required" />  
    <xs:attribute name="Aggregate" type="xs:boolean" use="optional" />  
    <xs:attribute name="BuiltIn" type="xs:boolean" use="optional" />  
    <xs:attribute name="StoreFunctionName" type="xs:string" use="optional" />  
    <xs:attribute name="NiladicFunction" type="xs:boolean" use="optional" />  
    <xs:attribute name="ParameterTypeSemantics" type="pm:TParameterTypeSemantics" use="optional" default="AllowImplicitConversion" />  
  </xs:complexType>  
  
  <xs:complexType name="TFunctions">  
    <xs:sequence>  
      <xs:element name="Function" type="pm:TFunction" minOccurs="0" maxOccurs="unbounded"/>  
    </xs:sequence>  
  </xs:complexType>  
  
  <xs:simpleType name="TPrimitiveTypeKind">  
    <xs:restriction base="xs:string">  
      <xs:enumeration value="Binary"/>  
      <xs:enumeration value="Boolean"/>  
      <xs:enumeration value="Byte"/>  
      <xs:enumeration value="Decimal"/>  
      <xs:enumeration value="DateTime"/>  
      <xs:enumeration value="Time"/>  
      <xs:enumeration value="DateTimeOffset"/>          
      <xs:enumeration value="Double"/>  
      <xs:enumeration value="Guid"/>  
      <xs:enumeration value="Single"/>  
      <xs:enumeration value="SByte"/>  
      <xs:enumeration value="Int16"/>  
      <xs:enumeration value="Int32"/>  
      <xs:enumeration value="Int64"/>  
      <xs:enumeration value="String"/>  
    </xs:restriction>  
  </xs:simpleType>  
  
  <xs:simpleType name="TParameterDirection">  
    <xs:restriction base="xs:string">  
      <xs:enumeration value="In"/>  
      <xs:enumeration value="Out"/>  
      <xs:enumeration value="InOut"/>  
    </xs:restriction>  
  </xs:simpleType>  
  
  <xs:simpleType name="TParameterTypeSemantics">  
    <xs:restriction base="xs:string">  
      <xs:enumeration value="ExactMatchOnly" />  
      <xs:enumeration value="AllowImplicitPromotion" />  
      <xs:enumeration value="AllowImplicitConversion" />  
    </xs:restriction>  
  </xs:simpleType>  
</xs:schema>  
```  
  
#### <a name="types-node"></a>Nodo Types  
 Il nodo Types nel manifesto del provider contiene informazioni sugli oggetti Types supportati a livello nativo dall'archivio dati o tramite il provider.  
  
##### <a name="type-node"></a>Nodo Type  
 Ogni nodo Type definisce un tipo di provider in termini di EDM. Il nodo Type descrive il nome del tipo di provider, informazioni correlate al tipo di modello al quale viene mappato, nonché facet per descrivere il mapping dei tipi specifico.  
  
 Per esprimere queste informazioni sui tipi nel manifesto del provider, è necessario che ogni dichiarazione TypeInformation definisca diverse descrizioni di facet per ogni oggetto Type:  
  
|Nome attributo|Tipo di dati|Obbligatorio|Valore predefinito|Descrizione|  
|--------------------|---------------|--------------|-------------------|-----------------|  
|Nome|Stringa|Yes|N/D|Nome del tipo di dati specifico del provider|  
|PrimitiveTypeKind|PrimitiveTypeKind|Yes|N/D|Nome del tipo EDM|  
  
###### <a name="function-node"></a>Nodo Function  
 Ogni oggetto Function definisce una sola funzione disponibile tramite il provider.  
  
|Nome attributo|Tipo di dati|Obbligatorio|Valore predefinito|Descrizione|  
|--------------------|---------------|--------------|-------------------|-----------------|  
|Nome|Stringa|Yes|N/D|Nome/identificatore della funzione|  
|ReturnType|Stringa|No|Void|Il tipo restituito EDM della funzione|  
|Aggregate|Booleano|No|False|Restituisce True se si tratta di una funzione di aggregazione|  
|BuiltIn|Booleano|No|True|Restituisce True se la funzione è inclusa nell'archivio dati|  
|StoreFunctionName|Stringa|No|\<Nome >|Nome della funzione nell'archivio dati.  Consente di eseguire un determinato tipo di reindirizzamento dei nomi delle funzioni.|  
|NiladicFunction|Booleano|No|False|Restituisce True se la funzione non richiede parametri e viene chiamata senza parametri|  
|ParameterType<br /><br /> Semantics|ParameterSemantics|No|AllowImplicit<br /><br /> Conversione|Scelta della modalità con cui la pipeline della query gestisce la sostituzione del tipo di parametro:<br /><br /> -ExactMatchOnly<br />-AllowImplicitPromotion<br />-AllowImplicitConversion|  
  
 **Nodo parametri**  
  
 Ogni funzione presenta una raccolta di uno o più nodi Parameter.  
  
|Nome attributo|Tipo di dati|Obbligatorio|Valore predefinito|Descrizione|  
|--------------------|---------------|--------------|-------------------|-----------------|  
|Nome|Stringa|Yes|N/D|Nome/identificatore del parametro.|  
|Tipo|Stringa|Yes|N/D|Tipo EDM del parametro.|  
|Modalità|Parametro<br /><br /> Direction|Yes|N/D|Direzione del parametro:<br /><br /> -   in<br />-out<br />-inout|  
  
##### <a name="namespace-attribute"></a>Attributo namespace  
 Ogni provider dell'archivio dati deve definire uno spazio dei nomi o un gruppo di spazi dei nomi per le informazioni definite nel manifesto. Tale spazio dei nomi può essere usato nelle query Entity SQL per risolvere nomi di funzioni e tipi. Ad esempio: SqlServer. Lo spazio dei nomi deve essere diverso dallo spazio dei nomi canonico, ovvero EDM, definito dai servizi di entità per funzioni standard che devono essere supportate dalle query Entity SQL.  
  
## <a name="see-also"></a>Vedere anche

- [Scrittura di un provider di dati Entity Framework](../../../../../docs/framework/data/adonet/ef/writing-an-ef-data-provider.md)
