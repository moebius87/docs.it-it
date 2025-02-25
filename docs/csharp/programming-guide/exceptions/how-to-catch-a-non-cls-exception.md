---
title: "Procedura: Rilevare un'eccezione non CLS"
ms.date: 07/20/2015
helpviewer_keywords:
- exceptions [C#], non-CLS
ms.assetid: db4630b3-5240-471a-b3a7-c7ff6ab31e8d
ms.openlocfilehash: 27b36d85b2ece957c8ef3fce70a6fd794bb3d4e2
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64595662"
---
# <a name="how-to-catch-a-non-cls-exception"></a>Procedura: Rilevare un'eccezione non CLS
Alcuni linguaggi .NET, inclusi C++ /CLI, consentono agli oggetti di generare eccezioni che non derivano da <xref:System.Exception>. Tali eccezioni sono chiamate *eccezioni non CLS* o *non eccezioni*. In C# non è possibile generare eccezioni non CLS, ma è possibile rilevarle in due modi:  
  
- All'interno di un blocco `catch (RuntimeWrappedException e)`.
  
     Per impostazione predefinita, un assembly Visual C# rileva le eccezioni non CLS come eccezioni con wrapper. Usare questo metodo se è necessario accedere all'eccezione originale, accessibile tramite la proprietà <xref:System.Runtime.CompilerServices.RuntimeWrappedException.WrappedException%2A?displayProperty=nameWithType>. La procedura illustrata più avanti in questo argomento spiega come rilevare le eccezioni in questo modo.  
  
- All'interno di un blocco catch generale, ovvero un blocco catch senza un tipo di eccezione specificato, posizionato dopo tutti i blocchi `catch`.
  
     Usare questo metodo quando si vuole eseguire un'azione, come ad esempio la scrittura in un file di log, in risposta alle eccezioni non CLS e non è necessario accedere alle informazioni dell'eccezione. Per impostazione predefinita, Common Language Runtime esegue il wrapping di tutte le eccezioni. Per disabilitare questo comportamento, aggiungere l'attributo a livello di assembly seguente al codice, in genere nel file AssemblyInfo.cs: `[assembly: RuntimeCompatibilityAttribute(WrapNonExceptionThrows = false)]`.  
  
### <a name="to-catch-a-non-cls-exception"></a>Per rilevare un'eccezione non CLS  
  
All'interno di un blocco `catch(RuntimeWrappedException e)` accedere all'eccezione originale tramite la proprietà <xref:System.Runtime.CompilerServices.RuntimeWrappedException.WrappedException%2A?displayProperty=nameWithType>.  
  
## <a name="example"></a>Esempio  
 L'esempio seguente illustra come rilevare un'eccezione non CLS generata da una libreria di classi scritta in C++/CLI. Si noti che in questo esempio il codice client C# sa in anticipo che il tipo di eccezione generato è <xref:System.String?displayProperty=nameWithType>. È possibile eseguire il cast della proprietà <xref:System.Runtime.CompilerServices.RuntimeWrappedException.WrappedException%2A?displayProperty=nameWithType> al tipo originale, a condizione che tale tipo sia accessibile dal codice.  
  
```csharp
// Class library written in C++/CLI.
var myClass = new ThrowNonCLS.Class1();

try
{
    // throws gcnew System::String(  
    // "I do not derive from System.Exception!");  
    myClass.TestThrow();
}
catch (RuntimeWrappedException e)
{
    String s = e.WrappedException as String;
    if (s != null)
    {
        Console.WriteLine(s);
    }
}
```  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.Runtime.CompilerServices.RuntimeWrappedException>
- [Eccezioni e gestione delle eccezioni](../../../csharp/programming-guide/exceptions/index.md)
