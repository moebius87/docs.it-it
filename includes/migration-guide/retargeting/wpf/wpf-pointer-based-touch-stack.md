---
ms.openlocfilehash: bb3d7c1afa9e506ffc7c6d068c48428d1e66ccb6
ms.sourcegitcommit: 4735bb7741555bcb870d7b42964d3774f4897a6e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2019
ms.locfileid: "66379437"
---
### <a name="wpf-pointer-based-touch-stack"></a>Stack per input tocco WPF basato sul puntatore

|   |   |
|---|---|
|Dettagli|Questa modifica aggiunge la possibilità di attivare uno stack facoltativo per l'input tocco/stilo WPF basato su WM_POINTER.  Gli sviluppatori che non attivano in modo esplicito questo supporto non dovrebbero riscontrare alcuna modifica nel comportamento dell'input tocco/stilo WPF. Problemi noti correnti relativi allo stack facoltativo per l'input tocco/stilo basato su WM_POINTER:<ul><li>Nessun supporto per l'input penna in tempo reale.</li><li>Anche se i plug-in dello stilo e dell'input penna continuano a funzionare, essi vengono elaborati nel thread dell'interfaccia utente, il che può comportare un peggioramento delle prestazioni.</li><li>Modifiche del comportamento dovute alla promozione da eventi di tocco/stilo agli eventi del mouse</li><li>La modifica potrebbe avere un comportamento diverso</li><li>L'opzione Trascina selezione non dà la risposta appropriata per l'input tocco</li><li>Questa operazione non influisce sull'input dello stilo</li><li>L'opzione Trascina selezione non può essere avviata per gli eventi di tocco/stilo</li><li>Questo può potenzialmente arrestare la risposta dell'applicazione fino a quando non viene rilevato l'input del mouse.</li><li>Gli sviluppatori dovranno quindi avviare l'opzione di trascinamento della selezione dagli eventi del mouse.</li></ul>|
|Suggerimento|Gli sviluppatori che desiderano abilitare questo stack possono aggiungere o unire quanto segue al file App.config dell'applicazione:<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Input.Stylus.EnablePointerSupport=true&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>Se si rimuove questo elemento o se ne imposta il valore su False si disattiva lo stack facoltativo. Si noti che lo stack è disponibile solo in Windows 10 Creators Update e versioni successive.|
|Ambito|Microsoft Edge|
|Versione|4.7|
|Tipo|Ridestinazione|
