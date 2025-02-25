---
title: ConfigurationCodeGenerator
ms.date: 03/30/2017
ms.assetid: 3913aae8-165f-4014-9262-7fe426f90cb2
ms.openlocfilehash: a01300024f89a0a189045d80622121f7db739a39
ms.sourcegitcommit: e08b319358a8025cc6aa38737854f7bdb87183d6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/29/2019
ms.locfileid: "64912405"
---
# <a name="configurationcodegenerator"></a>ConfigurationCodeGenerator
ConfigurationCodeGenerator è un strumento che consente di esporre le implementazioni del canale personalizzate nel sistema di configurazione. In questo modo gli utenti del canale personalizzato possono configurare il canale utilizzando un file con estensione config nello stesso modo in cui configurerebbero un'associazione fornita dal sistema, ad esempio `NetTcpBinding`, o un'associazione personalizzata utilizzando `TcpTransportBindingElement`.  
  
 Quando si scrive un canale personalizzato e lo si espone al modello di programmazione utilizzando un elemento `BindingElement` o `Binding` nuovo, è necessario creare un set di classi per rendere `BindingElement` o `Binding` configurabile utilizzando un file config. È possibile utilizzare lo strumento ConfigurationCodeGenerator per generare queste classi e migliorare l'esperienza del cliente.  
  
### <a name="to-build-the-tool"></a>Per compilare lo strumento  
  
1. Per compilare la soluzione, seguire le istruzioni riportate in [Building Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
2. La compilazione della soluzione genera il file: ConfigurationCodeGenerator.exe. Il file samplerun. cmd ha una riga di comando di esempio che illustra come usare questo strumento per generare le classi per il [trasporto: UDP](../../../../docs/framework/wcf/samples/transport-udp.md) esempio.  
  
### <a name="to-run-the-tool"></a>Per eseguire lo strumento  
  
1. Al prompt dei comandi digitare quanto segue se si dispone di un tipo `BindingElement` personalizzato e un tipo `Binding` personalizzato:  
  
    ```  
    ConfigurationCodeGenerator.exe /be:YourCustomBindingElementTypeName /sb:YourCustomStdBindingTypeName /dll:TheAssemblyWhereTheseTypesAreDefined  
    ```  
  
     In alternativa, digitare quanto segue se si dispone solo di un tipo `BindingElement` personalizzato:  
  
    ```  
    ConfigurationCodeGenerator.exe /be:YourCustomBindingElementTypeName /dll: TheAssemblyWhereThisTypeIsDefined  
    ```  
  
     In alternativa, digitare quanto segue se si dispone solo di un tipo `Binding` personalizzato:  
  
    ```  
    ConfigurationCodeGenerator.exe /sb:YourCustomStdBindingTypeName /dll:TheAssemblyWhereThisTypeIsDefined  
    ```  
  
     Il comando genera tre file con estensione cs per `BindingElement` (se si specifica l'opzione /be:), cinque file cs per l'elemento `Binding` standard (se si specifica l'opzione /sb:) e un file xml.  
  
    1. Se si utilizza l'opzione /be, uno dei file cs implementa `BindingElementExtensionSection` per l'elemento di associazione. Questo codice espone `BindingElement` al sistema di configurazione, in modo che le altre associazioni personalizzate possono utilizzare l'elemento di associazione. Gli altri file dispongono di classi che rappresentano impostazioni predefinite e costanti. Nei file sono presenti i commenti `//TODO` per ricordare l'aggiornamento dei valori predefiniti.  
  
    2. Se si specifica il l'opzione /sb, due dei file cs implementano rispettivamente un `StandardBindingElement` e un `StandardBindingCollectionElement` che espongono l'associazione standard al sistema di configurazione. Gli altri file dispongono di classi che rappresentano impostazioni predefinite e costanti. Nei file sono presenti i commenti `//TODO` per ricordare l'aggiornamento dei valori predefiniti.  
  
         Se è stato specificato il /sb: opzione il file CodeToAddTo\<*YourStdBinding*>. cs include il codice che è necessario aggiungere manualmente nella classe che implementa l'associazione standard.  
  
     Il file SampleConfig.xml contiene il codice di configurazione che è necessario aggiungere al file di configurazione che registra i gestori definiti nel passaggio 1 o 2 precedente.  
