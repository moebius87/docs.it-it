---
title: Integrazione di WPF e WF in XAML
ms.date: 03/30/2017
ms.assetid: a4f53b48-fc90-4315-bca0-ba009562f488
ms.openlocfilehash: 2e5387186be2006c2c6c3e14e7000a21ed88140f
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2019
ms.locfileid: "65639090"
---
# <a name="wpf-and-wf-integration-in-xaml"></a>Integrazione di WPF e WF in XAML
Questo esempio viene illustrato come creare un'applicazione che usa le funzionalità di Windows Presentation Foundation (WPF) e Windows Workflow Foundation (WF) in un unico documento XAML. A tale scopo, l'esempio utilizza l'estendibilità di Windows Workflow Foundation (WF) e XAML.

## <a name="sample-details"></a>Dettagli dell'esempio
 Il file ShowWindow.xaml viene deserializzato in un'attività <xref:System.Activities.Statements.Sequence> con due variabili di stringa modificate dalle attività della sequenza: `ShowWindow` e `WriteLine`. L'attività <xref:System.Activities.Statements.WriteLine> invia come output alla finestra della console l'espressione che assegna alla proprietà <xref:System.Activities.Statements.WriteLine.Text%2A>. L'attività `ShowWindow` visualizza una finestra [!INCLUDE[avalon2](../../../../includes/avalon2-md.md)] come parte della relativa logica di esecuzione. La proprietà <xref:System.Activities.ActivityContext.DataContext%2A> della finestra include le variabili dichiarate nella sequenza. I controlli della finestra dichiarati nell'attività `ShowWindow` usano l'associazione dati per modificare tali variabili. Infine, nella finestra è contenuto un controllo pulsante. L'evento `Click` per il pulsante viene gestito da un oggetto <xref:System.Activities.ActivityDelegate> denominato `MarkupExtension` che contiene un'attività `CloseWindow`. `MarkUpExtension` richiama l'attività esterna che fornisce come contesto tutti gli oggetti identificati da un oggetto `x:Name` nonché l'oggetto <xref:System.Activities.ActivityContext.DataContext%2A> della finestra contenitore. Pertanto, l'oggetto `CloseWindow.InArgument<Window>` può essere associato usando un'espressione che fa riferimento al nome della finestra.

 L'attività `ShowWindow` deriva dalla classe <xref:System.Activities.AsyncCodeActivity%601> per visualizzare una finestra [!INCLUDE[avalon2](../../../../includes/avalon2-md.md)] e viene completata quando la finestra viene chiusa. La proprietà `Window` è di tipo `Func<Window>`, il quale consente la creazione della finestra su richiesta per ogni esecuzione dell'attività. La proprietà `Window` usa un oggetto <xref:System.Xaml.XamlDeferringLoader> per abilitare questo modello di valutazione posticipato. L'oggetto `FuncFactoryDeferringLoader` consente di acquisire un oggetto `XamlReader` durante la serializzazione e di leggerlo durante l'esecuzione di attività.

 Un'attività scritta in modo corretto non blocca mai il thread dell'utilità di pianificazione. Tuttavia, l'attività `ShowWindow` non può essere completata finché non viene chiusa la finestra in corso di visualizzazione. L'attività `ShowWindow` realizza questo comportamento derivando dall'oggetto <xref:System.Activities.AsyncCodeActivity>, chiamando il metodo <xref:System.Activities.WorkflowInvoker.BeginInvoke%2A> nel metodo <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> e visualizzando come modale la finestra. Il delegato viene richiamato tramite la proprietà <xref:System.ServiceModel.InstanceContext.SynchronizationContext%2A> di WPF. L'attività `ShowWindow` assegna la proprietà <xref:System.Activities.ActivityContext.DataContext%2A> alla proprietà `Window.DataContext` per fornire qualsiasi accesso ai controlli associati a dati alle variabili dell'ambito.

 L'ultimo punto di interesse di questo esempio è un oggetto <xref:System.Workflow.ComponentModel.Serialization.MarkupExtension> denominato `DelegateActivityExtension`. Il metodo `ProvideValue` di questa estensione di markup restituisce un delegato che richiama un'attività incorporata. Questa attività viene eseguita in un ambiente che include il contesto dati [!INCLUDE[avalon2](../../../../includes/avalon2-md.md)] e qualsiasi valore `x:Name` nell'ambito. Nel metodo `GenericInvoke`, questo ambiente viene fornito all'attività tramite un'estensione <xref:System.Activities.Hosting.SymbolResolver>. Questa estensione viene aggiunta a un oggetto <xref:System.Activities.WorkflowInvoker> che viene quindi usato per richiamare l'attività incorporata ogni volta che viene richiamato il delegato dell'estensione di markup.

> [!NOTE]
>  La finestra di progettazione predefinita non supporta l'attività ShowWindow; pertanto, il file ShowWindow.Xaml non viene visualizzato correttamente nella finestra di progettazione.

#### <a name="to-use-this-sample"></a>Per usare questo esempio

1. Con Visual Studio 2010, aprire il file della soluzione Wpfwfintegration.

2. Per compilare la soluzione, premere CTRL+MAIUSC+B.

3. Per eseguire la soluzione, premere F5.

4. Digitare il proprio nome e cognome nella finestra di dialogo.

5. Chiudere la finestra di dialogo; la console restituirà il nome.

> [!IMPORTANT]
>  È possibile che gli esempi siano già installati nel computer. Verificare la directory seguente (impostazione predefinita) prima di continuare.  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  Se questa directory non esiste, andare al [Windows Communication Foundation (WCF) e gli esempi di Windows Workflow Foundation (WF) per .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) per scaricare tutti i Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] esempi. Questo esempio si trova nella directory seguente.  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WF\Scenario\WPFWFIntegration`
