---
title: Classe MissingMetadataException (.NET Native)
ms.date: 03/30/2017
ms.assetid: 408f25c4-6d60-475c-92b1-7b52b777c6db
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 079718c96a4818285c0b469da9f23213a6915c19
ms.sourcegitcommit: 7e129d879ddb42a8b4334eee35727afe3d437952
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/23/2019
ms.locfileid: "66052511"
---
# <a name="missingmetadataexception-class-net-native"></a>Classe MissingMetadataException (.NET Native)

**.NET per App di Windows per Windows 10, solo .NET Native**

Eccezione generata quando la reflection viene usata per recuperare i metadati che non sono presenti.

**Spazio dei nomi:** System.Reflection

> [!IMPORTANT]
> Il `MissingMetadataException` classe è destinata esclusivamente per uso interno dalla catena di strumenti .NET Native. La classe non può essere usata in codice di terze parti ed è preferibile evitare di gestire l'eccezione nel codice dell'applicazione. Al contrario, eliminare l'eccezione aggiungendo le voci al [file delle direttive di runtime](../../../docs/framework/net-native/runtime-directives-rd-xml-configuration-file-reference.md). Per altre informazioni, vedere la sezione Osservazioni.

## <a name="syntax"></a>Sintassi

[!code-csharp[ProjectN#4](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/missingmetadataexception_syntax1.cs#4)]

Si noti che la classe `MissingMetadataException` deriva da <xref:System.TypeAccessException>.

La classe `MissingMetadataException` ha i seguenti membri:

## <a name="constructors"></a>Costruttori

|Costruttore|Descrizione|
|-----------------|-----------------|
|`public MissingMetadataException()`|Inizializza una nuova istanza della classe `MissingMetadataException` usando un messaggio fornito dal sistema che descrive l'errore.<br /><br /> Questo costruttore è per uso interno da parte la catena di strumenti .NET Native solo.|
|`public MissingMetadataException(String message)`|Inizializza una nuova istanza della classe `MissingMetadataException` con un messaggio di errore specificato.<br /><br /> Questo costruttore è per uso interno da parte la catena di strumenti .NET Native solo.|

## <a name="properties"></a>Proprietà

|Proprietà|Descrizione|
|--------------|-----------------|
|`public IDictionary Data { get; }`|Ottiene una raccolta di coppie chiave-valore che fornisce informazioni aggiuntive definite dall'utente relative all'eccezione. Ereditato da <xref:System.Exception?displayProperty=nameWithType>.|
|`public string HelpLink { get; set; }`|Ottiene o imposta un collegamento al file della Guida associato all'eccezione. Ereditato da <xref:System.Exception?displayProperty=nameWithType>.|
|`public int HResult { get; protected set; }`|Ottiene o imposta `HRESULT`, un valore numerico codificato assegnato a una specifica eccezione. Ereditato da <xref:System.Exception?displayProperty=nameWithType>.|
|`public Exception InnerException { get; }`|Ottiene l'eccezione che ha causato l'eccezione corrente. Ereditato da <xref:System.Exception?displayProperty=nameWithType>.|
|`public string Message { get; }`|Ottiene un messaggio che descrive l'eccezione corrente. Ereditato da <xref:System.TypeLoadException>.|
|`public string Source { get; set; }`|Ottiene o imposta il nome dell'applicazione o dell'oggetto che ha causato l'errore. Ereditato da <xref:System.Exception?displayProperty=nameWithType>.|
|`public string StackTrace { get; }`|Ottiene una rappresentazione di stringa dei frame immediati nello stack di chiamate. Ereditato da <xref:System.Exception?displayProperty=nameWithType>.|
|`public MethodBase TargetSite { get; }`|Ottiene il metodo che ha generato l'eccezione corrente. Ereditato da <xref:System.Exception?displayProperty=nameWithType>.|
|`public string TypeName { get; ]`|Ottiene il nome completo del tipo i cui metadati non sono presenti. Ereditato da <xref:System.TypeLoadException>.|

## <a name="methods"></a>Metodi

|Metodo|Descrizione|
|------------|-----------------|
|`public bool Equals(Object obj)`|Determina se l'oggetto specificato è uguale all'oggetto corrente.  Ereditato da <xref:System.Exception?displayProperty=nameWithType>.|
|`protected void Finalize()`|Consente a un oggetto di effettuare un tentativo di liberare risorse ed eseguire altre operazioni di pulizia prima che venga recuperato da Garbage Collection. Ereditato da <xref:System.Object>.|
|`public Exception GetBaseException()`|Restituisce l'eccezione che rappresenta la causa radice di una o più eccezioni successive. Ereditato da <xref:System.Exception?displayProperty=nameWithType>.|
|`public int GetHashCode()`|Restituisce un codice hash per un'istanza `MissingMetadataException`.   Ereditato da <xref:System.Object>.|
|`public void GetObjectData(SerializationInfo info, StreamingContext context)`|Imposta un oggetto <xref:System.Runtime.Serialization.SerializationInfo> con le informazioni relative all'eccezione.  Ereditato da <xref:System.TypeLoadException>.|
|`public Type GetType()`|Ottiene il tipo di runtime dell'istanza corrente. Ereditato da <xref:System.Exception?displayProperty=nameWithType>.|
|`protected Object MemberwiseClone()`|Crea una copia superficiale dell'oggetto corrente. Ereditato da <xref:System.Object>.|
|`public string ToString()`|Restituisce la rappresentazione di stringa dell'eccezione corrente. Ereditato da <xref:System.Exception?displayProperty=nameWithType>.|

## <a name="events"></a>Eventi

|event|Descrizione|
|-----------|-----------------|
|`protected event EventHandler<SafeSerializationEventArgs> SerializeObjectState`|Si verifica quando un'eccezione viene serializzata per creare un oggetto di stato eccezione contenente i dati serializzati relativi all'eccezione. Ereditato da <xref:System.Exception?displayProperty=nameWithType>.|

## <a name="usage-details"></a>Dettagli sull'uso

Eccezione `MissingMetadataException` generata quando la reflection viene usata per accedere ai metadati che non sono disponibili in un assembly.

I metadati disponibili in un'app al runtime sono definiti dal file di direttive di runtime (configurazione XML), *.rd.xml. Per evitare che l'app generi questa eccezione, è necessario modificare il file \*.rd.xm per definire i metadati che devono essere presenti in fase di esecuzione. Per informazioni sul formato del file \*.rd.xml, vedere [Informazioni di riferimento sul file di configurazione delle direttive di runtime (rd.xml)](../../../docs/framework/net-native/runtime-directives-rd-xml-configuration-file-reference.md).

> [!IMPORTANT]
> L'eccezione indica che i metadati richiesti dall'applicazione non sono disponibili in fase di esecuzione. Per questo motivo, l'eccezione non va gestita in un blocco `try`/`catch`. È invece necessario diagnosticare la causa dell'eccezione ed eliminarla usando un file di direttive di runtime. Per ottenere la voce che è possibile aggiungere al file di direttive di runtime che elimina l'eccezione, è possibile usare uno dei due strumenti di risoluzione dei problemi:
>
> - Lo [strumento di risoluzione dei problemi MissingMetadataException](https://dotnet.github.io/native/troubleshooter/type.html) per i tipi.
> - Lo [strumento di risoluzione dei problemi MissingMetadataException](https://dotnet.github.io/native/troubleshooter/method.html) per i metodi.

La classe `MissingMetadataException` non contiene membri univoci. Tutti i membri sono ereditati dalla relativa classe base, <xref:System.TypeAccessException>.

## <a name="see-also"></a>Vedere anche

- <xref:System.Exception?displayProperty=nameWithType>
- <xref:System.TypeAccessException>
- [Classe MissingInteropDataException](../../../docs/framework/net-native/missinginteropdataexception-class-net-native.md)
- [Classe MissingRuntimeArtifactException](../../../docs/framework/net-native/missingruntimeartifactexception-class-net-native.md)
- [Informazioni di riferimento sul file di configurazione delle direttive di runtime (rd.xml)](../../../docs/framework/net-native/runtime-directives-rd-xml-configuration-file-reference.md)
