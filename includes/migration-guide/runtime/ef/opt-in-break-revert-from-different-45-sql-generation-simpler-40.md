---
ms.openlocfilehash: e5d81d791e1a2f1a2dbdafc787dec1227423883d
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/18/2019
ms.locfileid: "59774260"
---
### <a name="opt-in-break-to-revert-from-different-45-sql-generation-to-simpler-40-sql-generation"></a>Interruzione del consenso esplicito per tornare alla generazione di codice SQL più semplice della versione 4.0 dalla versione 4.5

|   |   |
|---|---|
|Dettagli|Le query che producono istruzioni JSON e contengono una chiamata a un'operazione di limitazione senza prima usare OrderBy producono ora codice SQL più semplice. Dopo l'aggiornamento a .NET Framework 4.5, queste query producevano codice SQL più complesso rispetto alle versioni precedenti.|
|Suggerimento|Questa funzionalità è disabilitata per impostazione predefinita. Se Entity Framework genera istruzioni JOIN aggiuntive che causano una riduzione del livello delle prestazioni, è possibile abilitare questa funzionalità aggiungendo la voce seguente alla sezione <code>&lt;appSettings&gt;</code> del file di configurazione dell'applicazione (app.config):<pre><code class="lang-xml">&lt;add key=&quot;EntityFramework_SimplifyLimitOperations&quot; value=&quot;true&quot; /&gt;&#13;&#10;</code></pre>|
|Ambito|Trasparente|
|Versione|4.5.2|
|Tipo|Runtime|
