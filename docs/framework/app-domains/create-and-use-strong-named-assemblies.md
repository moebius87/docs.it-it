---
title: Creazione e utilizzo degli assembly con nome sicuro
ms.date: 08/01/2017
helpviewer_keywords:
- strong-name bypass feature
- strong-named assemblies, about strong-named assemblies
- strong-named assemblies
- signing assemblies
- assemblies [.NET Framework], signing
- strong-named assemblies, scenarios
- assemblies [.NET Framework], strong-named
- strong-named assemblies, loading into trusted application domains
- assembly binding, strong-named
ms.assetid: ffbf6d9e-4a88-4a8a-9645-4ce0ee1ee5f9
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 629a59cd28e58e4b08805e3bb742d12e9cacbebd
ms.sourcegitcommit: 4735bb7741555bcb870d7b42964d3774f4897a6e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2019
ms.locfileid: "66377874"
---
# <a name="create-and-use-strong-named-assemblies"></a>Creare e usare gli assembly con nome sicuro

Un nome sicuro è costituito dall'identità dell'assembly, corrispondente al nome semplice in formato testo, al numero di versione e alle informazioni sulle impostazioni cultura eventualmente disponibili, nonché da una chiave pubblica e da una firma digitale. Questo nome viene generato da un file di assembly usando la chiave privata corrispondente. Il file di assembly contiene il manifesto dell'assembly, che include i nomi e gli hash di tutti i file che costituiscono l'assembly.

> [!WARNING]
> Non usare i nomi sicuri per la sicurezza, poiché forniscono solo un'identità univoca.

Gli assembly con nome sicuro possono usare solo tipi da altri assembly con nome sicuro. In caso contrario, l'integrità dell'assembly con nome sicuro risulterebbe compromessa.

## <a name="strong-name-scenario"></a>Scenario con nome sicuro

Lo scenario seguente descrive il processo di firma di un assembly con un nome sicuro per farvi successivamente riferimento in base a tale nome.

1. L'assembly A viene creato con un nome sicuro usando uno dei metodi seguenti:

    - Usando un ambiente di sviluppo che supporta la creazione di nomi sicuri, come Visual Studio.

    - Creando una coppia di chiavi crittografiche usando lo [strumento Nome sicuro (Sn.exe)](../../../docs/framework/tools/sn-exe-strong-name-tool.md) e assegnandola all'assembly mediante un compilatore della riga di comando o [Assembly Linker (Al.exe)](../../../docs/framework/tools/al-exe-assembly-linker.md). Windows Software Development Kit (SDK)

2. L'ambiente di sviluppo o lo strumento firma l'hash del file contenente il manifesto dell'assembly con la chiave privata dello sviluppatore. La firma digitale viene archiviata nel file eseguibile portabile (PE) che contiene il manifesto dell'assembly A.

3. L'assembly B è un consumer dell'assembly A. La sezione di riferimento del manifesto dell'assembly B include un token che rappresenta la chiave pubblica dell'assembly A. Un token è una parte della chiave pubblica completa e viene usato al posto della chiave stessa per risparmiare spazio.

4. Common Language Runtime verifica la firma con nome sicuro quando l'assembly viene inserito nella Global Assembly Cache. Quando si esegue l'associazione in base al nome sicuro in runtime, Common Language Runtime confronta la chiave archiviata nel manifesto dell'assembly B con la chiave usata per generare il nome sicuro per l'assembly A. Se i controlli di sicurezza di .NET Framework vengono superati e l'associazione riesce, l'assembly B ha la garanzia che i bit dell'assembly A non siano stati manomessi e che questi bit provengono in effetti dagli sviluppatori dell'assembly A.

> [!NOTE]
> Questo scenario non permette di risolvere i problemi di attendibilità. Gli assembly possono contenere firme Microsoft Authenticode complete oltre a un nome sicuro. Le firme Authenticode includono un certificato che definisce l'attendibilità. È importante tenere presente che i nomi sicuri non richiedono che il codice sia firmato in questo modo. I nomi sicuri forniscono solo un'identità univoca.

## <a name="bypass-signature-verification-of-trusted-assemblies"></a>Ignorare la verifica della firma degli assembly attendibili

