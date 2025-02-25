---
title: Global Assembly Cache
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], global assembly cache
- GAC (global assembly cache)
- ACLs [.NET Framework]
- global assembly cache
- cache [.NET Framework], global assembly cache
- global assembly cache, about
- access control lists [.NET Framework]
ms.assetid: cf5eacd0-d3ec-4879-b6da-5fd5e4372202
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 2ae9470020449719ccb9760fef992898674ba696
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64593622"
---
# <a name="global-assembly-cache"></a>Global Assembly Cache
Ogni computer in cui è installato Common Language Runtime ha una cache di codice a livello di computer detta Global Assembly Cache. La Global Assembly Cache archivia gli assembly specificamente designati per essere condivisi da più applicazioni nel computer.  
  
 Per condividere gli assembly è opportuno installarli nella Global Assembly Cache solo quando è necessario. Come regola generale, mantenere le dipendenze degli assembly private e individuare gli assembly nella directory dell'applicazione, a meno che la condivisione di un assembly non venga richiesta in modo esplicito. Non è inoltre necessario installare gli assembly nella Global Assembly Cache per renderli accessibili al codice non gestito o all'interoperabilità COM.  
  
> [!NOTE]
>  Esistono scenari in cui si sceglie in modo esplicito di non installare un assembly nella Global Assembly Cache. Se si inserisce uno degli assembly che costituiscono un'applicazione nella Global Assembly Cache, non sarà più possibile replicare o installare l'applicazione usando il comando **xcopy** per copiare la directory dell'applicazione. È necessario spostare anche l'assembly nella Global Assembly Cache.  
  
 Esistono due modi per implementare un assembly nella Global Assembly Cache:  
  
- Usare un programma di installazione progettato per funzionare con la Global Assembly Cache. Questa è l'opzione preferita per l'installazione degli assembly nella Global Assembly Cache.  
  
- Usare uno strumento per sviluppatori denominato [strumento Global Assembly Cache (Gacutil.exe)](../../../docs/framework/tools/gacutil-exe-gac-tool.md), disponibile con [!INCLUDE[winsdklong](../../../includes/winsdklong-md.md)].  
  
    > [!NOTE]
    >  Ai fini della distribuzione, per installare gli assembly nella Global Assembly Cache è necessario usare Windows Installer. Usare lo strumento Global Assembly Cache solo in scenari di sviluppo, poiché non prevede il conteggio dei riferimenti di assembly né altre funzionalità disponibili con il programma di installazione di Windows.  
  
 A partire da .NET Framework 4, il percorso predefinito della Global Assembly Cache è **%windir%\Microsoft.NET\assembly**. Nelle versioni precedenti di .NET Framework, il percorso predefinito è **%windir%\assembly**.  
  
 Gli amministratori spesso proteggono la directory Systemroot usando un elenco di controllo di accesso (ACL) per controllare l'accesso in scrittura ed esecuzione. Poiché la Global Assembly Cache viene installata in una sottodirectory della directory Systemroot, eredita l'ACL di tale directory. È consigliabile che solo gli utenti con privilegi di amministratore siano autorizzati a eliminare file dalla Global Assembly Cache.  
  
 Gli assembly implementati nella Global Assembly Cache devono avere un nome sicuro. Quando si aggiunge un assembly alla Global Assembly Cache, vengono eseguiti controlli di integrità su tutti i file che compongono l'assembly. La cache esegue questi controlli di integrità per garantire che un assembly non sia stato manomesso, ad esempio quando un file è stato modificato ma il manifesto non riflette la modifica.  
  
## <a name="see-also"></a>Vedere anche

- [Assembly in Common Language Runtime](../../../docs/framework/app-domains/assemblies-in-the-common-language-runtime.md)
- [Uso di assembly e della Global Assembly Cache](../../../docs/framework/app-domains/working-with-assemblies-and-the-gac.md)
- [Assembly con nomi sicuri](../../../docs/framework/app-domains/strong-named-assemblies.md)
