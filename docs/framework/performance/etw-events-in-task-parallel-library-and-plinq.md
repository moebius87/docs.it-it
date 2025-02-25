---
title: Eventi ETW nella libreria TPL (Task Parallel Library) e PLINQ
ms.date: 03/30/2017
helpviewer_keywords:
- tasks, ETW events
ms.assetid: 87a9cff5-d86f-4e44-a06e-d12764d0dce2
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 85ca55e976a010a4875d260b3da30f5bc3cf2ffb
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61723615"
---
# <a name="etw-events-in-task-parallel-library-and-plinq"></a>Eventi ETW nella libreria TPL (Task Parallel Library) e PLINQ

Sia Task Parallel Library che PLINQ generano eventi Event Trace for Windows (ETW) che è possibile usare per profilare e risolvere i problemi delle applicazioni con strumenti come Windows Performance Analyzer. Tuttavia, nella maggior parte degli scenari, il modo migliore per profilare il codice dell'applicazione parallela consiste nell'usare la [Visualizzatore di concorrenza](/visualstudio/profiling/concurrency-visualizer) in Visual Studio.

## <a name="task-parallel-library-etw-events"></a>Eventi ETW di Task Parallel Library

Nella struttura EVENT_HEADER il GUID ProviderId per gli eventi generati da <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType>, <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> e <xref:System.Threading.Tasks.Parallel.Invoke%2A?displayProperty=nameWithType> è:

```
0x2e5dba47, 0xa3d2, 0x4d16, 0x8e, 0xe0, 0x66, 0x71, 0xff, 0xdc, 0xd7, 0xb5
```

### <a name="parallel-loop-begin"></a>Inizio del ciclo parallelo

EVENT_DESCRIPTOR.Task = 1

EVENT_DESCRIPTOR.Id = 1

#### <a name="user-data"></a>Dati utente

|**Name**|**Type**|**Descrizione**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|ID di TaskScheduler che ha avviato il ciclo.|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|ID dell'attività che ha avviato il ciclo.|
|ForkJoinContextID|<xref:System.Int32?displayProperty=nameWithType>|Identificatore univoco usato per indicare l'annidamento e le coppie per gli eventi con semantica fork/join.|
|OperationType|<xref:System.Int32?displayProperty=nameWithType>|Indica il tipo di ciclo:<br /><br /> 1 = ParallelInvoke<br /><br /> 2 = ParallelFor<br /><br /> 3 = ParallelForEach|
|InclusiveFrom|<xref:System.Int64?displayProperty=nameWithType>|Valore iniziale del contatore di cicli|
|ExclusiveTo|<xref:System.Int64?displayProperty=nameWithType>|Valore finale del contatore di cicli|

### <a name="parallel-loop-end"></a>Fine del ciclo parallelo
 EVENT_DESCRIPTOR.Task = 2

 EVENT_DESCRIPTOR.Id = 2

#### <a name="user-data"></a>Dati utente

|**Name**|**Type**|**Descrizione**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|ID di TaskScheduler che ha avviato il ciclo.|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|ID dell'attività che ha avviato il ciclo.|
|ForkJoinContextID|<xref:System.Int32?displayProperty=nameWithType>|Identificatore univoco usato per indicare l'annidamento e le coppie per gli eventi con semantica fork/join.|
|totalIterations|<xref:System.Int64?displayProperty=nameWithType>|Numero totale di iterazioni|

### <a name="parallel-invoke-begin"></a>Inizio della chiamata parallela
 EVENT_DESCRIPTOR.Task = 3

 EVENT_DESCRIPTOR.Id = 3

#### <a name="user-data"></a>Dati utente

|**Name**|**Type**|**Descrizione**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|ID di TaskScheduler che ha avviato il ciclo.|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|ID dell'attività che ha avviato il ciclo.|
|ForkJoinContextID|<xref:System.Int32?displayProperty=nameWithType>|Identificatore univoco usato per indicare l'annidamento e le coppie per gli eventi con semantica fork/join.|
|totalIterations|<xref:System.Int64?displayProperty=nameWithType>|Numero totale di iterazioni|
|operationType|<xref:System.Int32?displayProperty=nameWithType>|Indica il tipo di ciclo:<br /><br /> 1 = ParallelInvoke<br /><br /> 2 = ParallelFor<br /><br /> 3 = ParallelForEach|
|ActionCount|<xref:System.Int32?displayProperty=nameWithType>|Numero di azioni che verranno eseguite nella chiamata parallela.|

### <a name="parallel-invoke-end"></a>Fine della chiamata parallela
 EVENT_DESCRIPTOR.Task = 4

 EVENT_DESCRIPTOR.Id = 4

#### <a name="user-data"></a>Dati utente

|**Name**|**Type**|**Descrizione**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|ID di TaskScheduler che ha avviato il ciclo.|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|ID dell'attività che ha avviato il ciclo.|
|ForkJoinContextID|<xref:System.Int32?displayProperty=nameWithType>|Identificatore univoco usato per indicare l'annidamento e le coppie per gli eventi con semantica fork/join.|

## <a name="plinq-etw-events"></a>Eventi ETW di PLINQ
 Il GUID EVENT_HEADER.ProviderId per PLINQ è:

```
0x159eeeec, 0x4a14, 0x4418, 0xa8, 0xfe, 0xfa, 0xab, 0xcd, 0x98, 0x78, 0x87
```

### <a name="parallel-query-begin"></a>Inizio della query parallela
 EVENT_DESCRIPTOR.Task = 1

 EVENT_DESCRIPTOR.Id = 1

#### <a name="user-data"></a>Dati utente

|**Name**|**Type**|**Descrizione**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|ID di TaskScheduler che ha avviato il ciclo.|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|ID dell'attività che ha avviato il ciclo.|
|QueryID|<xref:System.Int32?displayProperty=nameWithType>|Identificatore univoco di query.|

### <a name="parallel-query-end"></a>Fine della query parallela
 EVENT_DESCRIPTOR.Task = 2

 EVENT_DESCRIPTOR.Id = 2

#### <a name="user-data"></a>Dati utente

|**Name**|**Type**|**Descrizione**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|ID di TaskScheduler che ha avviato il ciclo.|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|ID dell'attività che ha avviato il ciclo.|
|QueryID|<xref:System.Int32?displayProperty=nameWithType>|Identificatore univoco di query.|

## <a name="see-also"></a>Vedere anche

- [Eventi ETW in .NET Framework](../../../docs/framework/performance/etw-events.md)
- [Task Parallel Library (TPL)](../../../docs/standard/parallel-programming/task-parallel-library-tpl.md)
- [Parallel LINQ (PLINQ)](../../../docs/standard/parallel-programming/parallel-linq-plinq.md)
