---
title: Novità di .NET Core 3.0
description: Informazioni sulle nuove funzionalità in .NET Core 3.0.
dev_langs:
- csharp
- vb
author: thraka
ms.author: adegeo
ms.date: 05/06/2019
ms.openlocfilehash: 8d6ff6bc55384281119600f2323212441c1815e9
ms.sourcegitcommit: 4c10802ad003374641a2c2373b8a92e3c88babc8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2019
ms.locfileid: "65452477"
---
# <a name="whats-new-in-net-core-30-preview-5"></a>Novità di .NET Core 3.0 (Preview 5)

Questo articolo descrive le novità di .NET Core 3.0 (Preview 5). Uno dei principali miglioramenti è il supporto per le applicazioni desktop di Windows (solo Windows). Con il componente Windows Desktop di .NET Core 3.0 SDK, è possibile convertire le applicazioni Windows Forms e WPF (Windows Presentation Foundation). Il componente Windows Desktop è dunque supportato e incluso solo in Windows. Per altre informazioni, vedere la sezione [Desktop di Windows](#windows-desktop) più avanti in questo articolo.

.NET Core 3.0 aggiunge il supporto per C# 8.0. È consigliabile usare la versione più recente di Visual Studio 2019 Update 1 Preview o Visual Studio Code con l'estensione OmniSharp.

[Scaricare e iniziare subito a usare .NET Core 3.0 Preview 5](https://aka.ms/netcore3download) in Windows, Mac e Linux.

Per altre informazioni su ogni versione di anteprima, vedere gli annunci seguenti:

- [Annuncio di .NET Core 3.0 Preview 5](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-0-preview-5/)
- [Annuncio di .NET Core 3.0 Preview 4](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-4/)
- [Annuncio di .NET Core 3.0 Preview 3](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-3/)
- [Annuncio di .NET Core 3.0 Preview 2](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-2/)
- [Annuncio di .NET Core 3.0 Preview 1](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-1-and-open-sourcing-windows-desktop-frameworks/)

## <a name="net-core-sdk-windows-installer"></a>Programma di installazione .NET Core SDK per Windows

Il programma di installazione di Windows Installer (MSI) per Windows è stato modificato a partire da .NET Core 3.0. I programmi di installazione di SDK aggiorneranno ora le versioni della banda di funzionalità sul posto. Le bande di funzionalità vengono definite nei gruppi *centinaia* nella sezione *patch* del numero di versione. Ad esempio, **3.0. * 101*** e **3.0. * 201*** sono versioni in due diverse bande di funzionalità, mentre **3.0. * 101*** e **3.0. * 199*** appartengono alla stessa banda. Quindi, quando .NET Core SDK **3.0.*101*** viene installato, .NET Core SDK **3.0.*100*** sarà rimosso dal computer se è esistente. Quando .NET Core SDK **3.0. * 200*** viene installato nello stesso computer, .NET Core SDK **3.0. * 101*** non sarà rimosso.

Per altre informazioni sul controllo delle versioni, vedere [Panoramica di come viene specificata la versione di .NET Core](../versions/index.md).

## <a name="c-80-preview"></a>Anteprima di C# 8.0

.NET core 3.0 supporta l'anteprima di C# 8.0. Per altre informazioni sulle funzionalità di C# 8.0, vedere [Novità di C# 8.0](../../csharp/whats-new/csharp-8.md).

## <a name="net-standard-21"></a>.NET Standard 2.1

Anche se .NET Core 3.0 supporta **.NET Standard 2.1**, il modello `dotnet new classlib` predefinito genera un progetto per **.NET Standard 2.0**. Per destinarlo a **.NET Standard 2.1**, modificare il file di progetto e la proprietà `TargetFramework` in `netstandard2.1`:

```xml
<Project Sdk="Microsoft.NET.Sdk">
 
  <PropertyGroup>
    <TargetFramework>netstandard2.1</TargetFramework>
  </PropertyGroup>
 
</Project>
```

Se si usa Visual Studio, è necessario Visual Studio 2019. Visual Studio 2017 non supporta **.NET Standard 2.1** o **.NET Core 3.0**. È consigliabile usare [Visual Studio 2019 Update 1 Preview](https://visualstudio.microsoft.com/vs/preview/).

## <a name="improved-net-core-version-apis"></a>Miglioramento delle API della versione .NET Core

A partire da .NET Core 3.0, le API della versione di .NET Core restituiscono le informazioni previste. Ad esempio:

```csharp
System.Console.WriteLine($"Environment.Version: {System.Environment.Version}");

// Old result
//   Environment.Version: 4.0.30319.42000
//
// New result
//   Environment.Version: 3.0.0
```

```csharp
System.Console.WriteLine($"RuntimeInformation.FrameworkDescription: {System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription}");

// Old result
//   RuntimeInformation.FrameworkDescription: .NET Core 4.6.27415.71
//
// New result
//   RuntimeInformation.FrameworkDescription: .NET Core 3.0.0-preview4-27615-11
```

> [!WARNING]
> Modifica importante: si tratta di una modifica tecnicamente importante perché è cambiato lo schema di controllo delle versioni.

## <a name="net-platform-dependent-intrinsics"></a>Intrinseci dipendenti dalla piattaforma .NET

Sono state aggiunte API che consentono l'accesso a determinate istruzioni CPU orientate alle prestazioni, ad esempio i set di **istruzioni SIMD** oppure di **istruzioni di manipolazione dei bit**. Queste istruzioni consentono di ottenere miglioramenti delle prestazioni significativi in determinati scenari, ad esempio l'elaborazione dei dati in modo efficiente in parallelo. 

Ove appropriato, le librerie .NET hanno iniziato a usare queste istruzioni per migliorare le prestazioni.

Per altre informazioni, vedere [.NET Platform Dependent Intrinsics](https://github.com/dotnet/designs/blob/master/accepted/platform-intrinsics.md) (Intrinseci dipendenti dalla piattaforma .NET).

## <a name="default-executables"></a>File eseguibili predefiniti

Per impostazione predefinita .NET Core ora compila [file eseguibili dipendenti dal framework](../deploying/index.md#framework-dependent-executables-fde). Si tratta di una novità per le applicazioni che usano una versione di .NET Core installata a livello globale. In precedenza solo le [distribuzioni autonome](../deploying/index.md#self-contained-deployments-scd) generavano un file eseguibile.

Durante `dotnet build` o `dotnet publish`, viene creato un file eseguibile che corrisponde all'ambiente e alla piattaforma dell'SDK usato. Il comportamento di questi file eseguibili è uguale a quello degli altri file eseguibili nativi, ad esempio:

* È possibile fare doppio clic sul file eseguibile.
* È possibile avviare l'applicazione direttamente da un prompt dei comandi, ad esempio `myapp.exe` in Windows e `./myapp` in Linux e macOS.

## <a name="single-file-executables"></a>File eseguibili singoli

Il comando `dotnet publish` supporta la creazione del pacchetto dell'app come file eseguibile singolo specifico per la piattaforma. Il file eseguibile è autoestraente e contiene tutte le dipendenze (incluse quelle native) necessarie per eseguire l'app. Quando l'app viene eseguita per la prima volta, l'applicazione viene estratta in una directory in base al nome dell'app e all'identificatore di compilazione. L'avvio dell'applicazione sarà più veloce alla successiva esecuzione. L'applicazione non dovrà ripetere l'estrazione una seconda volta, a meno che sia stata usata una nuova versione.

Per pubblicare un file eseguibile singolo, impostare `PublishSingleFile` nel progetto o nella riga di comando usando il comando `dotnet publish`:

```console
dotnet publish -r win10-x64 /p:PublishSingleFile=true
```

Per altre informazioni sulla pubblicazione di file singolo, vedere il [documento sulla progettazione di un bundler con file singolo](https://github.com/dotnet/designs/blob/master/accepted/single-file/design.md).

## <a name="tiered-compilation"></a>Compilazione a livelli

Per impostazione predefinita, con .NET Core 3.0 la [compilazione a livelli](https://devblogs.microsoft.com/dotnet/tiered-compilation-preview-in-net-core-2-1/) è attiva. Questa funzionalità consente al runtime di usare in modo più adattivo il compilatore JIT per ottenere prestazioni migliori.

Il vantaggio principale della compilazione a livelli (TC) consiste nell'abilitazione di metodi di ricompilazione JIT seguendo un approccio slower-but-faster (più lento ma più rapido) o higher-quality-but-slower (di migliore qualità ma più lento) per la produzione di codice. In questo modo è possibile migliorare le prestazioni di un'applicazione nelle sue vari fasi di esecuzione, dall'avvio allo stato stabile. Ciò si differenzia dall'approccio che non usa la compilazione a livelli. In questo caso ogni metodo viene compilato in un solo modo (come per il livello alta qualità), che dà priorità allo stato stabile piuttosto che alle prestazioni all'avvio.

Per abilitare la compilazione JIT rapida (codice JIT livello 0), usare questa impostazione nel file di progetto:

```xml
<PropertyGroup>
  <TieredCompilationQuickJit>true</TieredCompilationQuickJit>
</PropertyGroup>
```

Per disabilitare completamente la compilazione a livelli, usare questa impostazione nel file di progetto:

```xml
<TieredCompilation>false</TieredCompilation>
```

## <a name="build-copies-dependencies"></a>Copia delle dipendenze tramite la compilazione

Il comando `dotnet build` ora copia le dipendenze NuGet per l'applicazione dalla cache NuGet nella cartella di output per la compilazione. In precedenza, le dipendenze venivano copiate solo come parte di `dotnet publish`.

Esistono alcune operazioni, ad esempio il collegamento e la pubblicazione della pagina razor per cui sarà comunque necessaria la pubblicazione.

## <a name="local-tools"></a>Strumenti locali

.NET core 3.0 introduce gli strumenti locali. Gli strumenti locali sono simili agli [strumenti globali](../tools/global-tools.md), ma sono associati a una determinata posizione sul disco. Gli strumenti locali non sono disponibili a livello globale e vengono distribuiti come pacchetti NuGet.

> [!WARNING]
> Se è stata eseguita una prova degli strumenti locali in .NET Core 3.0 Preview 1, ad esempio eseguendo `dotnet tool restore` o `dotnet tool install`, eliminare la cartella della cache degli strumenti locali. In caso contrario, gli strumenti locali non funzioneranno in una versione più recente. Questa cartella si trova in:
>
> In macOS: Linux: `rm -r $HOME/.dotnet/toolResolverCache`
>
> In Windows: `rmdir /s %USERPROFILE%\.dotnet\toolResolverCache`

Gli strumenti locali si basano sul nome file manifesto `dotnet-tools.json` nella directory corrente. Questo file manifesto definisce gli strumenti che devono essere disponibili in tale cartella e relative sottocartelle. È possibile distribuire il file manifesto con il codice per assicurarsi che tutti gli utenti che usano il codice possano ripristinare e usare gli stessi strumenti.

Per gli strumenti sia locali che globali, è necessaria una versione compatibile del runtime. Molti strumenti attualmente presenti su NuGet.org hanno come destinazione .NET Core Runtime 2.1. Per installare questi strumenti a livello globale o locale, è comunque necessario installare il [runtime di NET Core 2.1](https://dotnet.microsoft.com/download/dotnet-core/2.1).

## <a name="major-version-roll-forward"></a>Roll forward alla versione principale

.NET core 3.0 introduce una funzionalità con consenso esplicito che consente all'app di eseguire il roll forward alla versione principale più recente di NET Core. È stata aggiunta anche una nuova impostazione per controllare come applicare il roll forward all'app. Questa funzionalità può essere configurata nei modi seguenti:

- Proprietà file di progetto: `RollForward`
- Proprietà file di configurazione runtime: `rollForward`
- Variabile di ambiente: `DOTNET_ROLL_FORWARD`
- Argomento della riga di comando: `--roll-forward`

È necessario specificare uno dei valori seguenti. Se non viene impostato un valore, l'impostazione **Minor** sarà quella predefinita.

- **LatestPatch**\
Esegue il roll forward alla versione di patch più recente. Disabilita il roll forward della versione secondaria.
- **Minor**\
Esegue il roll forward alla versione secondaria successiva minima, se la versione secondaria richiesta non esiste. Se la versione secondaria richiesta è presente, viene usato il criterio **LatestPatch**.
- **Major**\
Esegue il roll forward alla versione principale successiva minima e alla versione secondaria minima, se la versione principale richiesta non esiste. Se la versione principale richiesta è presente, viene usato il criterio **Minor**.
- **LatestMinor**\
Esegue il roll forward alla versione secondaria più recente, anche se la versione secondaria richiesta è presente. È destinata a scenari di hosting dei componenti.
- **LatestMajor**\
Esegue il roll forward alla versione principale e secondaria più recente, anche se la versione principale richiesta è presente. È destinata a scenari di hosting dei componenti.
- **Disable**\
Non esegue il roll forward. Esegue solo un'associazione alla versione specificata. Non è consigliato usare questo criterio per scopi generali poiché disabilita la possibilità di eseguire il roll forward alle patch più recenti. Questo valore è consigliato solo a scopo di test.

Ad eccezione dell'impostazione **Disable**, tutte le impostazioni useranno la versione di patch disponibile più recente.

## <a name="windows-desktop"></a>Desktop di Windows

.NET Core 3.0 supporta applicazioni desktop di Windows che usano Windows Forms e WPF (Windows Presentation Foundation). Questi framework supportano anche l'uso di controlli moderni e dello stile Fluent dalla libreria XAML dell'interfaccia utente di Windows tramite [isole XAML](/windows/uwp/xaml-platform/xaml-host-controls).

Il componente Windows Desktop fa parte di Windows .NET Core 3.0 SDK.

È possibile creare una nuova app WPF o Windows Form con i comandi `dotnet` seguenti:

```console
dotnet new wpf
dotnet new winforms
```

Visual Studio 2019 aggiunge modelli **Nuovo progetto** per .NET Core 3.0 Windows Forms e WPF.

Per altre informazioni su come convertire un'applicazione .NET Framework esistente, vedere [Convertire progetti WPF](../porting/wpf.md) e [Convertire progetti Windows Forms](../porting/winforms.md).

## <a name="com-callable-components---windows-desktop"></a>Componenti COM-Callable in Windows Desktop

In Windows è ora possibile creare componenti gestiti COM-Callable. Questa funzionalità è essenziale per usare .NET Core con modelli del componente aggiuntivo COM e anche per assicurare parità con .NET Framework.

A differenza di .NET Framework in cui *mscoree.dll* era usato come server COM, .NET Core aggiungerà un file dll di avvio nativo alla directory *bin* quando si compilerà il componente COM.

Per un esempio su come creare e usare un componente COM, vedere la [demo COM](https://github.com/dotnet/samples/tree/master/core/extensions/COMServerDemo).

## <a name="msix-deployment---windows-desktop"></a>Distribuzione MSIX in Windows Desktop

[MSIX](https://docs.microsoft.com/windows/msix/) è un nuovo formato di pacchetto di applicazioni Windows. Può essere usato per distribuire applicazioni desktop di .NET Core 3.0 in Windows 10.

Il [Progetto di creazione pacchetti di applicazione Windows](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-packaging-dot-net), disponibile in Visual Studio 2019, consente di creare pacchetti MSIX con applicazioni .NET Core [autonome](../deploying/index.md#self-contained-deployments-scd).

Il file di progetto .NET Core deve specificare i runtime supportati nella proprietà `<RuntimeIdentifiers>`:

```xml
<RuntimeIdentifiers>win-x86;win-x64</RuntimeIdentifiers>
```

## <a name="winforms-highdpi"></a>HighDPI in WinForms

Le applicazioni di Windows Forms in .NET Core possono impostare la modalità con valori DPI alti usando <xref:System.Windows.Forms.Application.SetHighDpiMode(System.Windows.Forms.HighDpiMode)?displayProperty=nameWithType>. Il metodo `SetHighDpiMode` imposta la modalità con valori DPI alti corrispondente a meno che l'impostazione sia stata configurata in altro modo, ad esempio con `App.Manifest` o P/Invoke prima di `Application.Run`.

I valori possibili per `highDpiMode`, espressi dall'enumerazione <xref:System.Windows.Forms.HighDpiMode?displayProperty=nameWithType>, sono i seguenti:

* `DpiUnaware`
* `SystemAware`
* `PerMonitor`
* `PerMonitorV2`
* `DpiUnawareGdiScaled`

Per altre informazioni sulle modalità con valori DPI alti vedere [High DPI Desktop Application Development on Windows](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows) (Sviluppo di applicazioni desktop con valori DPI alti in Windows).

### <a name="ranges-and-indices"></a>Gli intervalli e indici

Il nuovo tipo <xref:System.Index?displayProperty=nameWithType> può essere usato per l'indicizzazione. È possibile crearne uno da `int`, che esegue il conteggio dall'inizio, o con un operatore prefisso `^` (C#), che esegue il conteggio dalla fine:

```csharp
Index i1 = 3;  // number 3 from beginning
Index i2 = ^4; // number 4 from end
int[] a = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
Console.WriteLine($"{a[i1]}, {a[i2]}"); // "3, 6"
```

Esiste anche il tipo <xref:System.Range?displayProperty=nameWithType>, costituito da due valori `Index`, uno per l'inizio e uno per la fine, e può essere scritto con un'espressione intervallo `x..y` (C#). È anche possibile poi indicizzare usano un oggetto `Range`, che genera una sezione:

```csharp
var slice = a[i1..i2]; // { 3, 4, 5 }
```

Per altre informazioni, vedere l'[esercitazione su intervalli e indici](../../csharp/tutorials/ranges-indexes.md).

### <a name="async-streams"></a>Flussi asincroni

Il tipo <xref:System.Collections.Generic.IAsyncEnumerable%601> è una nuova versione asincrona di <xref:System.Collections.Generic.IEnumerable%601>. Il linguaggio consente di usare `await foreach` su `IAsyncEnumerable<T>` per utilizzarne gli elementi e di usare `yield return` per generare gli elementi.

L'esempio seguente illustra sia la produzione che l'utilizzo dei flussi asincroni. L'istruzione `foreach` è asincrona e usa `yield return` per produrre un flusso asincrono per i chiamanti. Questo modello (con `yield return`) è quello consigliato per la produzione di flussi asincroni.

```csharp
async IAsyncEnumerable<int> GetBigResultsAsync()
{
    await foreach (var result in GetResultsAsync())
    {
        if (result > 20) yield return result; 
    }
}
```

Oltre a poter usare `await foreach`, è anche possibile creare iteratori asincroni, ad esempio un iteratore che restituisce `IAsyncEnumerable/IAsyncEnumerator` in cui è possibile usare sia `await` che `yield`. Per gli oggetti che devono essere eliminati, è possibile usare `IAsyncDisposable`, implementato da diversi tipi BCL, ad esempio `Stream` e `Timer`.

Per altre informazioni, vedere l'[esercitazione sui flussi asincroni](../../csharp/tutorials/generate-consume-asynchronous-stream.md).

## <a name="ieee-floating-point-improvements"></a>Miglioramenti per le API a virgola mobile IEEE

È in corso l'aggiornamento di API virgola mobile per conformità alla [revisione IEEE 754-2008](https://en.wikipedia.org/wiki/IEEE_754-2008_revision). L'obiettivo di queste modifiche è esporre tutte le operazioni **obbligatorie** e assicurarsi che, a livello di comportamento, siano conformi alla specifica IEEE. Per altre informazioni sui miglioramenti di virgola mobile, vedere il post di blog [Floating-Point Parsing and Formatting improvements in .NET Core 3.0](https://devblogs.microsoft.com/dotnet/floating-point-parsing-and-formatting-improvements-in-net-core-3-0/) (Miglioramenti di analisi e formattazione di virgola mobile in .NET Core 3.0).

Di seguito sono riportate le correzioni per analisi e formattazione:

* Analisi corretta e arrotondamento degli input di qualsiasi lunghezza.
* Analisi corretta e formattazione dello zero negativo.
* Analisi corretta di valori `Infinity` e `NaN` con l'esecuzione di un controllo senza distinzione tra maiuscole e minuscole e consentendo un `+` precedente facoltativo, ove applicabile.

Le nuovi API <xref:System.Math?displayProperty=nameWithType> includono gli elementi seguenti:

* <xref:System.Math.BitIncrement(System.Double)> e <xref:System.Math.BitDecrement(System.Double)>\
Corrispondono alle operazioni IEEE `nextUp` e `nextDown`. Restituiscono il numero a virgola mobile più piccolo che risulta maggiore o minore rispetto all'input (rispettivamente). Ad esempio, `Math.BitIncrement(0.0)` restituirebbe `double.Epsilon`.

* <xref:System.Math.MaxMagnitude(System.Double,System.Double)> e <xref:System.Math.MinMagnitude(System.Double,System.Double)>\
Corrispondono alle operazioni IEEE `maxNumMag` e `minNumMag` e restituiscono il valore maggiore o minore in termini di grandezza dei due input (rispettivamente). Ad esempio, `Math.MaxMagnitude(2.0, -3.0)` restituirebbe `-3.0`.

* <xref:System.Math.ILogB(System.Double)>\
Corrispondono all'operazione IEEE `logB` che restituisce un valore integrale, restituisce il logaritmo in base 2 integrale del parametro di input. Questo metodo equivale in effetti a `floor(log2(x))`, ma con errori minimi di arrotondamento.

* <xref:System.Math.ScaleB(System.Double,System.Int32)>\
Corrisponde all'operazione IEEE `scaleB` che accetta un valore integrale, restituisce in effetti `x * pow(2, n)`, ma con errori minimi di arrotondamento.

* <xref:System.Math.Log2(System.Double)>\
Corrisponde all'operazione IEEE `log2` e restituisce il logaritmo in base 2. Gli errori di arrotondamento sono ridotti al minimo.

* <xref:System.Math.FusedMultiplyAdd(System.Double,System.Double,System.Double)>\
Corrisponde all'operazione IEEE `fma` ed esegue un'operazione FMA (Fused Multiply-Add). Vale a dire, esegue `(x * y) + z` come una singola operazione, riducendo così al minimo gli errori di arrotondamento. Un esempio è `FusedMultiplyAdd(1e308, 2.0, -1e308)` che restituisce `1e308`. L'operazione normale `(1e308 * 2.0) - 1e308` restituisce `double.PositiveInfinity`.

* <xref:System.Math.CopySign(System.Double,System.Double)>\
Corrisponde all'operazione IEEE `copySign` e restituisce il valore di `x`, ma con il segno di `y`.

## <a name="fast-built-in-json-support"></a>Supporto JSON predefinito rapido

Gli utenti .NET si sono ampiamente basati su [**Json.NET**](https://www.newtonsoft.com/json) e altre librerie JSON molto diffuse, che rimangono sempre scelte valide. Come tipo di dati di base, **Json.NET** usa le stringhe .NET, che in realtà sono UTF-16.

Il nuovo supporto JSON predefinito offre prestazioni elevate, bassa allocazione ed è basato su `Span<byte>`. Sono stati aggiunti tre nuovi tipi correlati principali JSON a .NET Core 3.0 nello spazio dei nomi <xref:System.Text.Json?displayProperty=nameWithType>. Questi tipi non supportano *ancora* la serializzazione e deserializzazione POCO (Plain Old CLR Object).

### <a name="utf8jsonreader"></a>Utf8JsonReader

<xref:System.Text.Json.Utf8JsonReader?displayProperty=nameWithType> è un lettore forward-only a prestazioni elevate e allocazione ridotta per il testo JSON con codifica UTF-8, letto da `ReadOnlySpan<byte>`. `Utf8JsonReader` è un tipo di base di basso livello, che può essere usato per creare parser e deserializzatori personalizzati. La lettura di un payload JSON con il nuovo `Utf8JsonReader` è due volte più veloce che con il lettore di **Json.NET**. Non viene allocato fino a quando non è necessario realizzare token JSON come stringhe (UTF-16).

Di seguito è riportato un esempio di lettura tramite il file [**launch.json**](https://github.com/dotnet/samples/blob/master/snippets/core/whats-new/whats-new-in-30/cs/launch.json) creato da Visual Studio Code:

[!CODE-csharp[Utf8JsonReader](~/samples/snippets/core/whats-new/whats-new-in-30/cs/program.cs#PrintJson)]

[!CODE-csharp[Utf8JsonReader](~/samples/snippets/core/whats-new/whats-new-in-30/cs/program.cs#PrintJsonCall)]

### <a name="utf8jsonwriter"></a>Utf8JsonWriter

<xref:System.Text.Json.Utf8JsonWriter?displayProperty=nameWithType> rende disponibile un metodo ad alte prestazioni, senza memorizzazione nella cache e forward-only per la scrittura di test JSON con codifica UTF-8 da tipi .NET comuni, come `String`, `Int32` e `DateTime`. Come il lettore, il writer è un tipo di base di basso livello, che può essere usato per creare serializzatori personalizzati. La scrittura di un payload JSON con il nuovo `Utf8JsonWriter` offre velocità maggiori del 30-80% rispetto all'uso del writer di **Json.NET** e non prevede allocazione.

### <a name="jsondocument"></a>JsonDocument

<xref:System.Text.Json.JsonDocument?displayProperty=nameWithType> è basato su `Utf8JsonReader`. `JsonDocument` offre la possibilità di analizzare i dati JSON e compilare un modello DOM (Document Object Model) di sola lettura su cui è possibile eseguire query per supportare l'accesso casuale e l'enumerazione. Gli elementi JSON che compongono i dati sono accessibili tramite il tipo <xref:System.Text.Json.JsonElement> che viene esposto da `JsonDocument` come una proprietà denominata `RootElement`. `JsonElement` contiene gli enumeratori di matrice e oggetto JSON insieme alle API per convertire il testo JSON in tipi .NET comuni. L'analisi di un payload JSON tipico e l'accesso a tutti i relativi membri tramite `JsonDocument` è 2/3 volte più veloce rispetto a **Json.NET** con allocazioni minime per dati di dimensioni ragionevoli, vale a dire inferiori a 1 MB.

Ecco un esempio di utilizzo di `JsonDocument` e `JsonElement` che può essere usato come punto di partenza:

Di seguito è riportato un esempio di lettura di C# 8.0 tramite il file [**launch.json**](https://github.com/dotnet/samples/blob/master/snippets/core/whats-new/whats-new-in-30/cs/launch.json) creato da Visual Studio Code:

[!CODE-csharp[JsonDocument](~/samples/snippets/core/whats-new/whats-new-in-30/cs/program.cs#ReadJson)]

[!CODE-csharp[JsonDocument](~/samples/snippets/core/whats-new/whats-new-in-30/cs/program.cs#ReadJsonCall)]

### <a name="jsonserializer"></a>JsonSerializer

<xref:System.Text.Json.Serialization.JsonSerializer?displayProperty=nameWithType> si basa su <xref:System.Text.Json.Utf8JsonReader> e <xref:System.Text.Json.Utf8JsonWriter> per offrire un'opzione di serializzazione veloce della memoria insufficiente quando si usano documenti e frammenti JSON.

VEDERE: https://github.com/dotnet/corefx/blob/master/src/System.Text.Json/docs/SerializerProgrammingModel.md per un esempio di conversione

Di seguito è riportato un esempio di serializzazione di un oggetto in formato JSON:

[!CODE-csharp[JsonSerializer](~/samples/snippets/core/whats-new/whats-new-in-30/cs/JSON.cs#JsonSerialize)]

Di seguito è riportato un esempio di deserializzazione di una stringa JSON in un oggetto. È possibile usare la stringa JSON generata nell'esempio precedente:

[!CODE-csharp[JsonDeserializer](~/samples/snippets/core/whats-new/whats-new-in-30/cs/JSON.cs#JsonDeserialize)]

## <a name="interop-improvements"></a>Miglioramenti di interoperabilità

.NET core 3.0 migliora l'interoperabilità di API native.

### <a name="type-nativelibrary"></a>Tipo: NativeLibrary

<xref:System.Runtime.InteropServices.NativeLibrary?displayProperty=nameWithType> offre un incapsulamento per il caricamento di una libreria nativa (usando la stessa logica di caricamento di .NET Core P/Invoke) e offre le funzioni di supporto pertinenti, ad esempio `getSymbol`. Per un esempio di codice, vedere la [demo DLLMap](https://github.com/dotnet/samples/tree/master/core/extensions/AppWithPlugin).

### <a name="windows-native-interop"></a>Interoperabilità nativa di Windows

Windows offre un'API nativa completa, sotto forma di API C semplici, COM e WinRT. Mentre .NET Core supporta **P/Invoke**, .NET Core 3.0 aggiunge la possibilità di **generare contestualmente API COM** e di **attivare API WinRT**. Per un esempio di codice, vedere la [demo Excel](https://github.com/dotnet/samples/tree/master/core/extensions/ExcelDemo).

## <a name="http2-support"></a>Supporto per HTTP/2

Il tipo <xref:System.Net.Http.HttpClient?displayProperty=nameWithType> supporta il protocollo HTTP/2. Il supporto è attualmente disabilitato, ma può essere attivato chiamando `AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2Support", true);` prima di usare <xref:System.Net.Http.HttpClient>. È anche possibile abilitare il supporto per HTTP/2 impostando la variabile di ambiente `DOTNET_SYSTEM_NET_HTTP_SOCKETSHTTPHANDLER_HTTP2SUPPORT` su `true` prima di eseguire l'app.

Se HTTP/2 è abilitato, la versione del protocollo HTTP verrà negoziata tramite TLS/ALPN e HTTP/2 verrà usato solo se il server deciderà di usarlo.

## <a name="tls-13--openssl-111-on-linux"></a>TLS 1.3 e OpenSSL 1.1.1 in Linux

.NET Core sfrutta ora il [supporto di TLS 1.3 in OpenSSL 1.1.1](https://www.openssl.org/blog/blog/2018/09/11/release111/), quando è disponibile in un determinato ambiente. Con TLS 1.3:

* La durata della connessione è migliorata grazie alla riduzione del numero di round trip necessari tra il client e il server.
* La sicurezza è migliorata grazie alla rimozione di vari algoritmi di crittografia obsoleti e non sicuri.

Quando è disponibile, .NET Core 3.0 usa **OpenSSL 1.1.1**, **OpenSSL 1.1.0** o **OpenSSL 1.0.2** in un sistema Linux. Quando **OpenSSL 1.1.1** è disponibile, entrambi i tipi <xref:System.Net.Security.SslStream?displayProperty=nameWithType> e <xref:System.Net.Http.HttpClient?displayProperty=nameWithType> useranno **TLS 1.3**, supponendo che sia il client sia il server supportino **TLS 1.3**.

>[!IMPORTANT]
>Windows e macOS non supportano ancora **TLS 1.3**. .NET Core 3.0 supporterà **TLS 1.3** in questi sistemi operativi quando il supporto sarà disponibile.

L'esempio seguente di C# 8.0 illustra .NET Core 3.0 in Ubuntu 18.10 che si connette a <https://www.cloudflare.com>:

[!CODE-csharp[TLSExample](~/samples/snippets/core/whats-new/whats-new-in-30/cs/TLS.cs#TLS)]

## <a name="cryptography-ciphers"></a>Crittografia

.NET 3.0 aggiunge supporto per le crittografie **AES-GCM** e **AES-CCM** implementate rispettivamente con <xref:System.Security.Cryptography.AesGcm?displayProperty=nameWithType> e <xref:System.Security.Cryptography.AesCcm?displayProperty=nameWithType>. Sono entrambi [algoritmi di cifratura autenticata con dati di associazione](https://en.wikipedia.org/wiki/Authenticated_encryption).

Il codice seguente illustra l'uso della crittografia `AesGcm` per crittografare e decrittografare dati casuali.

[!CODE-csharp[AesGcm](~/samples/snippets/core/whats-new/whats-new-in-30/cs/Cipher.cs#AesGcm)]

## <a name="cryptographic-key-importexport"></a>Importazione/Esportazione di chiavi crittografiche

.NET core 3.0 supporta l'importazione e l'esportazione di chiavi pubbliche e private asimmetriche da formati standard. Non è necessario usare un certificato X.509.

Tutti i tipi di chiave, ad esempio *RSA*, *DSA*, *ECDsa* e *ECDiffieHellman*, supportano i formati seguenti:

* **Chiave pubblica**
  * X.509 SubjectPublicKeyInfo

* **Chiave privata**
  * PKCS#8 PrivateKeyInfo
  * PKCS#8 EncryptedPrivateKeyInfo

Le chiavi RSA supportano anche i tipi seguenti:

* **Chiave pubblica**
  * PKCS#1 RSAPublicKey

* **Chiave privata**
  * PKCS#1 RSAPrivateKey

I metodi di esportazione generano dati binari con codifica DER e i metodi di importazione prevedono lo stesso tipo di comportamento. Se una chiave viene archiviata nel formato PEM per il testo, il chiamante dovrà applicare la decodifica Base 64 al contenuto prima di chiamare un metodo di importazione.

[!CODE-csharp[RSA](~/samples/snippets/core/whats-new/whats-new-in-30/cs/RSA.cs#Rsa)]

I file **PKCS#8** possono essere esaminati con la classe <xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo?displayProperty=nameWithType> e i file **PFX/PKCS#12** possono essere esaminati con la classe <xref:System.Security.Cryptography.Pkcs.Pkcs12Info?displayProperty=nameWithType>. I file **PFX/PKCS#12** possono essere modificati con la classe <xref:System.Security.Cryptography.Pkcs.Pkcs12Builder?displayProperty=nameWithType>.

## <a name="serialport-for-linux"></a>SerialPort per Linux

.NET Core 3.0 supporta <xref:System.IO.Ports.SerialPort?displayProperty=nameWithType> in Linux.

In precedenza, .NET Core supportava solo l'uso di `SerialPort` in Windows.

## <a name="docker-and-cgroup-memory-limits"></a>Docker e limiti di memoria cgroup

A partire da Preview 3, l'esecuzione di .NET Core 3.0 in Linux con Docker risulta migliorata usando limiti di memoria cgroup. Eseguendo un contenitore Docker con limiti di memoria, ad esempio `docker run -m`, il comportamento di .NET Core cambia.

* Dimensioni heap predefinite del Garbage Collector: massimo 20 MB o il 75% del limite di memoria nel contenitore.
* È possibile impostare dimensioni esplicite come numero assoluto o percentuale di un limite cgroup.
* Le dimensioni minime del segmento riservato per ogni heap del Garbage Collection sono pari a 16 MB. Le dimensioni riducono il numero di heap creati nei computer.

## <a name="smaller-garbage-collection-heap-sizes"></a>Riduzione delle dimensioni heap del Garbage Collector

Le dimensioni heap predefinite del Garbage Collector sono state ridotte tanto che .NET Core usa ora meno memoria. Questa modifica consente un migliore allineamento del budget di allocazione di generazione 0 con le dimensioni della cache dei processori moderni.

## <a name="garbage-collection-large-page-support"></a>Supporto per pagine grandi in Garbage Collection

Le pagine di grandi dimensioni, dette anche huge page in Linux, consentono al sistema operativo di creare aree di memoria più grandi rispetto alle dimensioni di pagina native (spesso 4K). In questo modo si migliorano le prestazioni dell'applicazione che richiede pagine di grandi dimensioni.

Il Garbage Collector può ora essere configurato con l'impostazione **GCLargePages** come funzionalità con consenso esplicito per scegliere di allocare le pagine di grandi dimensioni in Windows.

## <a name="gpio-support-for-raspberry-pi"></a>Supporto di GPIO per Raspberry Pi

Sono stati rilasciati due pacchetti NuGet che è possibile usare per la programmazione GPIO:

* [System.Device.Gpio](https://www.nuget.org/packages/System.Device.Gpio)
* [Iot.Device.Bindings](https://www.nuget.org/packages/Iot.Device.Bindings)

I pacchetti GPIO includono le API per i dispositivi *GPIO*, *SPI*, *I2C* e *PWM*. Il pacchetto di binding IoT include i binding di dispositivi. Per altre informazioni, vedere il [repository GitHub dei dispositivi](https://github.com/dotnet/iot/blob/master/src/devices/).

## <a name="arm64-linux-support"></a>Supporto ARM64 per Linux

.NET Core 3.0 aggiunge il supporto per ARM64 per Linux. Il caso d'uso principale per ARM64 è attualmente con gli scenari IoT. Per altre informazioni, vedere [.NET Core ARM64 Status](https://github.com/dotnet/announcements/issues/82) (Stato di ARM64 per .NET Core).

Sono disponibili [immagini Docker per .NET Core in ARM64](https://hub.docker.com/r/microsoft/dotnet/) per Alpine, Debian e Ubuntu.

> [!NOTE]
> Il supporto **ARM64** per Windows non è ancora disponibile.
