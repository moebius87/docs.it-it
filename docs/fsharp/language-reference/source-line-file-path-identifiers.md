---
title: Identificatori di riga, di file e di percorso di origine
description: Informazioni su come usare predefinito F# valori di identificatore che consentono di accedere all'origine della riga numero, directory e nome file del codice.
ms.date: 05/16/2016
ms.openlocfilehash: 4b145fe1fe20e3d7f868558e33bab26204fb0125
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61663622"
---
# <a name="source-line-file-and-path-identifiers"></a>Identificatori di riga, di file e di percorso di origine

Gli identificatori `__LINE__`, `__SOURCE_DIRECTORY__` e `__SOURCE_FILE__` sono valori predefiniti che consentono di accedere al sorgente riga numero, directory e file di nome nel codice.

## <a name="syntax"></a>Sintassi

```fsharp
__LINE__
__SOURCE_DIRECTORY__
__SOURCE_FILE__
```

## <a name="remarks"></a>Note

Ognuno di questi valori ha tipo `string`.

La tabella seguente riepiloga gli identificatori di riga, file e percorso di origine disponibili in F#. Questi identificatori non sono le macro del preprocessore; si tratta di valori predefiniti che sono riconosciuti dal compilatore.

|Identificatore predefinito|Descrizione|
|---------------------|-----------|
|`__LINE__`|Restituisce il numero di riga corrente, prendere in considerazione `#line` direttive.|
|`__SOURCE_DIRECTORY__`|Restituisce il percorso corrente completo della directory di origine, prendere in considerazione `#line` direttive.|
|`__SOURCE_FILE__`|Restituisce il nome di file di origine corrente e il relativo percorso, prendere in considerazione `#line` direttive.|

Per altre informazioni sul `#line` direttiva, vedere [direttive del compilatore](compiler-directives.md).

## <a name="example"></a>Esempio

Esempio di codice seguente viene illustrato l'utilizzo di questi valori.

[!code-fsharp[Main](../../../samples/snippets/fsharp/lang-ref-2/snippet7401.fs)]

Output:

```
Line: 4
Source Directory: C:\Users\username\Documents\Visual Studio 2017\Projects\SourceInfo\SourceInfo
Source File: C:\Users\username\Documents\Visual Studio 2017\Projects\SourceInfo\SourceInfo\Program.fs
```

## <a name="see-also"></a>Vedere anche

- [Direttive per il compilatore](compiler-directives.md)
- [Riferimenti per il linguaggio F#](index.md)
