---
title: Tipi di raccolta ordinati
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- SortedDictionary collection type
- SortedList class, grouping data in collections
- grouping data in collections, SortedList collection type
- SortedList collection type
- collections [.NET Framework], SortedList collection type
ms.assetid: 3db965b2-36a6-4b12-b76e-7f074ff7275a
author: mairaw
ms.author: mairaw
ms.openlocfilehash: d2b8866d3a1922988518dfe2857c98d4725cc464
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64664564"
---
# <a name="sorted-collection-types"></a>Tipi di raccolta ordinati
La classe <xref:System.Collections.SortedList?displayProperty=nameWithType> e le classi generiche <xref:System.Collections.Generic.SortedList%602?displayProperty=nameWithType> e <xref:System.Collections.Generic.SortedDictionary%602?displayProperty=nameWithType> sono simili alla classe <xref:System.Collections.Hashtable> e alla classe generica <xref:System.Collections.Generic.Dictionary%602> in quanto implementano l'interfaccia <xref:System.Collections.IDictionary>, ma gestiscono l'ordinamento degli elementi in base alla chiave e non hanno la caratteristica di inserimento e recupero O(1) propria delle tabelle hash. Le tre classi hanno diverse funzionalità in comune:  
  
- Le tre classi implementano tutte l'interfaccia <xref:System.Collections.IDictionary?displayProperty=nameWithType>. Le due classi generiche implementano anche l'interfaccia generica <xref:System.Collections.Generic.IDictionary%602?displayProperty=nameWithType>.  
  
- Ogni elemento è una coppia chiave/valore per scopi di enumerazione.  
  
    > [!NOTE]
    >  Con l'enumerazione la classe non generica <xref:System.Collections.SortedList> restituisce oggetti <xref:System.Collections.DictionaryEntry>, anche se i due tipi generici restituiscono oggetti <xref:System.Collections.Generic.KeyValuePair%602>.  
  
- Gli elementi vengono ordinati in base a un'implementazione di <xref:System.Collections.IComparer?displayProperty=nameWithType> (per <xref:System.Collections.SortedList> non generica) o in base a un'implementazione di <xref:System.Collections.Generic.IComparer%601?displayProperty=nameWithType> (per le due classi generiche).  
  
- Ogni classe specifica le proprietà che restituiscono raccolte contenenti solo chiavi o solo valori.  
  
 La tabella seguente elenca alcune differenze tra le due classi SortedList e la classe <xref:System.Collections.Generic.SortedDictionary%602>.  
  
|Classe non generica <xref:System.Collections.SortedList> e classe generica <xref:System.Collections.Generic.SortedList%602>|Classe generica <xref:System.Collections.Generic.SortedDictionary%602>|  
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|  
|Le proprietà che restituiscono chiavi e valori vengono indicizzate, consentendo un efficiente recupero indicizzato.|Senza recupero indicizzato.|  
|Il recupero è O(log `n`).|Il recupero è O(log `n`).|  
|L'inserimento e la rimozione sono in genere O(`n`); l'inserimento è tuttavia O(log`n`) per i dati già presenti nell'ordinamento, in modo che ogni elemento venga aggiunto alla fine dell'elenco. (Ciò presuppone che non sia necessario un ridimensionamento).|L'inserimento e la rimozione sono O(log `n`).|  
|Usa meno memoria di un <xref:System.Collections.Generic.SortedDictionary%602>.|Usa più memoria della classe non generica <xref:System.Collections.SortedList> e della classe generica <xref:System.Collections.Generic.SortedList%602>.|  
  
 Per gli elenchi ordinati o i dizionari che devono essere accessibili contemporaneamente da più thread, è possibile aggiungere logica di ordinamento per una classe che deriva da <xref:System.Collections.Concurrent.ConcurrentDictionary%602>.  
  
> [!NOTE]
>  Per i valori che contengono chiavi specifiche (ad esempio, record dei dipendenti che contengono un numero di ID dipendente) è possibile creare una raccolta con chiave che include alcune caratteristiche di un elenco e alcune caratteristiche di un dizionario mediante la derivazione dalla classe generica <xref:System.Collections.ObjectModel.KeyedCollection%602>.  
  
 A partire da [!INCLUDE[net_v40_long](../../../includes/net-v40-long-md.md)], la classe <xref:System.Collections.Generic.SortedSet%601> presenta una struttura ad albero auto-bilanciata che mantiene ordinati i dati dopo inserimenti, eliminazioni e ricerche. Questa classe e la classe <xref:System.Collections.Generic.HashSet%601> implementano l'interfaccia <xref:System.Collections.Generic.ISet%601>.  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.Collections.IDictionary?displayProperty=nameWithType>
- <xref:System.Collections.Generic.IDictionary%602?displayProperty=nameWithType>
- <xref:System.Collections.Concurrent.ConcurrentDictionary%602>
- [Tipi di raccolte comunemente utilizzate](../../../docs/standard/collections/commonly-used-collection-types.md)
