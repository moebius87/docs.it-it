---
title: Memoria e intervalli
ms.date: 10/03/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- Memory<T>
- Span<T>
- buffers"
- pipeline processing
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 76a5c32660c8a08ef34c40f8f4ee9430e5ead5c8
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2019
ms.locfileid: "65644284"
---
# <a name="memory--and-span-related-types"></a>Tipi correlati alla memoria e agli intervalli

A partire da .NET Core 2.1, .NET include un numero di tipi correlati che rappresentano un'area contigua fortemente tipizzata di memoria arbitraria. Sono inclusi:

- <xref:System.Span%601?displayProperty=nameWithType>, un tipo usato per accedere a un'area contigua di memoria. Un'istanza di <xref:System.Span%601> può essere supportata da una matrice di tipo `T`, una classe <xref:System.String>, un buffer allocato con [stackalloc](~/docs/csharp/language-reference/keywords/stackalloc.md) o un puntatore alla memoria non gestita. Poiché deve essere allocato nello stack, presenta alcune restrizioni. Un campo in una classe, ad esempio, non può essere di tipo <xref:System.Span%601> e l'intervallo non può essere usato nelle operazioni asincrone.

- <xref:System.ReadOnlySpan%601?displayProperty=nameWithType>, una versione non modificabile della struttura <xref:System.Span%601>.

- <xref:System.Memory%601?displayProperty=nameWithType>, un'area contigua della memoria allocata nell'heap gestito invece che nello stack. Un'istanza di <xref:System.Memory%601> può essere supportata da una matrice di tipo `T` o da una classe <xref:System.String>. Poiché può essere archiviato nell'heap gestito, <xref:System.Memory%601> non presenta nessuna delle limitazioni di <xref:System.Span%601>.

- <xref:System.ReadOnlyMemory%601?displayProperty=nameWithType>, una versione non modificabile della struttura <xref:System.Memory%601>.

- <xref:System.Buffers.MemoryPool%601?displayProperty=nameWithType>, che alloca blocchi fortemente tipizzati di memoria da un pool di memoria a un proprietario. Le istanze di <xref:System.Buffers.IMemoryOwner%601> possono essere noleggiate dal pool chiamando <xref:System.Buffers.MemoryPool%601.Rent%2A?displayProperty=nameWithType> e restituite al pool chiamando <xref:System.Buffers.MemoryPool%601.Dispose?displayProperty=nameWithType>.

- <xref:System.Buffers.IMemoryOwner%601?displayProperty=nameWithType>, che rappresenta il proprietario di un blocco di memoria e controlla la gestione della durata.

- <xref:System.Buffers.MemoryManager%601>, una classe di base astratta che può essere usata per sostituire l'implementazione di <xref:System.Memory%601> in modo che <xref:System.Memory%601> possa essere supportato da altri tipi, ad esempio handle sicuri. <xref:System.Buffers.MemoryManager%601> è destinato a scenari avanzati.

- <xref:System.ArraySegment%601>, un wrapper per un determinato numero di elementi di matrice a partire da un determinato indice.

- <xref:System.MemoryExtensions?displayProperty=nameWithType>, una raccolta di metodi di estensione per convertire stringhe, matrici e segmenti di matrici in blocchi <xref:System.Memory%601>.

> [!NOTE]
> Per i framework precedenti, <xref:System.Span%601> e <xref:System.Memory%601> sono disponibili nel [pacchetto NuGet System.Memory](https://www.nuget.org/packages/System.Memory/).

Per altre informazioni, vedere lo spazio dei nomi <xref:System.Buffers?displayProperty=nameWithType>.

## <a name="working-with-memory-and-span"></a>Uso di memoria e intervalli

Poiché i tipi correlati alla memoria e agli intervalli vengono in genere usati per archiviare i dati in una pipeline di elaborazione, è importante che gli sviluppatori seguano una serie di procedure consigliate quando usano <xref:System.Span%601>, <xref:System.Memory%601> e i tipi correlati. Queste procedure consigliate sono documentate nelle [linee guida all'utilizzo di Memory\<T> e Span\<T>](memory-t-usage-guidelines.md).

## <a name="see-also"></a>Vedere anche

- <xref:System.Memory%601?displayProperty=nameWithType>
- <xref:System.ReadOnlyMemory%601?displayProperty=nameWithType>
- <xref:System.Span%601?displayProperty=nameWithType>
- <xref:System.ReadOnlySpan%601?displayProperty=nameWithType>
- <xref:System.Buffers?displayProperty=nameWithType>
