---
title: Esporre un provider di automazione dell'interfaccia utente lato server
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- expose a server-side UI Automation provider
- UI Automation, server-side provider, exposing
- server-side UI Automation provider, exposing
ms.assetid: 55d419c0-2201-4101-90c9-2888df4dbb47
ms.openlocfilehash: 61515c4c87638d7e2bde63f0ca9faac58128f2e5
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61609802"
---
# <a name="expose-a-server-side-ui-automation-provider"></a>Esporre un provider di automazione dell'interfaccia utente lato server
> [!NOTE]
>  Questa documentazione è destinata agli sviluppatori di .NET Framework che vogliono usare le classi gestite di [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] definite nello spazio dei nomi <xref:System.Windows.Automation>. Per informazioni aggiornate sulle [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], vedere [Windows Automation API: Automazione interfaccia utente](https://go.microsoft.com/fwlink/?LinkID=156746).  
  
 Questo argomento contiene codice di esempio che illustra come esporre un provider di automazione interfaccia utente sul lato client all'interno di una finestra <xref:System.Windows.Forms.Control?displayProperty=nameWithType> .  
  
 L'esempio sostituisce la procedura della finestra per intercettare WM_GETOBJECT, ovvero il messaggio inviato dal servizio di base di [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] quando un'applicazione client richiede informazioni sulla finestra.  
  
## <a name="example"></a>Esempio  
 [!code-csharp[UIAFragmentProvider_snip#116](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAFragmentProvider_snip/CSharp/ListFragment.cs#116)]
 [!code-vb[UIAFragmentProvider_snip#116](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAFragmentProvider_snip/VisualBasic/ListFragment.vb#116)]  
  
## <a name="see-also"></a>Vedere anche

- [Panoramica dei provider di automazione interfaccia utente](../../../docs/framework/ui-automation/ui-automation-providers-overview.md)
- [Implementazione del provider di automazione interfaccia utente lato server](../../../docs/framework/ui-automation/server-side-ui-automation-provider-implementation.md)
