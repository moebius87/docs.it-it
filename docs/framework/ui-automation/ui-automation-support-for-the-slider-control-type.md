---
title: Supporto per automazione interfaccia utente del tipo di controllo Slider
ms.date: 03/30/2017
helpviewer_keywords:
- control types, Slider
- UI Automation, Slider control type
- Slider control type
ms.assetid: 045ea62f-7b50-46cf-a5a9-8eb97704355f
ms.openlocfilehash: b6d44b3f855410a524875ac2a43e21cb51d68e11
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61996632"
---
# <a name="ui-automation-support-for-the-slider-control-type"></a>Supporto per automazione interfaccia utente del tipo di controllo Slider
> [!NOTE]
>  Questa documentazione è destinata agli sviluppatori di .NET Framework che vogliono usare le classi gestite di [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] definite nello spazio dei nomi <xref:System.Windows.Automation>. Per informazioni aggiornate sulle [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], vedere [Windows Automation API: Automazione interfaccia utente](https://go.microsoft.com/fwlink/?LinkID=156746).  
  
 In questo argomento vengono fornite informazioni sul supporto di [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] per il tipo di controllo Slider. In [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]un tipo di controllo è un set di condizioni che un controllo deve soddisfare per usare la proprietà <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> . Le condizioni includono linee guida specifiche per la struttura ad albero di [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , i valori delle proprietà di [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] e i tipi di controllo.  
  
 Il controllo dispositivo di scorrimento è un controllo composito con pulsanti che consentono a un utente con un mouse di impostare un intervallo numerico o effettuare una selezione da un set di elementi.  
  
 Nelle sezioni seguenti vengono definiti la struttura ad albero, le proprietà, i pattern di controllo e gli eventi di [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] per il tipo di controllo Slider. I requisiti di [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] si applicano a tutti i controlli dispositivo di scorrimento, ovvero [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)], [!INCLUDE[TLA#tla_win32](../../../includes/tlasharptla-win32-md.md)]o [!INCLUDE[TLA#tla_winforms](../../../includes/tlasharptla-winforms-md.md)].  
  
<a name="Required_UI_Automation_Tree_Structure"></a>   
## <a name="required-ui-automation-tree-structure"></a>Struttura ad albero di automazione interfaccia utente obbligatoria  
 Nella tabella seguente viene illustrata la visualizzazione controlli e la visualizzazione contenuto dell'albero di [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] relativo ai controlli dispositivo di scorrimento e viene descritto il possibile contenuto di ogni visualizzazione. Per altre informazioni sull'albero di [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , vedere [UI Automation Tree Overview](../../../docs/framework/ui-automation/ui-automation-tree-overview.md).  
  
|Visualizzazione controlli|Visualizzazione contenuto|  
|------------------|------------------|  
|Slider<br /><br /> -Button (2 o 4)<br />-Thumb (solo 1)<br />-List Item (0 o più)|Slider<br /><br /> -List Item (0 o più)|  
  
<a name="Required_UI_Automation_Properties"></a>   
## <a name="required-ui-automation-properties"></a>Proprietà di automazione interfaccia utente obbligatorie  
 La tabella seguente elenca le proprietà [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] il cui valore o la cui definizione è particolarmente rilevante per il tipo di controllo Slider. Per ulteriori informazioni sul [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] proprietà, vedere [UI Automation Properties for Clients](../../../docs/framework/ui-automation/ui-automation-properties-for-clients.md).  
  
|Proprietà di[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] |Value|Note|  
|------------------------------------------------------------------------------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|Vedere le note.|Il valore di questa proprietà deve essere univoco in tutti i controlli in un'applicazione.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|Vedere le note.|Il rettangolo più esterno che contiene l'intero controllo.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|Vedere le note.|La maggior parte dei controlli dispositivo di scorrimento deve generare l'eccezione <xref:System.Windows.Automation.NoClickablePointException> perché l'intero rettangolo di delimitazione del controllo dispositivo di scorrimento è occupato da controlli figlio.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|Vedere le note.|Se il controllo può ricevere lo stato attivo, deve supportare questa proprietà.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|Vedere le note.|Il nome del controllo di modifica viene in genere generato da un'etichetta di testo statico. Se non è presente alcuna etichetta di testo statico, un valore di proprietà per `Name` deve essere assegnato dallo sviluppatore dell'applicazione. La proprietà `Name` non deve mai includere il contenuto testuale del controllo di modifica.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|Vedere le note.|Se è presente un'etichetta di testo statico associata al controllo, questa proprietà deve esporre un riferimento a tale controllo. Se il controllo testo è un sottocomponente di un altro controllo, non avrà una proprietà `LabeledBy` impostata.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|Slider|Questo valore è uguale per tutti i framework dell' [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] .|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|"slider"|Stringa localizzata corrispondente al tipo di controllo Edit.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|Il controllo di modifica viene sempre incluso nella visualizzazione contenuto dell'albero di [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] .|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|Il controllo di modifica viene sempre incluso nella visualizzazione controlli dell'albero di [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] .|  
  
<a name="Required_UI_Automation_Control_Patterns"></a>   
## <a name="required-ui-automation-control-patterns"></a>Pattern di controllo obbligatori per l'automazione interfaccia utente  
 La tabella seguente elenca i pattern di controllo di [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] che devono essere supportati da tutti i controlli dispositivo a scorrimento. Per altre informazioni sui pattern di controllo, vedere [UI Automation Control Patterns Overview](../../../docs/framework/ui-automation/ui-automation-control-patterns-overview.md).  
  
|Pattern di controllo|Supporto|Note|  
|---------------------|-------------|-----------|  
|<xref:System.Windows.Automation.Provider.ISelectionProvider>|A seconda dei casi|Un dispositivo di scorrimento deve supportare il pattern di controllo Selection se il contenuto rappresenta un valore incluso in un set discreto di opzioni. Se il pattern di controllo Selection è supportato, la selezione corrispondente deve essere esposta come uno o più elementi di elenco figlio del dispositivo di scorrimento.|  
|<xref:System.Windows.Automation.Provider.IRangeValueProvider>|A seconda dei casi|Un dispositivo di scorrimento deve supportare il pattern di controllo RangeValue se il contenuto può essere impostato su un valore incluso in un intervallo numerico.|  
|<xref:System.Windows.Automation.Provider.IValueProvider>|A seconda dei casi|Un dispositivo di scorrimento deve supportare il pattern di controllo Value se il contenuto rappresenta un valore incluso in un set discreto di opzioni.|  
  
<a name="Required_UI_Automation_Events"></a>   
## <a name="required-ui-automation-events"></a>Eventi di automazione interfaccia utente obbligatori  
 La tabella seguente elenca gli eventi dell' [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] che devono essere supportati da tutti i controlli dispositivo di scorrimento.  
  
 Per altre informazioni sugli eventi, vedere [UI Automation Events Overview](../../../docs/framework/ui-automation/ui-automation-events-overview.md).  
  
|o[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] |Supporto|Note|  
|---------------------------------------------------------------------------------|-------------|-----------|  
|<xref:System.Windows.Automation.SelectionPatternIdentifiers.InvalidatedEvent>|A seconda dei casi|nessuno|  
|Evento di modifica della proprietà<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty> |Obbligatorio|nessuno|  
|Evento di modifica della proprietà<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty> |Obbligatorio|nessuno|  
|Evento di modifica della proprietà<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty> |Obbligatorio|nessuno|  
|Evento di modifica della proprietà<xref:System.Windows.Automation.RangeValuePatternIdentifiers.ValueProperty> |A seconda dei casi|nessuno|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|Obbligatorio|nessuno|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|Obbligatorio|nessuno|  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.Windows.Automation.ControlType.Slider>
- [Panoramica dei tipi di controllo per l'automazione interfaccia utente](../../../docs/framework/ui-automation/ui-automation-control-types-overview.md)
- [Panoramica di automazione interfaccia utente](../../../docs/framework/ui-automation/ui-automation-overview.md)
