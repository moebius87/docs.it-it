---
ms.openlocfilehash: 8a1e2ca0790cb62e3c2c879f2ba0bb169ef07d77
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/18/2019
ms.locfileid: "59804259"
---
### <a name="applicationfiltermessage-no-longer-throws-for-re-entrant-implementations-of-imessagefilterprefiltermessage"></a>Application.FilterMessage non genera più un'eccezione per le implementazioni rientranti di IMessageFilter.PreFilterMessage

|   |   |
|---|---|
|Dettagli|Prima di .NET Framework 4.6.1, una chiamata a <xref:System.Windows.Forms.Application.FilterMessage(System.Windows.Forms.Message@)> con un <xref:System.Windows.Forms.IMessageFilter.PreFilterMessage(System.Windows.Forms.Message@)> che chiamava <xref:System.Windows.Forms.Application.AddMessageFilter(System.Windows.Forms.IMessageFilter)?displayProperty=name> o <xref:System.Windows.Forms.Application.RemoveMessageFilter(System.Windows.Forms.IMessageFilter)?displayProperty=name> (contemporaneamente alla chiamata a <xref:System.Windows.Forms.Application.DoEvents>) avrebbe causato una <xref:System.IndexOutOfRangeException?displayProperty=name>.<p/>A partire dalle applicazioni destinate a .NET Framework 4.6.1, questa eccezione non viene più generata e si possono usare filtri rientranti come descritto in precedenza.|
|Suggerimento|Tenere presente che <xref:System.Windows.Forms.Application.FilterMessage(System.Windows.Forms.Message@)> non genererà più un'eccezione per il comportamento <xref:System.Windows.Forms.IMessageFilter.PreFilterMessage(System.Windows.Forms.Message@)> rientrante descritto in precedenza. Questa condizione influisce solo sulle applicazioni destinate a .NET Framework 4.6.1. Le app destinate a .NET Framework 4.6.1 possono rifiutare esplicitamente questa modifica (o le app destinate a versioni precedenti possono acconsentire esplicitamente) usando l'opzione di compatibilità [DontSupportReentrantFilterMessage](~/docs/framework/migration-guide/mitigation-custom-imessagefilter-prefiltermessage-implementations.md#mitigation).|
|Ambito|Microsoft Edge|
|Versione|4.6.1|
|Tipo|Ridestinazione|
|API interessate|<ul><li><xref:System.Windows.Forms.Application.FilterMessage(System.Windows.Forms.Message@)?displayProperty=nameWithType></li></ul>|