A partire da [!INCLUDE[net_v35SP1_long](../../../includes/net-v35sp1-long-md.md)], le firme con nome sicuro non vengono convalidate quando un assembly viene caricato in un dominio applicazione con attendibilità totale, ad esempio il dominio applicazione predefinito per la zona `MyComputer`. Questo comportamento è reso possibile dalla funzionalità che consente di ignorare la verifica del nome sicuro. In un ambiente con attendibilità totale le richieste di <xref:System.Security.Permissions.StrongNameIdentityPermission> hanno sempre esito positivo per gli assembly con attendibilità totale firmati, indipendentemente dalla firma. La funzionalità che consente di ignorare la verifica del nome sicuro evita l'overhead superfluo della verifica delle firme con nome sicuro degli assembly con attendibilità totale in questa situazione, favorendo un caricamento più rapido degli assembly.

Questa funzionalità si applica a qualsiasi assembly firmato con un nome sicuro e che ha le caratteristiche seguenti:

- È completamente attendibile senza prova <xref:System.Security.Policy.StrongName> (ad esempio, include la prova della zona `MyComputer`).

- Viene caricato in un dominio <xref:System.AppDomain> completamente attendibile.

- Viene caricato da una località nell'ambito della proprietà <xref:System.AppDomainSetup.ApplicationBase%2A> di <xref:System.AppDomain>.

- Non ha firma ritardata.

Questa funzionalità può essere disabilitata per singole applicazioni o per un computer. Vedere [How to: Disabilitare la funzionalità che consente di ignorare il nome sicuro](../../../docs/framework/app-domains/how-to-disable-the-strong-name-bypass-feature.md).

## <a name="related-topics"></a>Argomenti correlati

|Titolo|Description|
|-----------|-----------------|
|[Procedura: Creare una coppia di chiavi pubblica/privata](../../../docs/framework/app-domains/how-to-create-a-public-private-key-pair.md)|Descrive come creare una coppia di chiavi crittografiche per la firma di un assembly.|
|[Procedura: Firmare un assembly con un nome sicuro](../../../docs/framework/app-domains/how-to-sign-an-assembly-with-a-strong-name.md)|Descrive come creare un assembly con nome sicuro.|
|[Denominazione sicura avanzata](../../../docs/framework/app-domains/enhanced-strong-naming.md)|Descrive i miglioramenti apportati ai nomi sicuri in .NET Framework 4.5.|
|[Procedura: Aggiungere un riferimento a un assembly con nome sicuro](../../../docs/framework/app-domains/how-to-reference-a-strong-named-assembly.md)|Descrive come fare riferimento a tipi o risorse in un assembly con nome sicuro in fase di compilazione o di esecuzione.|
|[Procedura: Disabilitare la funzionalità che consente di ignorare il nome sicuro](../../../docs/framework/app-domains/how-to-disable-the-strong-name-bypass-feature.md)|Descrive come disabilitare la funzionalità che consente di ignorare la convalida delle firme con nome sicuro. Questa funzionalità può essere disabilitata per tutte le applicazioni o per applicazioni specifiche.|
|[Creazione degli assembly](../../../docs/framework/app-domains/create-assemblies.md)|Offre una panoramica degli assembly a file singolo e su più file.|
|[Ritardare la firma di un assembly in Visual Studio](/visualstudio/ide/managing-assembly-and-manifest-signing#how-to-sign-an-assembly-in-visual-studio)|Descrive come firmare un assembly con un nome sicuro dopo la creazione dell'assembly.|
|[Sn.exe (strumento Nome sicuro)](../../../docs/framework/tools/sn-exe-strong-name-tool.md)|Descrive lo strumento incluso in .NET Framework che permette di creare assembly con nomi sicuri. In questo strumento sono disponibili opzioni per la gestione delle chiavi nonché per la generazione e la verifica delle firme.|
|[Al.exe (Assembly Linker)](../../../docs/framework/tools/al-exe-assembly-linker.md)|Descrive lo strumento incluso in .NET Framework che genera un file contenente un manifesto dell'assembly da moduli o file di risorse.|
