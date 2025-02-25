---
title: 'Procedura: Creare un elemento SecurityBindingElement per una modalità di autenticazione specificata'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: a7c7747a-5b8c-463f-8493-7266dac75066
ms.openlocfilehash: e35df9a5dacc5f281af48cec292a09b291312119
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61787660"
---
# <a name="how-to-create-a-securitybindingelement-for-a-specified-authentication-mode"></a>Procedura: Creare un elemento SecurityBindingElement per una modalità di autenticazione specificata
Windows Communication Foundation (WCF) sono disponibili varie modalità con cui i client e servizi l'autenticazione reciproca. È possibile creare elementi di associazione di sicurezza per tali modalità di autenticazione utilizzando metodi statici sulla classe <xref:System.ServiceModel.Channels.SecurityBindingElement> o tramite configurazione, come spiegato nell'esempio seguente.  
  
 Per altre informazioni sulle 18 modalità di autenticazione, vedere [modalità di autenticazione di SecurityBindingElement](../../../../docs/framework/wcf/feature-details/securitybindingelement-authentication-modes.md).  
  
## <a name="example"></a>Esempio  
 Nell'esempio di codice seguente sono illustrati i metodi per la creazione di associazioni per le varie modalità di autenticazione.  
  
> [!NOTE]
>  Dopo la creazione di un'istanza dell'oggetto <xref:System.ServiceModel.Channels.SecurityBindingElement>, diverse proprietà sono immutabili, ad esempio <xref:System.ServiceModel.Security.Tokens.IssuedSecurityTokenParameters.KeyType%2A> e <xref:System.ServiceModel.Channels.SecurityBindingElement.MessageSecurityVersion%2A>. Non cambiano neppure chiamando `set` su di esse.  
  
 [!code-csharp[c_CustomBindingsAuthMode#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_custombindingsauthmode/cs/source.cs#2)]
 [!code-vb[c_CustomBindingsAuthMode#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_custombindingsauthmode/vb/source.vb#2)]  
  
## <a name="see-also"></a>Vedere anche

- [Modalità di autenticazione di SecurityBindingElement](../../../../docs/framework/wcf/feature-details/securitybindingelement-authentication-modes.md)
- [Procedura: Creare un'associazione personalizzata usando SecurityBindingElement](../../../../docs/framework/wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)
