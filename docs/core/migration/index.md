---
title: Migrazione di .NET Core da project.json
description: Informazioni su come eseguire la migrazione di un progetto .NET Core meno recente usando project.json
ms.date: 07/19/2017
ms.custom: seodec18
ms.openlocfilehash: f48728e647b57a8c5796bdc2119f72b58a49d80f
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61663344"
---
# <a name="migrating-net-core-projects-from-projectjson"></a>Migrazione di progetti .NET Core da project.json

In questo documento vengono illustrati gli scenari di migrazione per progetti .NET Core, in particolare vengono esaminati i tre scenari di migrazione seguenti:

1. [Migrazione da uno schema valido più recente di *project.json* a *csproj*](#migration-from-projectjson-to-csproj)
2. [Migrazione da DNX a csproj](#migration-from-dnx-to-csproj)
3. [Migrazione da RC3 e progetti csproj .NET Core precedenti al formato finale](#migration-from-earlier-net-core-csproj-formats-to-rtm-csproj)

Questo documento è applicabile solo ai progetti .NET Core precedenti che usano ancora project.json. Non è applicabile per la migrazione da .NET Framework a .NET Core.

## <a name="migration-from-projectjson-to-csproj"></a>Migrazione da project.json a csproj

La migrazione dal formato con estensione *project.json* a quello con estensione *csproj* può essere eseguita mediante uno dei metodi seguenti:

- [Visual Studio 2017](#visual-studio-2017)
- [Strumento da riga di comando dotnet migrate](#dotnet-migrate)

Entrambi i metodi usano lo stesso motore sottostante per eseguire la migrazione dei progetti, pertanto i risultati saranno gli stessi. Nella maggior parte dei casi, è sufficiente usare uno dei due modi per eseguire la migrazione dal formato *project.json* a *csproj*, non è necessaria alcuna modifica manuale del file di progetto. Il file in formato *csproj* risultante verrà denominato con lo stesso nome della directory che lo contiene.

### <a name="visual-studio-2017"></a>Visual Studio 2017

Quando si apre un file con estensione *xproj* o un file di soluzione che fa riferimento a file con estensione *xproj*, viene visualizzata la finestra di dialogo **Aggiornamento unidirezionale**. La finestra di dialogo consente di visualizzare i progetti di cui eseguire la migrazione.
Se si apre un file di soluzione, verranno elencati tutti i progetti specificati nel file di soluzione. Rivedere l'elenco dei progetti di cui eseguire la migrazione e selezionare **OK**.

![Finestra di dialogo Aggiornamento unidirezionale con l'elenco di progetti di cui eseguire la migrazione](media/one-way-upgrade.jpg)

Visual Studio eseguirà la migrazione di progetti selezionati automaticamente. Durante la migrazione di una soluzione, se non si selezionano tutti i progetti, verrà visualizzata la stessa finestra di dialogo che richiederà di aggiornare i progetti rimanenti di tale soluzione. Dopo la migrazione del progetto, è possibile visualizzare e modificare il relativo contenuto facendo clic sul progetto con il pulsante destro del mouse nella finestra **Esplora soluzioni** e selezionando **Modifica \<nome progetto>.csproj**.

I file di cui è stata eseguita la migrazione (*project.json*, *global.json*, *xproj* e file di soluzione) verranno spostati nella cartella *Backup*. Il file di soluzione di cui è stata eseguita la migrazione verrà aggiornato a Visual Studio 2017 e non sarà possibile aprirlo in versioni precedenti di Visual Studio.
Viene salvato anche un file denominato *UpgradeLog.htm* che contiene un report di migrazione che viene aperto automaticamente.

> [!IMPORTANT]
> I nuovi strumenti non sono disponibili in Visual Studio 2015, pertanto non è possibile eseguire la migrazione di progetti in tale versione di Visual Studio.

### <a name="dotnet-migrate"></a>dotnet migrate

Nello scenario della riga di comando, è possibile usare il comando [`dotnet migrate`](../tools/dotnet-migrate.md). Esegue la migrazione di un progetto, di una soluzione o di un set di cartelle in tale ordine, a seconda di quale elemento è stato individuato.
Quando si esegue la migrazione di un progetto, questa riguarda il progetto e tutte le relative dipendenze.

I file di cui è stata eseguita la migrazione (*project.json*, *global.json*, e *xproj*) verranno spostati nella cartella *backup*.

> [!NOTE]
> Se si usa Visual Studio Code, il comando `dotnet migrate` non modifica i file specifici di Visual Studio Code, ad esempio `tasks.json`. Questi file devono essere modificati manualmente.
> Questo vale anche se si usa Project Ryder o qualsiasi editor o ambiente di sviluppo integrato (IDE, Integrated Development Environment) diverso da Visual Studio.

Per un confronto tra i formati project.json e csproj, vedere [Mapping tra le proprietà di project.json e csproj](../tools/project-json-to-csproj.md).

### <a name="common-issues"></a>Problemi comuni

- Se si verifica un errore: "No executable found matching command dotnet-migrate" (Non sono stati trovati eseguibili corrispondenti al comando dotnet migrate):

Eseguire `dotnet --version` per visualizzare la versione in uso. [`dotnet migrate`](../tools/dotnet-migrate.md) richiede la versione RC3 della CLI di .NET Core o versione successiva.
Questo tipo di errore viene visualizzato se si ha un file *global.json* nella directory corrente o principale e la versione `sdk` è impostata su una versione precedente.

## <a name="migration-from-dnx-to-csproj"></a>Migrazione da DNX a csproj

Se si sta ancora usando DNX per lo sviluppo di .NET Core, il processo di migrazione deve essere eseguito in due fasi:

1. Usare la [guida alla migrazione esistente DNX](from-dnx.md) per eseguire la migrazione da DNX alla CLI abilitata per project.json.
2. Seguire la procedura indicata nella sezione precedente per eseguire la migrazione da *project.json* a *csproj*.  

> [!NOTE]
> DNX è stato dichiarato ufficialmente deprecato in occasione della versione Preview 1 della CLI di .NET Core.

## <a name="migration-from-earlier-net-core-csproj-formats-to-rtm-csproj"></a>Migrazione da versioni precedenti dei formati csproj di.NET Core alla versione RTM

Il formato di file csproj di .NET Core è stato modificato e si è evoluto ad ogni versione provvisoria degli strumenti. Non esiste alcuno strumento che possa eseguire la migrazione del file di progetto da versioni precedenti del formato csproj alla versione più recente, pertanto è necessario modificare manualmente il file di progetto. I passaggi effettivi dipendono dalla versione del file di progetto di cui si esegue la migrazione. Di seguito vengono riportate alcune indicazioni da considerare in base alle modifiche avvenute tra le versioni:

* Rimuovere la proprietà della versione dall'elemento `<Project>`, se presente.
* Rimuovere lo spazio dei nomi XML (`xmlns`) dall'elemento `<Project>`.
* Se non è presente, aggiungere l'attributo `Sdk` all'elemento `<Project>` e impostarlo su `Microsoft.NET.Sdk` o su `Microsoft.NET.Sdk.Web`. Questo attributo specifica che il progetto usa l'SDK corretto. `Microsoft.NET.Sdk.Web` viene usato per le applicazioni web.
* Rimuovere le istruzioni `<Import Project="$(MSBuildExtensionsPath)\$(MSBuildToolsVersion)\Microsoft.Common.props" />` e `<Import Project="$(MSBuildToolsPath)\Microsoft.CSharp.targets" />` nella parte superiore e inferiore del progetto. Queste istruzioni di importazione sono previste dall'SDK, pertanto non sono necessarie nel progetto.
* Se sono presenti gli elementi `Microsoft.NETCore.App` o `NETStandard.Library` `<PackageReference>` nel progetto, è necessario rimuoverli. Questi riferimenti al pacchetto sono [impliciti nell'SDK](https://aka.ms/sdkimplicitrefs).
* Rimuovere l'elemento `Microsoft.NET.Sdk` `<PackageReference>`, se presente. Il riferimento all'SDK viene specificato tramite l'attributo `Sdk` dell'elemento `<Project>`.
* Rimuovere i [GLOB](https://en.wikipedia.org/wiki/Glob_(programming)) che sono [impliciti nell'SDK](../tools/csproj.md#default-compilation-includes-in-net-core-projects). Se i glob vengono lasciati nel progetto, verrà visualizzato un errore in fase di compilazione poiché gli elementi di compilazione verranno duplicati.

Dopo questi passaggi, il progetto sarà completamente compatibile con la versione RTM del formato di file csproj.

Per esempi del formato csproj prima e dopo la migrazione dalla versione precedente a quella nuova, vedere l'articolo [Updating Visual Studio 2017 RC – .NET Core Tooling improvements](https://devblogs.microsoft.com/dotnet/updating-visual-studio-2017-rc-net-core-tooling-improvements/) (Aggiornamento di Visual Studio 2017 RC - Miglioramento degli strumenti .NET Core) sul blog di .NET.

## <a name="see-also"></a>Vedere anche

- [Conversione, migrazione e aggiornamento dei progetti di Visual Studio](/visualstudio/porting/port-migrate-and-upgrade-visual-studio-projects)
