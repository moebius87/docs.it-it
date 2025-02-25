---
ms.openlocfilehash: 65d9edc1e7377a86f8185ebf28bb5bee3a3f887d
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/18/2019
ms.locfileid: "59235560"
---
### <a name="sqlconnectionopen-fails-on-windows-7-with-non-ifs-winsock-bsp-or-lsp-present"></a>SqlConnection.Open non riesce in Windows 7 se sono presenti un provider LSP o BSP Winsock non IFS

|   |   |
|---|---|
|Dettagli|<xref:System.Data.SqlClient.SqlConnection.Open> e <xref:System.Data.SqlClient.SqlConnection.OpenAsync(System.Threading.CancellationToken)> hanno esito negativo in .NET Framework 4.5 se in esecuzione in un computer Windows 7 in cui sono presenti un provider LSP o BSP Winsock non IFS. Per determinare se è installato un provider LSP o BSP Winsock non IFS, usare il comando <code>netsh WinSock Show Catalog</code> ed esaminare ogni elemento <code>Winsock Catalog Provider Entry</code> restituito. Se per il valore Service Flags è impostato il bit <code>0x20000</code>, il provider usa handle IFS e funzionerà correttamente. Se il bit <code>0x20000</code> non è impostato, si tratta di un LSP o BSP non-IFS.|
|Suggerimento|Questo bug è stato risolto in .NET Framework 4.5.2, pertanto può essere evitato eseguendo l'aggiornamento di .NET Framework. In alternativa, è possibile evitarlo rimuovendo qualsiasi provider LSP Winsock non-IFS installato.|
|Ambito|Secondario|
|Versione|4.5|
|Tipo|Runtime|
|API interessate|<ul><li><xref:System.Data.SqlClient.SqlConnection.Open?displayProperty=nameWithType></li><li><xref:System.Data.SqlClient.SqlConnection.OpenAsync(System.Threading.CancellationToken)?displayProperty=nameWithType></li></ul>|
